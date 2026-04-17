# Cloud Fundamentals & Why Terraform Matters

---

## 1. Overview

This section builds a strong foundation before working with cloud tools.

### Learning Objectives

- Understand cloud computing fundamentals  
- Learn why companies migrated from traditional infrastructure  
- Explore cloud service models & deployment models  
- Understand key cloud characteristics  
- Learn why Terraform is essential in this ecosystem  

> **Key Insight:**  
Terraform is ineffective without understanding the infrastructure it manages.

---

## 2. Core Concepts

### 2.1 What is Cloud Computing?

Cloud computing is the **on-demand delivery of computing resources** (servers, storage, networking, databases) over the internet.

---

### 2.2 Traditional vs Cloud

| Traditional (On-Prem) | Cloud |
|----------------------|-------|
| Buy servers upfront | Pay-as-you-go |
| Manual setup | Automated provisioning |
| Slow scaling | Instant scaling |
| High capital cost | Operational cost |

---

### 2.3 Why Cloud?

#### Problems with Traditional Infrastructure

- Hardware procurement delays (weeks/months)  
- Over-provisioning (wasted resources)  
- Under-provisioning (downtime risk)  
- High maintenance overhead  

#### Cloud Solutions

- Elasticity (scale anytime)  
- High availability  
- Global accessibility  
- Cost efficiency  

---

### 2.4 Cloud Service Models

#### 1. IaaS (Infrastructure as a Service)

- You manage: OS, applications, runtime  
- Cloud provides: VM, storage, networking  
- Example: AWS EC2  

> DevOps relevance: Most Terraform work starts here

---

#### 2. PaaS (Platform as a Service)

- You manage: Application  
- Cloud manages: OS, runtime, infrastructure  
- Example: AWS Elastic Beanstalk  

---

#### 3. SaaS (Software as a Service)

- Fully managed by provider  
- Example: Gmail, Slack  

---

### 2.5 Cloud Deployment Models

| Model | Description |
|------|------------|
| Public Cloud | Shared infrastructure (e.g., AWS) |
| Private Cloud | Dedicated infrastructure (banks, government) |
| Hybrid Cloud | Combination of public + private |
| Multi-Cloud | Multiple providers (AWS + Azure) |

---

### 2.6 Key Characteristics of Cloud

> Frequently asked in interviews

- On-demand self-service  
- Broad network access  
- Resource pooling  
- Rapid elasticity  
- Measured service (pay-per-use)  

---

### 2.7 Cloud Pricing Models

| Model | Description |
|------|------------|
| Pay-As-You-Go | Pay only for usage |
| Reserved Instances | Commit long-term for lower cost |
| Spot Instances | Very cheap, but can terminate anytime |

---

### 2.8 Latency

Latency = time taken for data to travel.

#### Why it matters

- Affects application performance  
- Critical for real-time systems (gaming, trading)  

#### Solutions

- Deploy closer to users  
- Use CDNs (Edge Locations)  

---

### 2.9 Disaster Recovery (DR)

Ensures system availability during failures.

| Strategy | Description |
|----------|------------|
| Backup & Restore | Cheapest, slow recovery |
| Pilot Light | Minimal infrastructure running |
| Warm Standby | Scaled-down active system |
| Active-Active | Fully redundant systems |

---

### 2.10 Why Terraform?

Cloud provides infrastructure via APIs.

Terraform enables:

- Infrastructure automation  
- Consistency across environments  
- Version-controlled infrastructure  
- Reduced manual errors  

#### Without Terraform

- Manual setup → inconsistent environments  

#### With Terraform

- Same infrastructure across dev, staging, production  

---

## 3. Deep Dive

### 3.1 How Cloud Works Internally

Cloud providers include:

- Global data centers  
- Virtualization (hypervisors)  
- APIs for infrastructure control  

#### Example: Launching an EC2 Instance

1. API request is sent  
2. Scheduler selects host machine  
3. VM created using hypervisor  
4. Network and storage attached  

---

