# Ansible Deployment for AWS EC2 Instances

This repository contains a GitHub Actions workflow and an Ansible playbook for deploying and configuring Nginx on AWS EC2 instances.

## Workflow Overview

The GitHub Actions workflow defined in `.github/workflows/ansible-aws.yaml` is triggered on every push. The workflow consists of the following steps:

1. **Cache Layer**: Caches dependencies speed up subsequent workflow runs.
2. **Install Ansible**: Installs Ansible and necessary dependencies if not already cached.
3. **Install amazon.aws Collection**: Installs the `amazon.aws` Ansible collection if not already cached.
4. **AWS Authentication**: Configures AWS credentials using GitHub secrets for accessing AWS services.
5. **Code Checkout**: Checks out the repository code.
6. **Validate Inventory File**: Validates the Ansible inventory file (`inventory.aws_ec2.yaml`).
7. **Write SSH Key Into File**: Writes the private SSH key from GitHub secrets into a file for Ansible to use.
8. **Run Ansible Playbook**: Executes the Ansible playbook (`ec2-playbook.yaml`) to provision and configure Nginx on EC2 instances.

## Ansible Playbook

The Ansible playbook (`ec2-playbook.yaml`) defined in the repository is responsible for configuring Nginx on AWS EC2 instances. It performs the following tasks:

1. **Install Nginx**: Uses `apt` module to install the latest version of Nginx.
2. **Serve Custom File**: Copies a custom `index.html` file to `/var/www/html/` directory, making it accessible through Nginx.
3. **Restart Nginx**: Ensures Nginx service is restarted after configuration changes.

## Prerequisites

Before running the workflow, ensure the following prerequisites are met:

- An AWS account with necessary permissions.
- AWS Access Key ID and Secret Access Key stored as GitHub secrets.
- EC2 instances provisioned and accessible via SSH with `key_name` property equals to `GitHubActionsKeyPair`.
- Configure AWS credentials as GitHub secrets (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`).
- Configure the `GitHubActionsKeyPair` as GitHub secret.

## Final Result

This is how it looks like when you access AWS EC2 instanc using the public IP or the DNS name:
![Nginx Custom Page](https://github.com/amr-elzahar/github-actions-ansible-aws/images/final_result.png)
