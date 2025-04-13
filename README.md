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


- 🔹**STEP 3: Create IAM Role for EC2**
     - Go to IAM → Roles → Create role
     - Select EC2 as trusted entity
       - Attach these policies:
       - AmazonEC2FullAccess
       - AmazonSSMFullAccess
       - AmazonEC2RoleforAWSCodeDeploy
       - AWSCodeDeployFullAccess
       - AmazonSSMManagedInstanceCore
- Name it: EC2CodeDeployRole
  - Attach this role to your EC2 instance:
  - EC2 → Actions → Security → Modify IAM Role → Select EC2CodeDeployRole

-🔹**STEP 4: Create CodeCommit Repository**
    - Go to Developer Tools → CodeCommit → Create Repository
      - Name: my-web-app

-🔹 **STEP 5: Create CodeDeploy Application & Deployment Group**
     - Go to CodeDeploy → Create application
       - Name: MyWebAppApp
       - Platform: EC2/On-premise
     - Create Deployment Group:
        - Name: MyWebAppDG
        - Environment: EC2
        - Tag: Name = CICD (should match EC2 tag)
        - Service role: EC2CodeDeployRole
        - Deployment type: In-place
        - Deployment config: CodeDeployDefault.AllAtOnce
        - ❌ Uncheck Load Balancer

- 🔹**STEP 6: Create CodeBuild Project**
       - Go to CodeBuild → Create project
       - Name: MyWebAppBuild
       - Source provider: AWS CodeCommit → Select my-web-app
       - Environment: Managed image (Amazon Linux 2)
       - Buildspec: Use buildspec.yml from repo
       - IAM Role: Create or select one with full access to CodeBuild

-🔹**STEP 7: Create CodePipeline**
    - Go to CodePipeline → Create pipeline
    - Pipeline name: MyWebAppPipeline
    - **Source:**
       - Provider: AWS CodeCommit
       - Repo: my-web-app
       - Branch: main
    - **Build:**
       - Provider: AWS CodeBuild
       - Project: MyWebAppBuild
    - **Deploy:**
       - Provider: AWS CodeDeploy
       - Application: MyWebAppApp
       - Deployment Group: MyWebAppDG
       
- 🔹 **STEP 8: Final Output Check**
      - Once you push changes to the CodeCommit repo:
        - Pipeline will trigger automatically
        - Files will be built and deployed to /var/www/html on EC2
      - Visit: http://3.110.90.253/
      - You will see your deployed index.html page! 🎉

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

![image](https://github.com/user-attachments/assets/ba227dc1-971d-475b-88b6-abd963c72893)

attach the role 
![image](https://github.com/user-attachments/assets/f3659ae9-ee9f-4136-925c-fbb80c036074)
![image](https://github.com/user-attachments/assets/2b2f2ed7-46ad-4f82-b646-3753f68ad9a1)

![image](https://github.com/user-attachments/assets/6ea3aef4-4277-4edf-89a3-1baa78c9bb5b)



![image](https://github.com/user-attachments/assets/4925ba31-48fe-4151-8c9d-cc02155be0c2)

Output
![image](https://github.com/user-attachments/assets/adc524ea-a42e-41fb-9954-1986b67b16af)


