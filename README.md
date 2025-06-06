## Task Description:

### Deploy a simple web application using AWS code commit, code build and deploy & access via browser and automate via codepipeline.
- 🔹 STEP 1: Create EC2 Instance
    - Go to EC2 → Launch new instance name **CICD**
    - Choose Amazon Linux 2 AMI
    - Instance type: t2.micro (Free Tier)
    - Allow HTTP and SSH in security group
        - ![image](https://github.com/user-attachments/assets/ee8d0f2f-87e1-4d04-b0ea-6eaf7c9bc9e2)

- 🔹 STEP 2: Folder Structure You Need:
my-web-app/
├── index.html
├── appspec.yml
├── buildspec.yml
└── scripts/
    └── install_dependencies.sh

- 📄 **File 1: index.html**
This is your actual website file.
      ![image](https://github.com/user-attachments/assets/4010af06-9fd6-4773-af34-66756cc2019b)

- 📄 **File 2: buildspec.yml**
       - Tells CodeBuild how to build your project.
       - ![image](https://github.com/user-attachments/assets/e1283659-7507-4d41-84ca-5fb79f9d68cb)

- 📄 **File 3: appspec.yml**
       - ![image](https://github.com/user-attachments/assets/a6f5ac5b-f8aa-4f76-b78e-038405c594ac)

- 📄 **File 4: scripts/install_dependencies.sh (Optional)**
    - You can use this if you want to install or prepare anything during deployment.
    -  ![image](https://github.com/user-attachments/assets/e135d39f-4137-498b-a3a8-027038b2646b)
    -  ![image](https://github.com/user-attachments/assets/c19e854a-4c0a-46d7-83cf-d4b8b0ef5eb1)

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
![image](https://github.com/user-attachments/assets/02e5ae7d-72ce-422b-bdc6-c6f3cdeb4213)

![image](https://github.com/user-attachments/assets/f3659ae9-ee9f-4136-925c-fbb80c036074)

-🔹**STEP 4: Create CodeCommit Repository**
    - 1. Go to Developer Tools → CodeCommit → Create Repository
    - 2. Name: my-web-app-repo
  ![image](https://github.com/user-attachments/assets/ba1952ae-7d85-4402-8e64-1a749b3ef088)
  ![image](https://github.com/user-attachments/assets/ff95128d-d441-41fa-bad3-230124cfc88d)

-🔹 **STEP 5: Create CodeDeploy Application & Deployment Group**
    - Go to CodeDeploy → Create application
      - Name: MyWebAppApp
      - Platform: EC2/On-premise

  ![image](https://github.com/user-attachments/assets/48e6aa40-2c7e-455b-a09e-12246189fa0b)
  
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
       
       
![Screenshot 2025-04-13 173931](https://github.com/user-attachments/assets/b60028d6-7031-418d-9c70-5d2861ba77ae)

       
- 🔹 **STEP 8: Final Output Check**
      - Once you push changes to the CodeCommit repo:
      - Pipeline will trigger automatically
      - Files will be built and deployed to /var/www/html on EC2
      - Visit: http://3.110.90.253/
      - You will see your deployed index.html page! 🎉

 ![image](https://github.com/user-attachments/assets/4925ba31-48fe-4151-8c9d-cc02155be0c2)
    
- Output

- ![image](https://github.com/user-attachments/assets/adc524ea-a42e-41fb-9954-1986b67b16af)

