# How to structure a terraform environment.

Prerequisites: 

1. Create an s3 bucket in your aws account
2. Create a keypair, i named mine: server-key.pem   - just for demo

# Terraform Infrastructure Modules for Multi-Environment Deployment

This repository contains reusable, modular Terraform code for provisioning core AWS infrastructure components across multiple environments (e.g., `dev`, `qa`, `prod`). The project follows infrastructure-as-code best practices by organizing infrastructure into environment-specific configurations and modular components.

## 🧱 Modules

The project is composed of the following core modules, located in the `modules/` directory:

- `ec2/`: Provisions EC2 instances with customizable parameters (AMI, instance type, tags, etc.)
- `vpc/`: Creates Virtual Private Clouds (VPCs) including subnets, route tables, and internet gateways
- `security_group/`: Defines and manages security groups for controlling inbound and outbound traffic

Each module is designed to be reused across multiple environments through variable injection.

## ⚙️ Provider and Backend Configuration

The `provider.tf` file, located at the root of the project, specifies the AWS provider, region, and remote backend configuration using S3 for storing state files.

Each environment (`dev`, `qa`, etc.) has its own backend configuration file (`dev.tfbackend`, `qa.tfbackend`, etc.), enabling isolated state management per environment.

## 🚀 Usage Instructions

### 1. Initialize Terraform Backend for an Environment

To begin working in a specific environment (e.g., `dev`), initialize the backend using the relevant backend config:

```bash
terraform init --backend-config="dev.tfbackend"
```

💡 Note: If you've previously initialized another environment (e.g., qa) and need to switch back, you must reconfigure:

Terraform will prompt whether to migrate state — you can safely use --reconfigure to point to the new backend without affecting other environments.

```bash
terraform init --backend-config="dev.tfbackend" --reconfigure
```

Once initialized, apply your infrastructure using the environment-specific tfvars file:

```bash
terraform apply -var-file="dev.tfvars"
```

Repeat the process for other environments (qa, prod) by replacing the respective .tfvars and .tfbackend files.

📦 Benefits of This Setup

🔁 Reusable Modules – DRY (Don't Repeat Yourself) principles with shared modules across environments
🔐 Environment Isolation – Separate remote state files per environment using S3 backends
📁 Clear Structure – Separation between module definitions and environment configurations
☁️ Cloud Agnostic Logic – Designed to run on any standard AWS setup using the AWS provider

📌 Notes

Each environment must be initialized separately.
To avoid backend conflicts, always reinitialize when switching between environments using --reconfigure.
Ensure your S3 backend includes versioning and locking for safe collaboration.

🤝 Contributions

Pull requests are welcome. For significant changes, please open an issue first to discuss what you would like to change.





