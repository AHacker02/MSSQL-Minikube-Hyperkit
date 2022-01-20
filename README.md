# Setup MSSQL server on minikube and hyperkkit


## Installation

- Uninstall docker from system

- Run this to install all the required installations.
    > brew install minikube hyperkit docker kubectl

> *Note*:
>
> "brew install docker" will only install docker-cli which is free to use
>
> "brew install docker --cask" will install docker daemon and other components which are not free


## Setup MSSQL

- Start minikube
    > minikube start --driver=hyperkit --container-runtime=docker

- Mount local folder to persist DB data
    > minikube mount (Local path):/mssql

- Setup docker-cli to pull docker images
    > eval $(minikube docker-env)

- Create persistant volume
    > kubectl apply -f PersistenVolume.yaml

- Create SQL server password
    > kubectl create secret generic mssql --from-literal=SA_PASSWORD="YourStrongPassword"

- Create MSSQL deployment
    > kubectl apply -f MSSQL.yaml

    > *Note*: This take some time to pull the image from docker hub. You can check the status using "kubectl logs  deployment/mssql-deployment"

- Create load balancer
    > kubectl apply -f LoadBalancer.yaml

- Get Server URL 
    > minikube service mssql-deployment --url


## Tips

- Use "minikube delete" to cleanup resources. This will delete VM and all database files.
- Use "minikube pause" to pause the resources without cleaning the VM.
- Use "minikube unpause" to resume the VM.
- Use "minikube dashboard --url" to get a web dashboard