### 3.2 Regions, Availability Zones & Edge Locations

| Component | Description |
|----------|------------|
| Region | Geographical area (e.g., Mumbai) |
| Availability Zone | Isolated data centers within a region |
| Edge Location | CDN nodes for faster content delivery |

#### Architecture Flow

```
User → Edge Location → Region → AZ → EC2
```

---

## 4. Practical Examples

### Scenario 1: Startup Scaling

- Start with 1 EC2 instance  
- Traffic increases  
- Scale to 10 instances automatically  

> Cloud enables instant scaling

---

### Scenario 2: Disaster Recovery

- Primary region fails  
- Traffic redirected to secondary region  

---

### Scenario 3: Terraform Use Case

Instead of manually creating servers:

```hcl
resource "aws_instance" "web" {
  count = 10
}
```

---

## 5. Commands & Configuration

### 5.1 Install AWS CLI (Linux)

```bash
sudo apt update
sudo apt install awscli -y
```

---

### 5.2 Configure AWS CLI

```bash
aws configure
```

You will need:

- Access Key  
- Secret Key  
- Region  

---

### 5.3 Basic AWS CLI Commands

```bash
# List EC2 instances
aws ec2 describe-instances

# Create key pair
aws ec2 create-key-pair --key-name my-key
```

---

## 6. Best Practices

- Choose nearest region for low latency  
- Follow naming conventions:

```
project-env-resource
example: ecommerce-dev-ec2
```

- Avoid using root AWS account  
- Use IAM users for security  

---

## 7. Key Takeaway

Cloud provides scalable infrastructure.

Terraform transforms it into:

- Automated  
- Repeatable  
- Reliable systems  

---
# Terraform Fundamentals, Installation & Core Workflow

---

## 1. Overview

This section introduces Terraform, a core DevOps tool for infrastructure automation.

### Learning Objectives

- Understand what Terraform is and how it works internally  
- Install and configure Terraform on Linux  
- Learn the Terraform workflow  
- Understand core commands (including `import`)  
- Learn Terraform configuration structure  

> **Outcome:**  
You will be able to provision and manage infrastructure using Terraform correctly.

---

## 2. Core Concepts

### 2.1 What is Terraform?

Terraform is an **Infrastructure as Code (IaC)** tool used to:

- Define infrastructure using code  
- Provision and manage resources automatically  

**Created by:** HashiCorp  

---

### 2.2 Why Terraform?

#### Without Terraform

- Manual cloud console operations  
- Error-prone and inconsistent  

#### With Terraform

- Version-controlled infrastructure  
- Reusable configurations  
- Automated deployments  

---

### 2.3 Infrastructure as Code (IaC)

IaC means writing infrastructure in code instead of manually creating it.

#### Example

