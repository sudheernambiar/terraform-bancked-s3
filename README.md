# terraform-bancked-s3
Keep a backup in s3 for safety and collaboration.

## Prerequisites
- Script is for AWS environment.
- An EC2 instance with role of admin. 
- Terraform installed.
- Installed with packages tree, git for verification and collection of data.
- Need a terraform project directory with all necessary contents in it. 
- an S3 bucket with versioning enabled.

## Files Explained

### Overview
In a production environment we should need to make sure there won’t be any serious error happen, like deletion of statefile. Since the statefile keeps all the information about the deployment it is also prone to any human made errors. S3 bucket with versioning keeps each terraform.tfstate file(which generates a successful "terraform apply" command. This will help us to keep preventive measure if there is a mistaken file (specifically to the terraform.tfstate file) loss or something.

#### backend.tf
creates a s3 backend option which enables to keep store Terraform state files. (do not need any extra configurations for multiple workspaces)
```
#vim backend.tf
terraform {
  backend "s3" {
    bucket = "terraform-default-backup-060222"       # S3 bucket name with version enabled.
    key = "terraform.tfstate"                        # the file name 
    region = "ap-south-1"
    }
}
```
## Execution steps.
- terraform init (to initialise with the provider)
```
$ terraform init 
```
- To identify the procedure pre flight results
```
$ terraform plan 
```
- Execute the plan (with a yes you can permit after an overview, or explicitly work it with "terraform apply -auto-approve".
```
$ terraform apply 
```
- Incase to deploy without a pre flight check.
```
$ terraform apply -auto-approve 
```
Note: In case you want to make a clean-up use "terraform destroy"
```
$ terraform destroy
```
## Observations.
- Once you have committed for the first time, you will start notice your terraform.tfstate file will be inside the S3 bucket. If you own
- If you owns multiple workspaces, they also will come under .Env directory inside the S3 bucket. Under the .Env with workspace name you will be able to see terraform state files, which you used earlier with version enabled.
<img src="Untitled picture.png" width="auto" height="auto">
## Summary
For a better collaboration among different teams, we can use this kind of a setup. Since statefile is preserved with all information about the state of the deployment, it is also important to keep them safely. Versioning helps us to keep such a state preservation even though we have applied the latest one into production.
