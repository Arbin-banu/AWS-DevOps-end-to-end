# AWS-DevOps-end-to-end
End-to-end project deployment of an application using CodeCommit, CodeArtifact, CodeBuild, CodeDeploy, CodePipeline.

Steps:

1. Create a IAM user with "CodecommitPowerUser"  and "IAMUserChangePassword" permission
2. Create a user and download the .csv file
3. Open your created user and go to security tab and scrool down and generate HTTPS Git credentials for AWS CodeCommit and save the file.
4. Create a new instance 
5. Open terminal
6. $sudo su
7. $apt-get update
8. $git clone<url from CodeCommit repo>
9. Go inside that directory
10. Enter your username and password from the .csv file which u downloaded (codecommit_credentials)
11. enter git commands:
  git status
  git add .
  git commit -m "first commit"
  git push origin master
  12. go to CodeCommit and check the file pushed
  13. Create a branch using "git checkout -b dev"
  14. create a new file and push the file in dev branch
  15. check CodeCommit and then click on Create pull request
  16. Complete the pull request and merge the branches.
  This Completes the CodeCommit Part
  
  1. Click on CodeBuild-> Create a build project
  2. Select the repo under sources
  3. For BuildSpec file, create a file in EC2 terminal. Find the file code below:
  
 vi buildspec.yml
  version: 0.2

phases:
  install:
    commands:
      - echo Installing NGINX
      - sudo apt-get update
      - sudo apt-get install nginx -y

  build:
    commands:
      - echo Build Started on $(date)
      - cp index.html /var/www/html/

  post_build:
    commands:
      - echo Configuration NGINX

artifacts:
  files:
    - /var/www/html/index.html

  
  git branch --> to know the working branch
  git checkout master--> to change the branch
  Note: If you get git error, type this commands,
   1.  git pull origin master
   2.  git config --global pull.rebase true
   3.  git pull origin master
   4.  git push origin master
  
  4. Create CodeBuild
  5. Click on Start Build
  6. After your project is built successful , then go to build projects
  7. select the project--> edit--> artifact
  
  ![image](https://user-images.githubusercontent.com/48252581/224111172-d9cb60ac-5700-4632-9fa3-19649a519458.png)

  
Follow these steps and go further.
  
  
  Here completes your CodeBuild part.
  
  Now go further for CodeDeploy part.
  
  1. Click on CodeDeploy
  2. Create a new application
  3. And select cloud platform as EC2
  4. next create a deployment group
  5. Create an IAM role with below mentioned permissions:
  
 AmazonS3FullAccess	
AmazonEC2RoleforAWSCodeDeployLimited	
AWSCodeDeployRole
AWSCodeDeployFullAccess	
AmazonEC2RoleforAWSCodeDeploy	
AmazonEC2FullAccess
  
  after creating the role, go to trust relationship tab and edit the policy and add this new policy :
  
  {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "codedeploy.amazonaws.com",
          "ec2.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
  
  and save the role
  
6. Copy the ARN of the iam role and paste it under service role in deployment group
  
7. Select amazon EC2 instance under environment configuration
8. launch an ec2 instance , and then come back to deployment group and add that ec2 instance name
  
 9. Uncheck the load balancer checkbox.
  
  Finally deployment group is succesfully created.
  
  Now you can go further.
  
  

  
  
  
