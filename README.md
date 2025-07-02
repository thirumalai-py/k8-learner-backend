# learnerReportCS_backend
Nodejs, mongodb


# Docker Steps

- Build Images
`docker build -t thirumalai-b10/learn-api:latest . `

- Tag Image to the ECR
`docker tag thirumalai-b10/learn-api:latest 975050024946.dkr.ecr.ap-south-1.amazonaws.com/thirumalai-b10/learn-api:latest`

- Push Image to the ECR
`docker push 975050024946.dkr.ecr.ap-south-1.amazonaws.com/thirumalai-b10/learn-api:latest`

- Run the docker in local via ECR image

docker run -d -p 3001:3001 --name api-learn-ecr \
        -e ATLAS_URI="mongodb+srv://thirumalaipy:SL2OuQmQhJs9UwFP@cluster0.1uz02.mongodb.net/learnerReport?retryWrites=true&w=majority" \
        -e HASH_KEY="thisIsMyHashKey" \
        -e JWT_SECRET_KEY=thisIsMyJwtSecretKey \
        975050024946.dkr.ecr.ap-south-1.amazonaws.com/thirumalai-b10/learn-api:latest


- local run

docker run -d -p 3001:3001 --name api-learn \
        -e ATLAS_URI="mongodb+srv://thirumalaipy:SL2OuQmQhJs9UwFP@cluster0.1uz02.mongodb.net/learnerReport?retryWrites=true&w=majority" \
        -e HASH_KEY="thisIsMyHashKey" \
        -e JWT_SECRET_KEY=thisIsMyJwtSecretKey \
        thirumalaipy/learnapi:1.0

# Kubernetes Steps

- Create Helm Chart
`helm create learn-api`

- Run Helm Chart
cd /learn-api/

helm upgrade --install learn-api . -f values.yaml

- Run Helm as Variable

helm upgrade --install learn-api . \
-f values.yaml \
--set-string image.repository="thirumalaipy/learnapi" \
--set-string image.tag="v1" \
--set-string database_url="mongodb+srv://thirumalaipy:SL2OuQmQhJs9UwFP@cluster0.1uz02.mongodb.net/learnerReport?retryWrites=true&w=majority" \
--set-string hash_key="thisIsMyHashKeydev" \
--set-string jwt_secret_key="thisIsMyJwtSecretKeyDev" \
--set replicaCount=2


- Port Forward in local
`kubectl port-forward service/learn-api-service 3001:3001 -n lp`


- Create Cluster

eksctl create cluster \
--name thiru-cluster-1 \
--region ap-south-1 \
--node-type t2.medium \
--zones ap-south-1a,ap-south-1b


- Add Cluster to Local Kube Config

aws eks update-kubeconfig --region ap-south-1 --name thiru-cluster-1

- Set Context to thiru-cluster-1


- Jenkins Install Kubectl

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin

kubectl version --client



-- Fixes on the code
Need to add 
"0.0.0.0" on the index.js to allow working from outside pod or internet else the Loadbalancer will not work

app.listen(port, "0.0.0.0", () => {
  connect();
  console.log(`Example app listening at http://localhost:3001`);
});
