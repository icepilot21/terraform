# Terraform
## Building and Testing a Basic Terraform Module

#### Ensure terraform is installed
```
terraform version
```
#### Make a directory for the terraform projects
```
mkdir terraform_project
cd terraform project
```
#### Create a custom directory called modules and a directory inside it called vpc:
```
mkdir -p modules/vpc
cd modules/vpc/
```
#### Create a new file called main.tf
```
provider "aws" {
  region = var.region
}

resource "aws_vpc" "this" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "this" {
  vpc_id     = aws_vpc.this.id
  cidr_block = "10.0.1.0/24"
}

data "aws_ssm_parameter" "this" {
  name = "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2"
}
```
#### Create a new file called variables.tf
```
variable "region" {
  type    = string
  default = "us-east-1"
}
```
#### Create a new file called outputs.tf
```
output "subnet_id" {
  value = aws_subnet.this.id
}

output "ami_id" {
  value = data.aws_ssm_parameter.this.value
}
```
### Write your main terraform code
```
cd ~/terraform_project
```
#### Create a new file called main.tf
```
variable "main_region" {
  type    = string
  default = "us-east-1"
}

provider "aws" {
  region = var.main_region
}

module "vpc" {
  source = "./modules/vpc"
  region = var.main_region
}

resource "aws_instance" "my-instance" {
  ami           = module.vpc.ami_id
  subnet_id     = module.vpc.subnet_id
  instance_type = "t2.micro"
}
```
#### Create a new file called outputs.tf
```
output "PrivateIP" {
  description = "Private IP of EC2 instance"
  value       = aws_instance.my-instance.private_ip
}
```
### Deply your code and test your module
#### Format the code in all of your files in preparation for deploymen
```
terraform fmt -recursive
```
#### Initialize the Terraform configuration to fetch any required providers and get the code being referenced in the module block:
```
terraform init
```
#### Validate the code to look for any errors in syntax, parameters, or attributes within Terraform resources that may prevent it from deploying correctly:
```
terraform validate
```
#### Review the actions that will be performed when you deploy the Terraform code:
- In this case, it will create 3 resources, which includes the EC2 instance configured in the root code and any resources configured in the module. If you scroll up and view the resources that will be created, any resource with module.vpc in the name will be created via the module code, such as module.vpc.aws_vpc.this.
```
terraform plan
```
#### Deploy the code:
```
terraform apply --auto-approve
```
- Once the code has executed successfully, note in the output that 3 resources have been created and the private IP address of the EC2 instance is returned as was configured in the outputs.tf file in your main project code.
#### View all of the resources that Terraform has created and is now tracking in the state file:
```
terraform state list
```
### After validating that the resaources have been built in AWS, destroy the deployment
```
terraform destroy
```