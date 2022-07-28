# Terraform
## Using Terraform CLI Commands (workspace and state) to Manipulate a Terraform Deployment

#### List current workspaces
```
terraform workspace list
```
#### Create a new workspace
```
terraform workspace new <NAME>
```
#### Initialize terraform working directory
```
terraform init
```
#### View the providers for the deployment
```
cat main.tf
```
#### View the network configs
```
cat network.tf
```
#### Deploy the infastructure
```
terraform apply --auto-approve
```
#### View the resources that should have been deployed to AWS using terraform
```
terraform state list
```
#### Switch to the default workspace and look for the resources that have been built here
```
terraform workspace select default
terraform state list
```
#### Log into AWS and confirm the resources that were deployed
#### Deploy the infrastructure from the default workspace
```
terraform init
terraform apply --auto-approve
terraform state list
```
#### Log into AWS and confirm the resources that were deployed
#### On the server, you can view all of the terraform workspace state files in terraform.tfstate.d
```
ls
cd terraform.tfstate.d
```
### Delete the resources that have been deployed
```
terraform workspace select test
terraform destroy --auto-approve
terraform workspace select default
terraform destroy --auto-approve
```