# Getting started guide

## Tools and Technologies
* Azure Container registry (Free tier)
* Azure DevOps (Free tier)
* Github 
* Docker
* Dotnet 5.0 SDK

## Prerequisites
1. Access to ACR from Azure DevOps project.
2. Create service connections from Azure DevOps project to Github and ACR.
3. Disable direct push to main branch of the repository.

## Directory structure
**1. Base directory**
```
.
├── build-pipeline.yaml
├── docker
├── INSTRUCTIONS.md
├── LICENSE
├── README.md
└── templates
```
**2. Directory contains source code and docker file**
```
docker
├── DevOpsChallenge.SalesApi.sln
├── Dockerfile
├── src
└── tests
```
**3. Directory contains template files for Azure DevOps pipeline**
```
templates
├── junit.tpl
├── trivy-docker-scan.yaml
└── variables.yaml 
```

## Steps to follow
### Test the app locally
1. Run SQL in a docker container.
```
sudo docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<your_password>" -p 1433:1433 --name devops-challenge --hostname devops_challenge -d mcr.microsoft.com/mssql/server:2022-latest
```
2. Go to the dotnet project directory and install the dependencies and, build the code.
```
dotnet restore
dotnet build --no-restore -o sample
```
3. Run the application in output directory.
```
dotnet ./sample/DevOpsChallenge.SalesApi.dll
```
4. Run the test suite
```
dotnet test --verbosity normal
```

### Automate using the CI Pipeline
1. Pipeline definition can be found in `build-pipeline.yaml` in root directory.
2. Create a new pipeline with Azure DevOPs. 
   - Select Github from connect step.
   - Select the relevant repository from the Select step.
   - Select  the option `Existing Azure Pipeline YAML file` from configure step. And provide the branch and the pipeline definition YAML file from the menu.
   - Run the  pipeline

3. When doing a new code change, create a new branch from the main branch do the changes and create a pull request to main branch.


## Reference
1. https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-linux-ver15&preserve-view=true&pivots=cs1-bash#pullandrun2019
2. https://docs.docker.com/language/dotnet/build-images/
3. https://docs.docker.com/language/dotnet/run-containers/
4. https://learn.microsoft.com/en-us/azure/devops/pipelines/ecosystems/containers/acr-template?view=azure-devops
5. https://learn.microsoft.com/en-us/azure/devops/pipelines/ecosystems/dotnet-core?view=azure-devops&tabs=dotnetfive
6. https://lgulliver.github.io/trivy-scan-results-to-azure-devops/