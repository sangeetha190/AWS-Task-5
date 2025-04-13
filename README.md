## Task Description:

### Deploy a simple web application using AWS code commit, code build and deploy & access via browser and automate via codepipeline.
- 🔹 STEP 1: Create EC2 Instance
    - Go to EC2 → Launch new instance name **CICD**
    - Choose Amazon Linux 2 AMI
    - Instance type: t2.micro (Free Tier)
    - Allow HTTP and SSH in security group
    - Connect using terminal and install Apache:
        - sudo yum update -y
        - sudo yum install -y httpd
        - sudo systemctl start httpd
        - sudo systemctl enable httpd
        - ![image](https://github.com/user-attachments/assets/ee8d0f2f-87e1-4d04-b0ea-6eaf7c9bc9e2)

- 🔹 STEP 2: Folder Structure You Need:
my-web-app/
├── index.html
├── appspec.yml
├── buildspec.yml
└── scripts/
    └── install_dependencies.sh
![image](https://github.com/user-attachments/assets/c19e854a-4c0a-46d7-83cf-d4b8b0ef5eb1)

- 📄 **File 1: index.html**
This is your actual website file.
      <!DOCTYPE html>
       <html>
             <head>
               <title>My CI/CD Web App</title>
             </head>
      <body>
          <h1>Hello, Sangeetha! 💜 This is a CodePipeline Deployment</h1>
      </body>
      </html>
- 📄 **File 2: buildspec.yml**
      - Tells CodeBuild how to build your project.
_version: 0.2
phases:
  install:
    commands:
      - echo Installing...
  build:
    commands:
      - echo Build completed
artifacts:
  files:
    - '**/*'_
- 📄 **File 3: appspec.yml**
      - Required for CodeDeploy to know where to place files.
_version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html_
- 📄 **File 4: scripts/install_dependencies.sh (Optional)**
      - You can use this if you want to install or prepare anything during deployment.
_#!/bin/bash
echo "Dependencies installed successfully"_
- 🟣 Right now it just echoes a message, but you can later install Node.js, npm, etc.

- 🧱 Step 1: ✅ Create CodeBuild Project
Go to AWS Console → CodeBuild

Click Create build project
- 🔹 STEP 2: Create CodeCommit Repo
    - Go to CodeCommit → Create repository
    - ![image](https://github.com/user-attachments/assets/ba1952ae-7d85-4402-8e64-1a749b3ef088)
    - Name it: my-web-app-repo
    - ![image](https://github.com/user-attachments/assets/ff95128d-d441-41fa-bad3-230124cfc88d)
    - Clone the repo locally or upload your files directly using Git.
    - Sample index.html:
    - <!DOCTYPE html>
        <html>
         <head><title>My Web App</title></head>
         <body><h1>Hello from CodePipeline!</h1></body>
       </html>


![image](https://github.com/user-attachments/assets/48e6aa40-2c7e-455b-a09e-12246189fa0b)
✅ Final Review Summary:
- Pipeline Name: My-repo

- Source Provider: GitHub (via GitHub App)
    - Connected to repo: sangeetha190/AWS-Task-5
    - Default branch: main
- Build Stage:
    - Just running a sample shell command (echo "Hello World") — this is fine for now.
- Deploy Stage:
    - CodeDeploy with Application: MyWebAppApp
    - Deployment Group: MyWebAppDG
    - Region: ap-south-1 ✅
    - Rollback enabled on failure
- 🎯 Yes, you can now click Create Pipeline.
- ![image](https://github.com/user-attachments/assets/c22d2f57-ba18-4b7c-9226-bb7bc70ec75b)
- ![image](https://github.com/user-attachments/assets/2f6eb14a-de68-43d5-96a2-670b7a1081ea)
- ![image](https://github.com/user-attachments/assets/ea027609-816e-4a64-8b4d-58f59a9987ac)

attach the role 
![image](https://github.com/user-attachments/assets/f3659ae9-ee9f-4136-925c-fbb80c036074)