```hcl
resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

---

### 2.4 Terraform vs Other Tools

| Tool | Type | Approach |
|------|------|----------|
| Terraform | Declarative | Define desired state |
| Ansible | Procedural | Define steps |
| CloudFormation | Declarative | AWS-specific |

---

### 2.5 Declarative Nature

Terraform is **declarative**, meaning:

- You define *what you want*  
- Terraform decides *how to achieve it*  

---

## 3. Deep Dive

### 3.1 Terraform Architecture

Terraform consists of three main components:

#### 1. Core
- Reads configuration  
- Builds execution plan  

#### 2. Providers
- Plugins to interact with APIs (AWS, Azure, GCP)  

#### 3. State
- Tracks current infrastructure  

---

### 3.2 Terraform Workflow (Critical)

#### Step 1: Write Configuration

```hcl
resource "local_file" "test" {
  filename = "test.txt"
  content  = "Hello DevOps"
}
```

---

#### Step 2: Initialize

```bash
terraform init
```

- Downloads providers  
- Prepares working directory  

---

#### Step 3: Plan

```bash
terraform plan
```

- Shows changes  
- No execution  

---

#### Step 4: Apply

```bash
terraform apply
```

- Creates/updates resources  

---

#### Step 5: Destroy

```bash
terraform destroy
```

- Deletes infrastructure  

---

### Workflow Summary

```
Write → Init → Plan → Apply → Manage → Destroy
```

---

### 3.3 Terraform Configuration Structure

#### Basic File: `main.tf`

##### Provider Block

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

##### Resource Block

```hcl
resource "aws_instance" "web" {
  ami           = "ami-xyz"
  instance_type = "t2.micro"
}
```

##### Optional Components

- Variables  
- Outputs  

---

### 3.4 Providers

Providers allow Terraform to interact with external systems:

- AWS  
- Azure  
- GCP  
- Local system  

#### Example

```hcl
provider "local" {}
```

---

## 4. Practical Examples

### 4.1 Create a Local File

```hcl
resource "local_file" "example" {
  filename = "hello.txt"
  content  = "Welcome to Terraform"
}
```

---

### 4.2 Simulate Infrastructure Change

Modify:

```hcl
content = "Updated content"
```

Run:

```bash
terraform plan
```

> Shows changes before applying

---

### 4.3 Import Existing Resource

```bash
terraform import aws_instance.web i-1234567890
```

> Brings existing infrastructure under Terraform control

---

## 5. Commands & Configuration

### 5.1 Install Terraform (Linux)

```bash
sudo apt update
sudo apt install -y gnupg software-properties-common curl

curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -

sudo apt-add-repository "deb https://apt.releases.hashicorp.com $(lsb_release -cs) main"

sudo apt update
sudo apt install terraform -y
```

---

### 5.2 Verify Installation

```bash
terraform version
```

---

### 5.3 Core Commands

```bash
terraform init        
terraform fmt         
terraform validate    
terraform plan        
terraform apply       
terraform destroy     
```

---

### 5.4 Terraform Import

```bash
terraform import <resource_type>.<name> <id>
```

#### Example

```bash
terraform import aws_instance.web i-123456
```

---

### 5.5 Debugging

```bash
TF_LOG=DEBUG terraform apply
```

---

## 6. Best Practices

- Always run:
  ```bash
  terraform fmt
  terraform validate
  terraform plan
  ```

- Never skip the `plan` step  
- Use `.gitignore` for:

```
.terraform/
terraform.tfstate
```

- Keep configurations modular  
- Use meaningful resource names  

---

## 7. Key Takeaway

Terraform enables:

- Automated infrastructure  
- Consistent environments  
- Scalable DevOps workflows  

> It transforms cloud infrastructure into **code-driven systems**

---

# Terraform State, Backend & Internal Working (Critical Concepts)

---

## 1. Overview

This section covers one of the most critical aspects of Terraform: **state management**.

### Learning Objectives

- Understand Terraform state and its purpose  
- Learn how Terraform tracks infrastructure  
- Understand internal execution flow  
- Learn about backends and their importance  
- Explore local vs remote backends  
- Introduction to remote state (S3 + DynamoDB)  

> **Outcome:**  
You will understand how Terraform "remembers" infrastructure, enabling safe automation.

---

## 2. Core Concepts

### 2.1 What is Terraform State?

Terraform state is a file that stores the mapping between:

- Terraform configuration  
- Real-world infrastructure  

**Default file:**

```
terraform.tfstate
```

---

### 2.2 Why is State Required?

Terraform is **stateless by design**, so it relies on state to:

- Track existing resources  
- Detect changes  
- Avoid duplication  

#### Without State (Problem)

- Terraform cannot track existing resources  
- Attempts to recreate everything  
- Leads to duplication and errors  

#### With State (Solution)

- Compares desired vs current state  
- Performs only necessary changes  

---

### 2.3 Desired vs Current State

| Type | Source |
|------|--------|
| Desired State | `.tf` files |
| Current State | `.tfstate` file |

---

### 2.4 What Does State File Contain?

- Resource IDs (e.g., EC2 ID)  
- Metadata  
- Dependencies  
- Output values  

#### Example

```json
{
  "resources": [
    {
      "type": "local_file",
      "name": "test",
      "instances": [
        {
          "attributes": {
            "filename": "devops.txt"
          }
        }
      ]
    }
  ]
}
```

---

## 3. Deep Dive

### 3.1 Internal Working of Terraform

#### Execution Steps

1. Read `.tf` configuration  
2. Load state file  
3. Compare desired vs current state  
4. Create execution plan  
5. Apply changes  
6. Update state file  

#### Execution Flow

```
.tf files → State → Diff → Plan → Apply → Updated State
```

---

### 3.2 State Locking (Critical for Teams)

#### Problem

- Multiple users run `terraform apply` simultaneously  
- Leads to state corruption  

#### Solution

- State locking mechanism prevents concurrent updates  

---

### 3.3 What is a Backend?

A backend defines:

- Where state is stored  
- How it is accessed  

---

### 3.4 Types of Backends

#### 1. Local Backend (Default)

- State stored locally  
- Not suitable for teams  

#### 2. Remote Backend

- State stored remotely (e.g., S3)  
- Supports locking  
- Enables team collaboration  

---

### 3.5 Why Backend is Required?

Without backend:

- No collaboration  
- Risk of losing state  
- No locking mechanism  

---

### 3.6 S3 Backend (Industry Standard)

#### Components

- **S3 Bucket** → Stores state file  
- **DynamoDB** → Handles state locking  

#### Architecture

```
Terraform → S3 (State Storage)
           ↓
        DynamoDB (Locking)
