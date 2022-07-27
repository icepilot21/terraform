# Terraform
## Installing Terraform and Working with Terraform Providers 

## Install Terraform
NOTE, this is for Amazon Linux, which is similar to CentOS, RHEL, and Fedora
https://releases.hashicorp.com/terraform/
- Edit the commands below for the version you are installing
```
wget -c https://releases.hashicorp.com/terraform/0.13.4/terraform_0.13.4_linux_amd64.zip
unzip terraform_0.13.4_linux_amd64.zip
sudo mv terraform /usr/sbin
terraform version
```

## Create a directory for providers
- mkdir providers
- cd providers/
- vim main.tf
```
provider "aws" {
  alias  = "us-east-1"
  region = "us-east-1"
}

provider "aws" {
  alias  = "us-west-2"
  region = "us-west-2"
}


resource "aws_sns_topic" "topic-us-east" {
  provider = aws.us-east-1
  name     = "topic-us-east"
}

resource "aws_sns_topic" "topic-us-west" {
  provider = aws.us-west-2
  name     = "topic-us-west"
}
```
### Enable verbose logging for terraform commands
```
export TF_LOG=TRACE
```
### Initialize
```
terraform init
terraform fmt
terraform plan
terraform apply
```
### Verify that the services started in AWS, then destroy the infrastructure
```
terraform destroy
```