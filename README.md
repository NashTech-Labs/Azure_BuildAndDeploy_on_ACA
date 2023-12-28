# Azure_BuildAndDeploy_on_ACA
This Azure Task allows users to easily deploy their application source to an Azure Container App by either providing a previously built image, a Dockerfile that an image can be built from, or using a builder to create a runnable application image

## Pipeline Requirements

This task requires the following parameters to be defined:

Parameters:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | :-------------: | :-------------: | ------------- |
| imageToBuild |  | String |  |  | Required |  |
| dockerImageTag |  | String |  |  | Required |  |
| azureSubscription |  | String |  |  | Required |  |
| resourceGroup |  | String |  |  | Required |  |
| containerAppName |  | String |  |  | Required |  |
| acrName |  | String |  |  | Required |  |
| appSourcePath |  | String | '$(System.DefaultWorkingDirectory)' |  | Required |  |
| dockerfilePath |  | String | 'Dockerfile' |  | Required |  |


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