```

---

### 3.7 Backend Configuration Example

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state-bucket"
    key            = "dev/terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-lock"
  }
}
```

---

### 3.8 State Commands

```bash
terraform show                      
terraform state list               
terraform state show local_file.test  
```

---

### 3.9 Sensitive Nature of State

State files may contain:

- Passwords  
- Secrets  
- Private keys  

>  Never expose state files publicly

---

## 4. Practical Examples

### 4.1 Observe State File

```bash
cat terraform.tfstate
```

> Shows actual infrastructure mapping

---

### 4.2 Change Detection

```bash
terraform plan
```

> Detects differences using state

---

### 4.3 Team Conflict Scenario

- Without locking → state corruption  
- With backend → safe execution  

---

## 5. Commands & Configuration

### 5.1 State Commands

```bash
terraform state list
terraform state show <resource>
terraform show
```

---

### 5.2 Backend Initialization

```bash
terraform init
```

> Re-initializes and configures backend

---

### 5.3 Migrate State (Local → Remote)

```bash
terraform init -migrate-state
```

---

### 5.4 Refresh State

```bash
terraform refresh
```

---

### 5.5 Remove Resource from State

```bash
terraform state rm <resource>
```

---

## 6. Best Practices

- Never commit state file to Git  
- Always use remote backend in teams  
- Enable state locking  
- Encrypt S3 bucket  
- Enable versioning in S3  

### Recommended `.gitignore`

```
.terraform/
terraform.tfstate
terraform.tfstate.backup
```

---

## 7. Key Takeaway

Terraform state is the backbone of infrastructure management.

It enables:

- Accurate change detection  
- Safe automation  
- Reliable infrastructure lifecycle management  

> Without state, Terraform cannot function correctly

---

# Terraform Practical — Local Execution, File Automation & Provisioners

---

## 1. Overview

This section focuses on **hands-on Terraform usage beyond cloud provisioning**.

### Learning Objectives

- Execute local Linux commands using Terraform  
- Create, modify, and compress files  
- Understand provisioners and their behavior  
- Learn execution-based vs resource-based automation  
- Identify when to use (and avoid) provisioners  

> **Outcome:**  
You will be able to automate local system tasks using Terraform like a DevOps engineer.

---

## 2. Core Concepts

### 2.1 What Are Provisioners?

Provisioners allow Terraform to **execute scripts or commands after resource creation**.

#### Types of Provisioners

