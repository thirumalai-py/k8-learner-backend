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

cd /learn-api/

helm upgrade --install learn-api . -f values.yaml

`kubectl port-forward service/learn-api-service 3001:3001 -n lp`