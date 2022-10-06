Use Azure Pipelines to build and publish a Node.js package
Node.js Tool Installer task

# Node.js tool installer
# Finds or downloads and caches the specified version spec of Node.js and adds it to the PATH
https://github.com/Azure-Samples/js-e2e-express-server
Edit your azure-pipelines.yml file.

trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '16.x'
  displayName: 'Install Node.js'

- script: |
    npm install
  displayName: 'npm install'

- script: |
    npm run build
  displayName: 'npm build'
  
  
  ======
  
  - task: CopyFiles@2
  inputs:
    sourceFolder: '$(Build.SourcesDirectory)'
    contents: '*.tgz' 
    targetFolder: $(Build.ArtifactStagingDirectory)/npm
  displayName: 'Copy npm package'

- task: CopyFiles@2
  inputs:
    sourceFolder: '$(Build.SourcesDirectory)'
    contents: 'package.json' 
    targetFolder: $(Build.ArtifactStagingDirectory)/npm
  displayName: 'Copy package.json'   

- task: PublishPipelineArtifact@1
  inputs:
    targetPath: '$(Build.ArtifactStagingDirectory)/npm'
    artifactName: npm
  displayName: 'Publish npm artifact'
