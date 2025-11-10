# IAC
Infrastructure as Code means managing and provisioning your cloud infrastructure through code, instead of doing it manually through a console or UI.

- ***Automation*** : No manual clicks — infrastructure is deployed automatically through scripts.
- ***Consistency*** :	Same code → same infrastructure → no configuration drift.
- ***Speed*** :	You can create or destroy full environments (Dev, QA, Prod) in minutes.
- ***Version Control*** :	IaC files are stored in Git, so every infrastructure change is tracked.
- ***Reusability*** :	You can reuse and modularize your code for multiple projects.
- ***Disaster Recovery*** :	Rebuild entire environments quickly if something fails.
- ***ollaboration*** :	Multiple teams can work on the same codebase using Git workflows.
  
# What is terraform
Terraform is an open-source tool created by HashiCorp that helps you manage your cloud infrastructure using code. It uses a simple language called HCL (HashiCorp Configuration Language) to define what resources you need, like servers, networks, or databases.

Terraform can automatically create and manage these resources across different cloud providers such as AWS, Azure, and Google Cloud, all from a single set of configuration files.

# Why Terraform is Popular
- ***Multi-cloud support***	Works with AWS, Azure, GCP, VMware, etc.
- ***Declarative syntax***	You describe what you want, not how to do it.
- ***State management***	Tracks resources in a .tfstate file.
- ***Idempotent***	Running the same code again won’t recreate unchanged resources.
- ***Modules***	Lets you reuse infrastructure components (like functions).
# Terraform lifecycle
Terraform lifecycle defines the complete process of how Terraform initializes, plans, applies, and manages infrastructure as code from setup to destory or deletion.

Terraform workflow follows this sequence:

> ***Init → Fmt → Validate → Plan → Apply → Show/State → Destroy***

| Phase | Description | Command Example |
|--------|--------------|----------------|
| **1️⃣ Init (Initialize)** | Sets up your Terraform working directory. It downloads provider plugins (like AWS, Azure), initializes modules, and configures backend (like S3 & DynamoDB). Must be run first before any other command. | `terraform init` |
| **2️⃣ Fmt (Format)** | Formats your Terraform `.tf` files to a standard readable format. It ensures clean, consistent code style across your project. | `terraform fmt` |
| **3️⃣ Validate (Syntax Check)** | Checks if your Terraform code is syntactically and logically correct. Ensures no typos, missing variables, or invalid blocks before planning. | `terraform validate` |
| **4️⃣ Plan (Dry Run)** | Creates a detailed execution plan by comparing desired configuration with the current state. Shows which resources will be created, changed, or destroyed. | `terraform plan -var-file="dev.tfvars"` |
| **5️⃣ Apply (Deploy Infrastructure)** | Executes the plan — actually creates, updates, or deletes resources in your cloud. Updates the Terraform state file once done. | `terraform apply -auto-approve` |
| **6️⃣ Show (Review State)** | Displays the details of your deployed infrastructure from the Terraform state file. Helps verify what resources exist. | `terraform show` |
| **7️⃣ State (Manage State File)** | Allows you to inspect, move, or remove resources manually from the Terraform state. Used for advanced troubleshooting or imports. | `terraform state list` |
| **8️⃣ Destroy (Tear Down)** | Deletes all infrastructure created by Terraform safely. Used when you want to clean up environments like dev/test. | `terraform destroy -auto-approve` |

# Terraform Variables — Types and Precedence 
Terraform variables make your configuration **dynamic, reusable, flexible, and environment-independent**.  
Instead of hardcoding values, you can define variables and pass values dynamically from files, environment variables, or CLI to  manage different environments (like dev, stage, prod) using the same `.tf` files
# Example
<pre>
variable "region" {
  description = "AWS region"
  default     = "ap-south-1"
} 

provider "aws" {
  region = var.region
} </pre>

# Terraform Variables — Types and Loading Order
Terraform supports different variable **data types** such as **strings**, **numbers**, **booleans**, **lists**, **maps**, and **objects**.

| Type | Example | Description |
|-------|----------|-------------|
| **String** | `"ap-south-1"` | Simple text |
| **Number** | `3`, `10`, `2.5` | Numeric values |
| **Bool** | `true`, `false` | True or False values |
| **List** | `["t2.micro", "t2.small"]` | Ordered list of values |
| **Map** | `{ env = "dev", owner = "teamA" }` | Key-value pairs |
| **Object / Tuple** | Complex structures | Used in advanced modules |

# Example
<pre>variable "instance_type" {
  type    = string
  default = "t2.micro"
}

variable "allowed_ports" {
  type    = list(number)
  default = [22, 80, 443]
}

variable "tags" {
  type = map(string)
  default = {
    env   = "dev"
    owner = "ragha"
  }
} </pre>

# Terraform Variable Precedence

Terraform can take variable values from multiple sources.  If the same variable is defined in more than one place, Terraform follows a **specific order of precedence**.  
The **last (highest priority)** value always wins.

***Terraform Variable Loading Order***

| Step | Source | Example | Priority |
|------|----------|----------|-----------|
| **1** | **Default** | `default = "ap-south-1"` |  Lowest |
| **2** | **terraform.tfvars** | `region = "us-east-1"` | ⬆️ |
| **3** | **Custom tfvars file** | `terraform apply -var-file="prod.tfvars"` | ⬆️⬆️ 
| **4** | **Environment variable** | `export TF_VAR_region="ap-northeast-1"` | ⬆️⬆️⬆️ |
| **5** | **CLI variable** | `terraform apply -var="region=ca-central-1"` | **Highest** |

Terraform loads variables in this order:  
**Default → tfvars → custom tfvars → environment → CLI**

When duplicates exist —  The **last defined source (highest priority)** always overwrites the previous ones.

