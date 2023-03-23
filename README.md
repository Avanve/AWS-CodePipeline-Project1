# AWS-CodePipeline-Project1
AWS CodePipeline with AWS CodeCommit, AWS CodeBuild, AWS CodeDeploy


Pipeline Structure

When this application is deployed, it will create an AWS CodePipeline pipeline that has up to the following 5 stages:

Source: This stage is the entry point of the pipeline. It is triggered when you push a change to the specified Git repository or CodeCommit repository branch.
Build: This stage builds the project using AWS CodeBuild.
Test (optional): This stage runs the integration tests of the project using CodeBuild. This stage will only be created if you provide the IntegTestRoleName parameter when setting up this module. See the "Parameters" section below.
Deploy (optional): This stage deploys the project using CloudFormation. This stage will only be created if you provide the DeployRoleName parameter when setting up this application. See the "Parameters" section below.
Publish (optional): This stage publishes the project to AWS Serverless Application Repository using the publish app. This stage will only be created if you pass 'true' to the PublishToSAR parameter when setting up this module. See the "Parameters" section below.

Installation

Create an AWS account if you do not already have one and login
If your source code repository is on GitHub, then create a GitHub OAuth token (see instructions below).
Go to this app's page on the Serverless Application Repository and click "Deploy"
Provide the required app parameters and click "Deploy"
