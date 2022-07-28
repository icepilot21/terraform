# Terraform
## Exploring Terraform State Functionality

#### Check the Terraform status using the version command:
```
terraform version
```
#### Check the minikube status:
```
minikube status
```
### Clone the terraform code repo from github
```
git clone https://github.com/linuxacademy/content-hashicorp-certified-terraform-associate-foundations.git

cd lab_code/
cd section2-hol1
```
### Deploy Terraform Code And Observe the State File
#### Initialize the working directory and download the required providers:
```
terraform init
```
#### Review the actions that will be performed when you deploy the Terraform code
- In this case, it will create 2 resources as configured in the Terraform code.
```
terraform plan
```
#### List the files i the directory and deply the code
- Notice that the list of files does not include the terraform.tfstate at this time. You must deploy the Terraform code for the state file to be created.
```
ls
terraform apply
```
### Observe How the Terraform State File Tracks Resources
#### Once the code has executed successfully, list the files in the directory:
- Notice that the terraform.tfstate file is now listed. This state file tracks all the resources that Terraform has created.
```
ls
```
#### Optionally, verify that the pods required were created by the code as configured using kubectl:
- There are currently 2 pods in the deployment.
```
kubectl get pods
```
#### List all the resources being tracked by the Terraform state file using the terraform state command:
- There are two resources being tracked: kubernetes_deployment.tf-k8s-deployment and kubernetes_service.tf-k8s-service.
```
terraform state list
```
#### View the replicas attribute being tracked by the Terraform state file using grep and the kubernetes_deployment.tf-k8s-deployment resource:
- There should be 2 replicas being tracked by the state file.
```
terraform state show kubernetes_deployment.tf-k8s-deployment | egrep replicas
```
#### Open the main.tf file to edit it:
- Change the integer value for the replicas attribute from 2 to 4.
#### Review the actions that will be performed when you deploy the Terraform code:
- In this case, 1 resource will change: the kubernetes_deployment.tf-k8s-deployment resource for which we have updated the replicas attribute in our Terraform code.
```
terraform plan
```
#### Deploy the code again:
```
terraform apply
```
#### Optionally, verify that the pods required were created by the code as configured:
- There are now 4 pods in the deployment.
```
kubectl get pods
```
#### View the replicas attribute being tracked by the Terraform state file again:
- There should now be 4 replicas being tracked by the Terraform state file. It is accurately tracking all changes being made to the Terraform code.
```
terraform state show kubernetes_deployment.tf-k8s-deployment | egrep replicas
```
### When all is complete, destroy the AWS infrastructure
```
terraform destroy
```
#### List the files in the directory:
- Notice that Terraform leaves behind a backup file — terraform.tfstate.backup — in case you need to recover to the last deployed Terraform state.
```
ls
```