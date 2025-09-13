# EKS Cluster Deployment with Terraform

This repository contains Terraform code to provision an **Amazon Elastic Kubernetes Service (EKS)** cluster on AWS.  
The code is organized into reusable modules and backend configuration for better scalability and maintainability.

---

## 📂 Repository Structure

EKS_terraform_clean/
├── Backend/ # Remote backend configuration for storing Terraform state in S3 and DynamoDB
├── Modules/ # Reusable modules for networking, IAM roles, and EKS resources
├── main.tf # Root Terraform configuration that ties modules together
├── variables.tf # Input variables for customization
├── output.tf # Outputs to display after provisioning
└── .gitignore # Prevents unnecessary files (state, providers) from being committed


---

## 🧩 **Code Explanation**

### 1️⃣ **Backend/**  
- **Purpose**: Configures a **remote backend** so Terraform state is stored securely in an **S3 bucket**, with **DynamoDB table** for state locking.  
- **Why**: Prevents local state corruption, enables team collaboration, and avoids multiple people applying conflicting changes.  
- **Key Elements**:
  - `bucket`: S3 bucket name for storing state.
  - `key`: Path to the state file inside the bucket.
  - `region`: AWS region for S3 and DynamoDB.
  - `dynamodb_table`: Table for state locking.

---

### 2️⃣ **Modules/**  
- **Purpose**: Contains reusable modules for different components:
  - **VPC Module**: Creates networking resources like VPC, subnets, route tables, and internet gateway.
  - **EKS Module**: Provisions the EKS cluster, worker node groups, and attaches IAM roles.
  - **IAM Module**: Manages roles and permissions for EKS and worker nodes.
- **Why**: Makes your Terraform code modular, DRY (Don’t Repeat Yourself), and easier to maintain.

---

### 3️⃣ **main.tf**  
- **Purpose**: The main entry point that wires together all the modules and backend.  
- **What It Does**:
  - Configures the AWS provider and region.
  - Calls the VPC module to create networking.
  - Calls the IAM module to create necessary roles.
  - Calls the EKS module to create the cluster and node groups.
  - References variables and outputs.

---

### 4️⃣ **variables.tf**  
- **Purpose**: Defines input variables for customizable parameters.  
- **Examples**:
  - `region`: AWS region where resources will be deployed.
  - `cluster_name`: Name of the EKS cluster.
  - `node_instance_type`: EC2 instance type for worker nodes.
  - `desired_capacity`: Number of worker nodes.  
- **Why**: Makes your code flexible for different environments without editing the main code.

---

### 5️⃣ **output.tf**  
- **Purpose**: Defines the values Terraform will display after a successful `apply`.  
- **Examples**:
  - `cluster_endpoint`: EKS API server endpoint.
  - `cluster_security_group_id`: Security group ID for the cluster.
  - `node_group_role_arn`: IAM role for worker nodes.  
- **Why**: Helps users quickly retrieve important information needed for connecting `kubectl` or other tools.

---

### 6️⃣ **.gitignore**  
- **Purpose**: Prevents committing unnecessary files to the repository.  
- **Key Entries**:
  - `.terraform/` and `.terraform.lock.hcl` — Local provider cache and lock files.  
  - `*.tfstate` and `*.tfstate.backup` — Terraform state files.  
  - `crash.log` — Optional crash logs.  
  - Provider binaries — Avoid pushing large files to GitHub.  

---

## 🚀 **How to Use**

### Prerequisites
- AWS CLI configured with valid credentials.
- Terraform installed (`terraform -version`).

### Steps
1. **Initialize Terraform**  
   terraform init

2. **Review the plan**
   terraform plan

4. **Apply changes**
   terraform apply
 
6. **To destroy resources**
   terraform destroy

Notes

Never commit .tfstate or .terraform/ directories.
To update the cluster, modify variables or module code and re-run terraform apply. 

Outcome

Running terraform apply will provision:
A fully functional EKS cluster.
VPC networking (subnets, route tables, etc.).
IAM roles and security groups for EKS and worker nodes.
Scalable worker nodes ready for Kubernetes workloads.

