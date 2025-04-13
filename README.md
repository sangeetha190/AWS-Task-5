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
