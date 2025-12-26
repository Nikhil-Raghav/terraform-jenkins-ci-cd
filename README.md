# Terraform + Jenkins CI/CD

This repository demonstrates how AWS infrastructure is provisioned using
Terraform and executed through Jenkins CI/CD pipelines.

The goal is to showcase safe and consistent Infrastructure as Code (IaC)
automation using CI/CD best practices.

## 🔧 What’s included

- Terraform configuration to create AWS VPC and networking components
- Jenkins pipeline to run Terraform commands
- Separation of plan and apply stages
- Manual approval gate before infrastructure changes
- Parameterized pipeline for apply and destroy actions

## 🔄 Jenkins Workflow

1. Jenkins checks out the Terraform code
2. Terraform is initialized
3. `terraform plan` is generated and reviewed
4. Manual approval is required
5. Terraform `apply` or `destroy` is executed based on user input

## 📁 Repository Structure

terraform-jenkins-ci-cd/
├── Jenkinsfile
├── main.tf
└── README.md
