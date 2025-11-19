**Terraform**

**AWS Cloud Infrastructure Automation using Terraform**
This project demonstrates how to deploy AWS infrastructure automatically using Terraform.

Features:
- Launch an EC2 instance in a specified VPC and subnet
- Create an S3 bucket with versioning enabled
- Create a security group allowing SSH (port 22) and HTTP (port 80)
- Fully automated infrastructure provisioning using Terraform (Infrastructure as Code)

Technologies Used:
- AWS EC2
- AWS S3
- AWS Security Groups
- Terraform
- SSH Key Pair

# **Step-by-Step Commands You Ran**

### 1️⃣ Install Git (already done)
sudo yum install git -y

### 2️⃣ Navigate to project folder (you were already in it)
cd terraform-project

### 3️⃣ Check current files
ls -l
You should see:
main.tf  terraform.tfstate  terraform.tfstate.backup

### 4️⃣ Fix file permission (main.tf was root)
sudo chown ec2-user:ec2-user main.tf

### 5️⃣ Edit main.tf (replace with working script)
Open file:
nano main.tf
Paste this full Terraform script:

```hcl
provider "aws" {
  region = "ap-south-1"
}

# S3 Bucket
resource "aws_s3_bucket" "mybucket" {
  bucket = "tameem-terraform-bucket-20251120-01"

  tags = {
    Name = "TameemBucket"
  }
}

# Enable versioning
resource "aws_s3_bucket_versioning" "versioning" {
  bucket = aws_s3_bucket.mybucket.id
  versioning_configuration {
    status = "Enabled"
  }
}

# Security Group
resource "aws_security_group" "web_sg" {
  name        = "terraform-web-sg"
  description = "Allow SSH and HTTP"
  vpc_id      = "vpc-09f6a5d59440c6908"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# EC2 Instance
resource "aws_instance" "myweb" {
  ami           = "ami-0d176f79571d18a8f"
  instance_type = "t3.micro"
  subnet_id     = "subnet-0abbddd181a40529c"
  key_name      = "mywebserver"

  vpc_security_group_ids = [
    aws_security_group.web_sg.id
  ]

  tags = {
    Name = "Terraform-EC2"
  }
}

Save and exit:
CTRL + O
Enter
CTRL + X

### 6️⃣ Initialize Terraform
terraform init

### 7️⃣ Check plan
terraform plan

### 8️⃣ Apply Terraform
terraform apply
Type: yes

✅ This created:
* S3 bucket `tameem-terraform-bucket-20251120-01`
* Enabled versioning
* EC2 instance `i-XXXXXXXXXXXX`
* Security group `terraform-web-sg`

### 9️⃣ Verify in AWS Console
* EC2 → Running instance
* S3 → Bucket created and versioning enabled
* Security Group → Rules for SSH and HTTP applied
<img width="1919" height="830" alt="image" src="https://github.com/user-attachments/assets/75926397-779a-4de0-a816-d8d3277d1ddc" />
<img width="1917" height="827" alt="image" src="https://github.com/user-attachments/assets/3d2561cd-4dbc-4463-8dbc-3daba5f32650" />
<img width="784" height="514" alt="image" src="https://github.com/user-attachments/assets/e597954b-21dc-420a-b9a2-f119c0138f29" />
<img width="829" height="310" alt="image" src="https://github.com/user-attachments/assets/87e77919-8c36-4052-9fc0-ea6b65565122" />





Do you want me to do that next?

