# Ansible Kubernetes Cluster Setup

## Overview

Welcome to the **Ansible Kubernetes Cluster Setup** project! This project demonstrates the configuration and scaling of a minimalist Kubernetes cluster using Ansible and Terraform. It provides an automated approach to deploying a Kubernetes cluster on AWS, making it easier to manage and scale containerized applications.

## Description

The Ansible Kubernetes Cluster Setup project is designed to simplify the deployment and management of Kubernetes clusters using infrastructure as code principles. By leveraging Ansible for configuration management and Terraform for provisioning cloud resources, this project allows users to quickly set up a robust Kubernetes environment. The setup includes automated installation of necessary components, configuration of networking, and scaling of resources as needed. The project also employs Ansible roles to organize tasks and configurations effectively, promoting reusability and clarity in the deployment process.

## Features

1. **Automated Deployment**:
   - Streamlined deployment of Kubernetes clusters using Ansible playbooks and Terraform scripts.

2. **Ansible Roles**:
   - Utilization of Ansible roles to structure the configuration and management tasks, enhancing modularity and reusability.

3. **Scalable Architecture**:
   - Easily scale the number of worker nodes in the Kubernetes cluster to handle varying workloads.

4. **Dynamic Inventory Management**:
   - Use Ansible's dynamic inventory capabilities to manage cloud resources efficiently.

5. **SSH Key Management**:
   - Simplified SSH key generation and management for secure access to EC2 instances.

6. **Kubernetes Configuration**:
   - Deploy and manage Kubernetes resources, including deployments and services, directly through Ansible playbooks.

7. **Support for Nginx Deployment**:
   - Example deployment of an Nginx server to demonstrate application hosting on the Kubernetes cluster.

## Project Structure

The project is organized as follows:

```
- configuration-management-automation-with-ansible-VaishnaviBarot/: Contains Ansible playbooks, roles, and inventory configuration.
- seneca-acs730-ansible-k8s/: Includes Terraform configuration for infrastructure scaling.
```

![Project Structure](https://github.com/NCC-0514/configuration-management-automation-with-ansible-VaishnaviBarot/assets/89254669/e4f86072-7dc5-4a56-acdc-ed81a13e7719)

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed on your local machine:

1. Ansible
2. Terraform
3. AWS credentials configured

### Setup Instructions

1. Clone this repository to your local machine:
   ```bash
   git clone https://github.com/kktse/seneca-acs730-ansible-k8s.git
   ```

2. Change the path to the key name in `bastionhost.tf`:
   ```hcl
   public_key = file("/home/ec2-user/.ssh/${var.bastion_host_key_name}.pub")
   ```

3. Change the path to the key name in `k8scommon.tf`:
   ```hcl
   public_key = file("/home/ec2-user/.ssh/${var.k8s_key_name}.pub")
   ```

4. Set up keys for SSH access using the following commands:
   ```bash
   ssh-keygen -t rsa -f /home/ec2-user/.ssh/bastion_host_key
   ssh-keygen -t rsa -f /home/ec2-user/.ssh/k8s_key
   ```

5. Navigate to the Terraform folder and run:
   ```bash
   cd seneca-acs730-ansible-k8s
   terraform init
   terraform validate
   terraform plan
   terraform apply
   ```

6. Clone the Ansible repository to your local machine:
   ```bash
   git clone https://github.com/NCC-0514/configuration-management-automation-with-ansible-VaishnaviBarot.git
   ```

7. View the dynamic inventory:
   ```bash
   ansible-inventory -i aws_ec2.yaml --graph
   ```

8. Deploy the Kubernetes cluster:
   ```bash
   ansible-playbook -i aws_ec2.yaml deploy_kubernetes.yaml
   ```

9. SSH into the bastion host:
   ```bash
   ssh -i /home/ec2-user/.ssh/bastion_host_key ec2-user@<bastion-host-public-ip>
   ```

10. On the bastion host, run the following commands:
    ```bash
    cd /home/ec2-user
    ls -a
    cd .kube
    ls -a
    sudo mv "admin.conf" "config"
    cd ..
    ```

11. Run these commands on the bastion host:
    ```bash
    kubectl get nodes
    kubectl create deployment nginx --image=nginx
    kubectl get deployments
    kubectl create service nodeport nginx --tcp=80:80
    kubectl get services
    curl <worker-node-private-ip>:<nginx-portnumber>
    ```

12. Navigate back to the Terraform folder and scale the worker nodes:
    ```bash
    cd seneca-acs730-ansible-k8s
    terraform apply --var worker_count=2
    ```

13. Repeat step 8 to deploy the updated configuration.

14. SSH into the bastion host using step 9.

15. Finally, check the status of the nodes:
    ```bash
    kubectl get nodes
    ```

## Conclusion

This project provides a comprehensive approach to deploying and managing a Kubernetes cluster using Ansible and Terraform. By following the steps outlined above, you will be able to set up and scale a Kubernetes environment in AWS efficiently.

