# Portfolio Lab 2 — Multi-Tier SaaS VPC Architecture (AWS)

## Objective

Design and deploy a multi-tier SaaS-ready AWS VPC infrastructure simulating real-world production cloud environments using:

- Public subnets
- Private subnets
- NAT Gateway
- Internet Gateway
- Application Load Balancer (ALB)
- Full internal routing
- Secure SSH access via Bastion approach

## Architecture Overview

- VPC CIDR: 10.20.0.0/16
- Public Subnet 1: 10.20.1.0/24
- Public Subnet 2: 10.20.4.0/24 (for ALB multi-AZ requirement)
- Private Subnet Backend: 10.20.2.0/24
- Private Subnet Database: 10.20.3.0/24

## Key Components

- Internet Gateway attached to VPC
- Public Route Table: 0.0.0.0/0 → IGW
- Private Route Table: 0.0.0.0/0 → NAT Gateway
- NAT Gateway launched in Public Subnet
- EC2 Instances:
  - Frontend EC2 in Public Subnet (Bastion Access)
  - Backend EC2 in Private Subnet (secured)
- Application Load Balancer (ALB) in Public Subnet
- ALB forwards HTTP traffic to Backend EC2
- Target Group for backend registered with ALB
- Health Check configured for Target Group

## Security Groups

### Public Subnet EC2 SG

- Allow SSH (22) from My IP
- Allow HTTP (80) from 0.0.0.0/0

### Backend Private EC2 SG

- Allow SSH (22) from Frontend EC2 private IP (10.20.1.31/32)
- Allow HTTP (80) from ALB Security Group

### ALB SG

- Allow HTTP (80) from 0.0.0.0/0

## Testing & Validation

- Successfully accessed ALB DNS endpoint:  
  `http://portfolio-alb-02-XXXXXXXX.us-east-2.elb.amazonaws.com`

- Backend served Apache default page:  
  `It works!`

- Verified NAT Gateway for private subnet outbound traffic.

- Verified Target Group health check status as healthy.

- Verified SSH access from public EC2 to private backend EC2 via PEM key forwarding.

## Architecture Diagram

AWS SaaS Multi-Tier Architecture

               Internet
                  |
              Internet Gateway
                  |
          Application Load Balancer
                  |
          Public Subnet (Frontend)
                  |
      ---------------------------
      |                         |
  Private Subnet 1        Private Subnet 2
  (Backend EC2)           (Database RDS)
                  |
          NAT Gateway for outbound internet


## Key Learnings

- Full SaaS multi-tier VPC design
- Public/private subnet separation
- NAT Gateway architecture
- ALB Target Group configuration
- Internal SSH tunneling / Bastion concept
- Security Group hardening for enterprise-grade cloud networking
- Cost optimization by destroying resources after validation

## Proof of Work

- VPC, Subnets, Route Tables, NAT Gateway screenshots
- Security Group configurations
- EC2 Launch details
- Load Balancer DNS output
- Target Group health status
- SSH session proofs from Bastion → Backend
- Browser screenshot of ALB serving HTTP response

## Billing & Cost Controls

- All resources were deleted after testing to avoid AWS charges.
- Elastic IPs released.
- NAT Gateway destroyed.
- ALB deleted.

## Conclusion

Successfully deployed a real-world SaaS multi-tier cloud infrastructure inside AWS with public-facing ALB and fully isolated private backend using professional architecture design standards.
