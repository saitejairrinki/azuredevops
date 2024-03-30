# Automating Docker Build and Push Pipeline with Azure DevOps 🚀

In modern software development, Docker has become a cornerstone for building, shipping, and running applications consistently. Automating the Docker build and push process is crucial for streamlining the deployment workflow. In this blog post, we'll explore how to set up a Docker build and push pipeline using Azure DevOps.

## Introduction to Pipeline

Azure DevOps provides robust CI/CD capabilities, allowing developers to automate the software delivery process. By defining pipelines as code, teams can ensure consistency, reliability, and repeatability in their deployment workflows.

## Pipeline Configuration

Let's dive into the YAML code for our Docker build and push pipeline:

```yaml
# Define the trigger for the pipeline to run when changes are pushed to the 'main' branch
trigger:
  - main

# Define the VM image to use for the build agent
pool:
  vmImage: ubuntu-latest

# Define the stages of the pipeline
stages:
  # Stage for development environment
  - stage: Dev
    variables:
      # Define variable group for generic Dev environment
      - group: DevEnvironment
    displayName: 'Dev'
    jobs:
      # Define a deployment job for the Dev stage
      - deployment: Dev
        displayName: 'CI/CD Stage for Generic Sample image'
        environment: DevEnvironment
        strategy:
          runOnce:
            deploy:
              template: your-template.yml@template-repo  # Execute the specified template from the template repository

  # Stage for higher environments (Higher_Env)
  - stage: HigherEnv
    variables:
      # Define variable group for generic Higher_Env environments
      - group: HigherEnvVariables
    displayName: 'Higher_Env'
    jobs:
      # Define a deployment job for the Higher_Env stage
      - deployment: Higher_Env
        displayName: 'CI/CD Stage for Generic Sample image'
        environment: Higher_Env
        strategy:
          runOnce:
            deploy:
              template: your-template.yml@template-repo  # Execute the specified template from the template repository
```

### Code Level Comments 🛠️
- **trigger**: Defines when the pipeline should run, triggered on changes to the 'main' branch.
- **pool**: Specifies the virtual machine image to use for the build agent.
- **stages**: Defines stages for the pipeline, including 'Dev' and 'HigherEnv' for different environments.
- **Dev Stage**: Builds and pushes Docker images for the development environment.
- **HigherEnv Stage**: Handles Docker image deployment for higher environments.

## Template File for Docker Task

Here's the template file containing instructions for building and pushing Docker images:

```yaml
steps:
- task: Docker@2
  displayName: 'Build & Push Docker image'
  inputs:
    repository: $(GenericSampleImage)
    command: 'buildAndPush'
    Dockerfile: 'Dockerfile'
    containerRegistry: $(ACRServiceConnection)
    tags: |
      $(Build.BuildId)
      latest
```

### Code Level Comments 🛠️
- **steps**: Specifies tasks to be executed in the pipeline.
- **Docker Task**: Utilizes Docker@2 task to build and push Docker images.
- **Inputs**: Defines parameters such as repository, Dockerfile path, container registry, and tags.
- **Tags**: Includes versioning tags for Docker images.

## Conclusion

Automating Docker build and push pipelines with Azure DevOps enhances efficiency, consistency, and reliability in software delivery. By leveraging pipelines as code, teams can streamline the deployment process and focus more on building great software.

Start automating your Docker workflows today and experience the benefits of CI/CD in your development journey! 🚀
