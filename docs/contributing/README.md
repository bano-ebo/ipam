# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Running an IPAM Development Environment with Docker Compose
We have included a Docker Compose file in the root directory of the project (`docker-compose.yml`), to quickly build a fully functional Azure IPAM development environment. The Docker Compose file is also dependant on an `env` file to correctly pass all of the required environment variables into the containers. You can use the `env.example` file, also found at the root directory of the project, as a template to create your own `env` file. 

To start a development environment of the Azure IPAM solution via Docker Compose, run the following commands from the root directory of the project:

```shell
# Build the Container Images
docker compose build --no-cache

# Start IPAM Development Environment
docker compose up --force-recreate

# Stop & Remove Containers when Finished
docker compose rm -s -v -f
```

## Building Production Containers Images and Pushing them to DockerHub
We user Dockerfiles to build the containers for each of the Azure IPAM supporting components including the Engine, UI, and Load Balancer. If you choose, you can build these containers yourself and host them in DockerHub.

To do so, run the following Docker commands from the root directory of the project:

```shell
# Engine Container
docker build --rm --no-cache -t <Repository Name>/ipam-engine:latest -f ./engine/Dockerfile.deb ./engine
docker push <Repository Name>/ipam-engine:latest

# Function Container
docker build --rm --no-cache -t <Repository Name>/ipam-func:latest -f ./engine/Dockerfile.func ./engine
docker push <Repository Name>/ipam-func:latest

# UI Container
docker build --rm --no-cache -t <Repository Name>/ipam-ui:latest -f ./ui/Dockerfile.deb ./ui
docker push <Repository Name>/ipam-ui:latest

# Load Balancer Container
docker build --rm --no-cache -t <Repository Name>/ipam-lb:latest -f ./lb/Dockerfile ./lb
docker push <Repository Name>/ipam-lb:latest
```

## Building Production Containers Images Using a Private ACR
In addition to the DockerHub option (above), alternatively you may choose to leverage an Azure Container Registry to host your Azure IPAM containers. Also, you may have selected the `-PrivateACR` flag during the deployment of your Azure IPAM environment, and from time to time you will need to update your containers as new code is released.

To do so, run the following Azure CLI commands from the root directory of the project:

```shell
# Authenicate to Azure CLI
az login

# Set Target Azure Subscription
az account set --subscription "<Target Subscription Name/GUID>"

# Engine Container
az acr build -r <ACR Name> -t ipam-engine:latest -f ./engine/Dockerfile.deb ./engine

# Function Container
az acr build -r <ACR Name> -t ipam-func:latest -f ./engine/Dockerfile.func ./engine

# UI Container
az acr build -r <ACR Name> -t ipam-ui:latest -f ./ui/Dockerfile.deb ./ui

# Load Balancer Container
az acr build -r <ACR Name> -t ipam-lb:latest -f ./lb/Dockerfile ./lb
```
