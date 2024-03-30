# Multistage Pipeline Using Template from Another Repository 🚀

```yaml
# Pipeline to run a template from another repository 🚀

# Define the repository containing the template 📁
resources:
  repositories:
  - repository: template-repo
    name: org/template-repo
    type: git
    ref: main

# Define the trigger and agent pool 🎯
trigger:
- main
pool:
  vmImage: ubuntu-latest

# Define pipeline stages 🚀
stages:
  - stage: Dev
    displayName: Development Stage
    jobs:
    - deployment: Dev
      displayName: 'CI/CD Stage for Generic Sample image'
      environment: DevEnvironment
      strategy:
        runOnce:
          deploy:
            steps:
            # Checkout the current repository and the template repository 🔄
            - checkout: self  # Checkout the current repository
            - checkout: template-repo  # Checkout the template repository
            
            # Run the template for Dev stage 🛠️
            - template: your-template.yml@template-repo  # Execute the specified template from the template repository

  - stage: Staging
    displayName: Staging Stage
    jobs:
    - deployment: Staging
      displayName: 'CI/CD Stage for Staging Environment'
      environment: StagingEnvironment
      strategy:
        runOnce:
          deploy:
            steps:
            # Checkout the current repository and the template repository 🔄
            - checkout: self  # Checkout the current repository
            - checkout: template-repo  # Checkout the template repository
            
            # Run the template for Staging stage 🛠️
            - template: your-template.yml@template-repo  # Execute the specified template from the template repository
```

This pipeline YAML includes environments for both the Development and Staging stages. The `Dev` stage is associated with the `DevEnvironment`, and the `Staging` stage is associated with the `StagingEnvironment`. Each deployment job within the stage runs the specified template from the template repository.
