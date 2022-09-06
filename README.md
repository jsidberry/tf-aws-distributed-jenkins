## A Distributed, Multi-Region, Jenkins CI/CD Deployment

![architectural diagram](https://github.com/jsidberry/tf-aws-distributed-jenkins/blob/main/images/arch-diag.png)


The end result of this project is to have a distributed Jenkins setup on a cloud platform that will function as the backbone of a CI/CD pipeline for software development.

This project uses several technologies used in the principles of DevOps. 
- **AWS** : data-center platform to host the infrastructure.
- **Terraform** : Infrastructure-as-code (IaC) used to build and maintain the infrastructure
- **Ansible** : configuration management (CM) used to install and configure the tools, like Jenkins, on the hosts machines
- **Jenkins** : the main tool used for the continuous integration & continuous deployment/delivery (CI/CD) functionality

I personally use this repo for initial build-outs for other projects. 

# Requirements
- AWS cli 
- boto3 (`pip3 install boto3 --user`)

# Notes

## ansible
Ensure there is a proper `ansible.cfg` file in the same directory as this code. Once the .cfg is included in this repo. However, for security reasons, please review the details of the configuration file.

## terraform
- setup your `variables.tf` file first for the initial variables
- setup the `terraform.tfvars` file, which is not in this repo. You must create or have this separate for security reasons.
- create a `backend` for an `s3` resource to host the `state` files
- create the initial `providers` for the AWS resources
- create the **networking**
    - create the `vpc` resource(s) per region
    - create the **internet gateway** (`igw`) resource per `vpc`
    - create the `subnet` resources for each **AZ** in each `vpc`
    - create **VPC Peering** resources between VPCs
    - create **Route Table** (`rtb`) resources 
- create the **security groups** (`sg`) resources
- create the VM resources (`ec2` **instances**)
    - get the AMI ID using the API from Systems Manager (SSM) 
    - Deploy Key Pairs for the Application Nodes
    - Deploy Jenkins Master and Worker Instances
    - create outputs for the IP address of the created VMs resources (`ec2`)
        
- use ANSIBLE to install & start services on the Jenkins nodes
- create an **Application Load Balancer (ALB)** and route traffic to EC2 App (Jenkins) Node
    - add the load balancer with `aws_lb` resource
    - add the target group with `aws_lb_target_group` resource
    - add add a listener with `aws_lb_listener` resource
    - attach the `target_group` to the `alb` by adding `aws_lb_target_group_attachment` resource
- Set up HTTPS and a Route 53 Record