| Type | Description |
|------|------------|
| `local-exec` | Executes commands on local machine |
| `remote-exec` | Executes commands on remote systems (e.g., EC2) |

---

### 2.2 Why Use Provisioners?

Common use cases:

- Configure systems after provisioning  
- Run shell scripts  
- Automate file operations  
- Trigger workflows  

---

### 2.3 Important Warning 

Provisioners should be used cautiously.

#### Limitations

- Not idempotent  
- Harder to debug  
- Can cause inconsistent states  

> Recommended: Use only as a **last resort**

---

### 2.4 Local Operations with Terraform

Terraform uses a placeholder resource:

```hcl
resource "null_resource" "example" {}
```

> Used to execute provisioners without creating real infrastructure

---

## 3. Deep Dive

### 3.1 How `local-exec` Works

#### Execution Flow

1. Terraform creates resource  
2. Provisioner executes command  
3. Output is displayed in terminal  

#### Execution Model

```
Resource Creation → Provisioner Trigger → Command Execution
```

---

### 3.2 Idempotency Challenge

Terraform is designed to be:

- Idempotent (same result on repeated runs)

Provisioners may:

- Run multiple times  
- Create duplicate outputs  
- Break consistency  

---

### 3.3 Triggers in `null_resource`

Used to control execution behavior.

```hcl
resource "null_resource" "example" {
  triggers = {
    always_run = timestamp()
  }
}
```

> Forces execution on every run

---

### 3.4 Dependency Management

Use explicit dependencies:

```hcl
depends_on = [resource_name]
```

> Ensures correct execution order

---

## 4. Practical Examples

### 4.1 Create a File

```hcl
resource "null_resource" "create_file" {
  provisioner "local-exec" {
    command = "echo 'Hello DevOps' > file.txt"
  }
}
```

---

### 4.2 Append Content

```hcl
command = "echo 'Second Line' >> file.txt"
```

---

### 4.3 Zip a File

```hcl
resource "null_resource" "zip_file" {
  provisioner "local-exec" {
    command = "zip file.zip file.txt"
  }
}
```

---

### 4.4 Combined Operations

```hcl
resource "null_resource" "file_ops" {
  provisioner "local-exec" {
    command = <<EOT
      echo "Terraform Automation" > file.txt
      echo "Learning DevOps" >> file.txt
      zip file.zip file.txt
    EOT
  }
}
```

---

## 5. Commands & Configuration

### 5.1 Project Setup

```bash
mkdir terraform-day4
cd terraform-day4
```

---

### 5.2 Full Terraform Script

```hcl
provider "local" {}

resource "null_resource" "file_creation" {
  provisioner "local-exec" {
    command = "echo 'Hello from Terraform' > devops.txt"
  }
}

resource "null_resource" "zip_creation" {
  depends_on = [null_resource.file_creation]

  provisioner "local-exec" {
    command = "zip devops.zip devops.txt"
  }
}
```

---

### 5.3 Execute Terraform

```bash
terraform init
terraform plan
terraform apply
```

---

### 5.4 Verify Output

```bash
ls
cat devops.txt
unzip devops.zip
```

---

### 5.5 Clean Up

```bash
terraform destroy
```

---

## 6. Best Practices

- Use provisioners only when necessary  
- Prefer alternatives:
  - Cloud-init  
  - Configuration tools (e.g., Ansible)  

- Always define dependencies (`depends_on`)  
- Keep scripts simple and maintainable  
- Log outputs for debugging  

---

## 7. Key Takeaway

Provisioners extend Terraform’s capability beyond infrastructure.

However:

- They should be used **strategically and minimally**  
- Best suited for **edge-case automation scenarios**  

> Terraform is strongest when used for **infrastructure provisioning, not scripting**

---

# AWS + Terraform Integration — EC2 Provisioning via Console, CLI & Terraform

---

## 1. Overview

This section connects all prior concepts into real-world infrastructure provisioning.

### Learning Objectives

