# AWS-CodePipeline-Project1
AWS CodePipeline with AWS CodeCommit, AWS CodeBuild, AWS CodeDeploy


AWS IAM steps:

        1. create IAM user

        2. give permission to IAM user to acess AWS CodeCommit, AWS CodeBuild, AWS Code Deploy

        3. create service role for CodeDeploy and give necessary permission to role 

        4. create role for EC2 to give permissions to access S3, CodeDeploy, EC2Full accesss   


AWS CodeCommit steps:

        1. create AWS CodeCommit repository

        2. Create IAM user and give permission to access AWS CodeCommit

        3. Clone repository using Clone HTTPS on local machine

        4. Use any code edior and give credential to access repository(I used VS Code)

        5. Add index.html 

        6. Add this file to repository using git commands

                git add .

                git commit -m "file added"

                git push origin master

        7. refesh AWS Code Commit repository and see file added in repo.


Amazon S3 steps:

1. create a S3 bucket to store Build Artifacts


AWS CodeBuild steps:

1. create a build project
  
     - give project name

     - source provider-AWS CodeCommit

     - select repository name

     - select branch-master

     - environment type-managed image-Ubantu

     - runtime-standard

     - image-latest

     - create new service role

     - Buildspec file-add buildspecfile 

     - store artifats in Amazon S3

     - Start build

2. After successful build artifacts stored in Amazon S3


AWS CodeDeploy steps:


1 Create application

   1. Create Application
   
   2. give compute name as EC2
   
   3. Click create application

2 Create Deployment group

  1. Give name to deployment group
  
  2. Service role--give ARN of service linked role 
  
  3. Launch EC2 instance
  
  4. Environment Configuration as Amazon EC2-give name and instance value
  
  5. Set up CodeDeploy agent on EC2
  
     Connect EC2 instance.
    
     vim install.sh
    
        #!/bin/bash 
        
        # This installs the CodeDeploy agent and its prerequisites on Ubuntu 22.04.  
        
        sudo apt-get update 
        
        sudo apt-get install ruby-full ruby-webrick wget -y 
        
        cd /tmp 
        
        wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/releases/codedeploy-agent_1.3.2-1902_all.deb 
        
        mkdir codedeploy-agent_1.3.2-1902_ubuntu22 
        
        dpkg-deb -R codedeploy-agent_1.3.2-1902_all.deb codedeploy-agent_1.3.2-1902_ubuntu22 
        
        sed 's/Depends:.*/Depends:ruby3.0/' -i ./codedeploy-agent_1.3.2-1902_ubuntu22/DEBIAN/control 
        
        dpkg-deb -b codedeploy-agent_1.3.2-1902_ubuntu22/ 
        
        sudo dpkg -i codedeploy-agent_1.3.2-1902_ubuntu22.deb 
        
        systemctl list-units --type=service | grep codedeploy 
        
        sudo service codedeploy-agent status
       
       :wq
       
     bash install.sh
   
  7. create deplyment group
    
    sudo sevice codeceply-agent restatrt
    
    sudo service codedeploy-agent satatus
  
     If status is up and running restart deployment

  8. create appspec.yml file 
  
  9. push code to repository
  
  10.Build project
  
  11. Retry deplyment
  
  
create pipepline steps:

Pipeline Structure

When this application is deployed, it will create an AWS CodePipeline pipeline that has up to the following 3 stages:

Source: This stage is the entry point of the pipeline. It is triggered when you push a change to the specified CodeCommit repository branch.

Build: This stage builds the project using AWS CodeBuild.

Deploy (optional): This stage deploys the project using CloudFormation. This stage will only be created if you provide the DeployRoleName parameter when setting up this application. See the "Parameters" section below.

