# Jenkins on AWS EKS
A complete way to deploying Jenkins on AWS Elastic Kubernetes Service (EKS) with Helm and then deploy it on the domain on namecheap.

## Prerequisites
- AWS Account
- kubectl installed
- AWS CLI v2 installed
- Homebrew
- eksctl installed

## Installation Step-by-step process

### Step 1: Install Homebrew
[Homebrew Installation](1install_homebrew.png)

### Step 2: Install eksctl
[eksctl Installation](2eksctl_install.png)
Install eksctl using Homebrew in zsh shell:

brew install eksctl

Verify installation:
eksctl version

### Step 3: Create EKS Cluster

[Cluster Creation](3cluster_create.png)

Create a new EKS cluster by setup the aws configure on our local machine:

eksctl create cluster \
  --name jenkins-cluster \
  --region us-west-2 \
  --node-type t3.medium \
  --nodes 2

### 4. Check Pods
[Pods](4pods.png)

Verify your pods are running:
kubectl get pods -A

### 5. Install Helm for Jenkins
[Helm for Jenkins](5helm_for_jenki.png)

Install Helm package manager:
brew install helm

Add Jenkins repository:

helm repo add jenkins https://charts.jenkins.io
helm repo update

### 6. Deploy Jenkins

[Jenkins Pod Created](6sucessfully_jenkins_pod_created.png)

Create a values file for Jenkins configuration by directly update it by the helm and deploy:

helm update jenkins jenkinsci/jenkins \
-namespace jenkins \
--set controller.persistance.enabled=False \
--set controller.serviceType=LoadBalancer \
--set controller.admin.username=twilight \
--set controller.admin.password='iris'

Check Jenkins pod status:

kubectl get pods -n jenkins

### 7. Get AWS IP/ELB
[AWS IP](7AWS_ip.png)

Get the external IP for Jenkins service:

kubectl get svc -n jenkins

### 8. Deploy on Domain

[Deploy on Domain](8deploy_on_domain.png)
Configure DNS records to point your domain to the AWS Load Balancer IP.

### 9. Configure DNS Records

[Records](9records.png)
Set up the appropriate DNS records in A and ALIAS in Advanced DNS provider.

### 10. Lookup the IP
[Lookup IP](10lookup_the_ip.png)

Verify DNS resolution:

nslookup your-domain.com

## Successful Deployment
[Successfully Deployed](sucessfuly_deployed.png)

Once completed, you should have a fully functional Jenkins installation running on AWS EKS accessible via your custom domain.

## AWS CLI Installation

The `aws/` directory contains the AWS CLI v2 installation files. For installation instructions, see [AWS CLI README](aws/README.md).

## Accessing Jenkins

1. Get the initial admin password:
2. Access Jenkins through your browser using the configured domain
3. Complete the setup wizard
4. Remove the cluster by the same use of cmd eksctl

## Troubleshooting

- Ensure your AWS credentials are properly configured
- Check security group settings for access
- Verify PVC configurations
- Ensure proper IAM roles and permissions and region

## License
This project is for educational purposes.