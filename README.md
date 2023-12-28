# Azure_BuildAndDeploy_on_ACA
This Azure Task allows users to easily deploy their application source to an Azure Container App by either providing a previously built image, a Dockerfile that an image can be built from, or using a builder to create a runnable application image

## Pipeline Requirements

This task requires the following parameters to be defined:

Parameters:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | :-------------: | :-------------: | ------------- |
| imageToBuild | Docker image to build | String |  |  | Required | The custom name of the image that is to be built, pushed to ACR and deployed to the Container App by this task. Note: this image name should include the ACR server; e.g., <acr-name>.azurecr.io/<repo>:<tag> |
| dockerImageTag | Docker image tag | String |  |  | Optional |  |
| azureSubscription | Azure Resource Manager connection | String |  |  | Required | Specify an Azure Resource Manager service connection for the deployment. This service connection must be linked to the user's Azure Subscription where the Container App will be created/updated. |
| resourceGroup | Azure resource group name | String |  |  | Optional | The existing resource group that the Azure Container App will be created in (or currently exists in). If not provided, this value will be in the form of <container-app-name>-rg |
| containerAppName | Azure Container App name | String |  |  | Optional | The name of the Azure Container App that will be created or updated. If not provided, this value will be in the form of ado-task-app-<build-id>-<build-number> |
| acrName | Azure Container Registry name | String |  |  | Required | The name of the Azure Container Registry that the runnable application image will be pushed to. When pushing a new image to ACR, the acrName and appSourcePath task inputs are required. |
| appSourcePath | Application source path | String | '$(System.DefaultWorkingDirectory)' |  | Required | Absolute path on the runner of the source application code to be built. If not provided, the 'imageToDeploy' argument must be provided to ensure the Container App has an image to reference. When pushing a new image to ACR, the acrName and appSourcePath task inputs are required. |
| dockerfilePath | Dockerfile path | String | 'Dockerfile' |  | Optional | Relative path (_without file prefixes) to the Dockerfile in the provided application source that should be used to build the image that is then pushed to ACR and deployed to the Container App |


These parameters provide multiple use case options for the template, enable/disable flags for the utilization of different templates as per the requirements.


## Use Cases

You can directly call a particular template as per the requirement. for example: 

  ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/Azure_BuildAndDeploy_on_ACA
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
  - template: BuildAndDeployOnContainerApp.yaml
    parameters:
      appSourcePath: ${{ parameters.appSourcePath }}
      azureSubscription: ${{ parameters.azureSubscription }}
      acrName: ${{ parameters.acrName }}
      containerAppName: ${{ parameters.containerAppName }}
      resourceGroup: ${{ parameters.resourceGroup }}
      dockerfilePath: ${{ parameters.dockerfilePath }}
      imageToBuild: ${{ parameters.imageToBuild }}
      dockerImageTag: ${{ parameters.dockerImageTag }}
        
  
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

  ```
