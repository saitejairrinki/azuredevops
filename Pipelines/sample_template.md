# Running Pipeline Using Template from Another Repository 🚀

**Introduction:**

In the realm of continuous integration and deployment, Azure Pipelines simplifies automation by allowing code reuse through templates. This guide demonstrates how templates facilitate seamless integration and execution of pipelines stored in different repositories, enabling streamlined workflows with ease.

**Pipeline Configuration**:

```yaml
# Pipeline to run a template from another repository

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
  - stage: ExecuteTemplate
    displayName: Execute Pipeline Template
    jobs:
    - job: 
      steps:
      # Checkout the current repository and the template repository 🔄
      - checkout: self  # Checkout the current repository
      - checkout: template-repo  # Checkout the template repository
      
      # Run the template 🛠️
      - template: your-template.yml@template-repo  # Execute the specified template from the template repository

```

**Understanding the Pipeline**:

- **Resources**: The `resources` section defines the repository containing the template. This ensures Azure Pipelines can access the template when required.

- **Trigger and Agent Pool**: The pipeline triggers on changes to the `main` branch and utilizes an Ubuntu-based agent pool.

- **Stages**: We have a single stage named `ExecuteTemplate`, dedicated to executing the pipeline template. Within this stage, we define a single job.

- **Job Steps**:
  - We checkout both the current repository and the repository containing the template.
  - We use the `template` directive to execute the specified template from the other repository.


### Sample Template file to print "Hello World" `your-template.yml`
```yaml
steps:
- task: PowerShell@2
  displayName: 'Print Hello World'
  inputs:
    targetType: 'inline'
    script: |
      Write-Host "Hello World"
```

**Conclusion**:

Templates in Azure Pipelines enable easy code reuse, simplifying pipeline builds and deployments. With templates, changes can be made in a single location, ensuring consistency and efficiency across workflows. 🔄🚀