- Create AWS resources using Console  
- Provision infrastructure using AWS CLI  
- Automate infrastructure using Terraform  
- Apply naming conventions and best practices  

> **Outcome:**  
Understand how DevOps engineers provision infrastructure across multiple interfaces — and why Terraform is preferred.

---

## 2. Core Concepts

### 2.1 EC2 (Elastic Compute Cloud)

EC2 is a **virtual server in AWS** used to run applications.

#### Key Components

| Component | Description |
|----------|------------|
| AMI | OS template (Amazon Machine Image) |
| Instance Type | CPU & RAM (e.g., `t2.micro`) |
| Key Pair | Used for SSH access |
| Security Group | Firewall rules |
| Region & AZ | Deployment location |

---

### 2.2 Three Ways to Create Infrastructure

| Method | Usage |
|--------|------|
| Console | Manual (learning, quick tasks) |
| CLI | Script-based automation |
| Terraform | Infrastructure as Code (production) |

---

### 2.3 Why Terraform Wins

- Reusable configurations  
- Version-controlled infrastructure  
- Consistent environments  
- Team collaboration  

---

## 3. Deep Dive

### 3.1 EC2 Creation Flow (Internal)

When an EC2 instance is launched:

1. Request sent to AWS API  
2. Scheduler selects Availability Zone  
3. Hypervisor creates virtual machine  
4. Storage and networking attached  
5. Instance becomes available  

---

### 3.2 Authentication Methods

AWS CLI and Terraform use:

- Access Key  
- Secret Key  

Stored in:

```
~/.aws/credentials
```

---

### 3.3 Terraform AWS Provider

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

> Connects Terraform to AWS APIs

---

### 3.4 Naming Convention (Important)

Follow:

```
project-env-resource
```

#### Example

```
ecommerce-dev-ec2
```

---

## 4. Practical Examples

### 4.1 Create EC2 via AWS Console

#### Steps

1. Go to EC2 Dashboard  
2. Click **Launch Instance**  
3. Configure:
   - Name: `week6-dev-ec2`  
   - AMI: Ubuntu  
   - Instance Type: `t2.micro`  
   - Key Pair: Create/download  
4. Launch instance  

---

### 4.2 Create EC2 via AWS CLI

```bash
aws ec2 run-instances \
  --image-id ami-xxxxxxxx \
  --instance-type t2.micro \
  --key-name my-key \
  --security-groups default
```

---

### 4.3 Create EC2 via Terraform

```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "dev" {
  ami           = "ami-xxxxxxxx"
  instance_type = "t2.micro"

  tags = {
    Name = "week6-dev-ec2"
  }
}
```

---

## 5. Commands & Configuration

### 5.1 Configure AWS CLI

```bash
aws configure
```

---

### 5.2 Verify Credentials

```bash
aws sts get-caller-identity
```

---

### 5.3 Terraform Project Setup

```bash
mkdir terraform-aws
cd terraform-aws
```

---

### 5.4 Full Terraform Script

```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "dev" {
  ami           = "ami-0f58b397bc5c1f2e8"  # Example Ubuntu AMI (update if needed)
  instance_type = "t2.micro"

  tags = {
    Name = "week6-dev-ec2"
  }
}
```

---

### 5.5 Run Terraform

```bash
terraform init
terraform plan
terraform apply
```

---

### 5.6 Verify EC2 Instance

```bash
aws ec2 describe-instances
```

---

### 5.7 Destroy Resources

```bash
terraform destroy
```

---

## 6. Best Practices

- Always tag resources properly  
- Use variables instead of hardcoding values  
- Use IAM users/roles instead of root credentials  
- Use remote backend for team workflows  
- Separate environments:


    - dev  
    - staging
    - prod

---

### Example: Using Variables

```hcl
variable "instance_type" {
  default = "t2.micro"
}
```

---

## 7. Key Takeaway

Infrastructure can be created via:

- Console → Manual  
- CLI → Scripted  
- Terraform → Automated & scalable  

> Terraform is the **industry standard** for managing infrastructure as code.

---