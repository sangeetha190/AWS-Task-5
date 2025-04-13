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
