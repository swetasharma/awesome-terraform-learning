# Terraform

1. `terraform init` Pulls down the specific providers that we need for project you can always see the providers in the .terraform directory, under plugins and there is executable for it. .terraform directory should not be committed to the git repository.
```
provider "google" {
  credentials = "${file("../account.json")}"
  project = "testsweta567"
  region = "us-west1"
}

provider "aws" {
  region = "us-west-2"
}

provider "azurerm" {
  subscription_id = "0"
  client_id = "1"
  client_secret = "2"
  tenant_id = "3"
}

resource "google_compute_network" "our_development_network" {
  name = "devnetwork"
  auto_create_subnetworks = true
}

resource "aws_vpc" "environment-example-two" {
  cidr_block = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support = true
  tags {
    Name = "terraform-aws-vpc-example-two"
  }
}
```

2. `terraform plan` Shows diff between existing infrastructure and what state we wanted to be in based on our configuration.
3. `terraform apply` runs the diff.

## How to secure deployments
- We cannot deploy from our machine there should be some pipeline which should take of this we need to secure that state file as we need to secure our cloud.
we need to put out state file into storage account and lock down security case to it and access to it
There should be a script file which will tell state file not to write locally write to a storage account
*state file is very important keep safe*
terraform.tfvars
State file is gonna sit inside azure storage, we also have instance of key vault.
The key vault is securing my secrets for the storage accounts.
We take the key that we get when we create storage account and we insert into key vault and keep your cloud secure. It is not open and available to anyone in the organizations 
They have access to Azure however they dont have access to that resource
you can lock it down except for the processes and person needs to.
## Terraform modules deploying reusable code
The power of terraform lies in variables, modules and workspaces.
you can think of a module as a seperate funtion or deploybale unit because main.tf file gets very long amd complex for big environments
it meakes it repeatable and redeployable not just one data center one environment, multiple environment
it is very much like calling a method. Call module in this case and tell terraform to deploy that
We add resource to the module. Add it module by module and take it away module by module
Do they have same environments for dev test and production? Manage them seperatly but have the same
infrastructure and we have three different state files



## How to use terraform in your pipelines
Everything works straight through a command line tool,
which mean I can easily easily embed this to my CI/CD pipeline.
If you have infrastructure as code written in terraform to put it into the pipeline, run terraform tasks and when that done you deploy your application using infrastructure.
provision infrastructure thourgh my pipeline using terraform.
we can deploy on prem, we can deploy other cloud, we can deploy microsoft azure with ease
Everything in pipeline starts from the build. I checkin code and cick off the build.
changes to code -> cick of the build cycle -> 
In release pipeline it will change the infrastructure first and then deploy the code.
Steps of pipeline: you can automate all the below steps in your pipeline.
1. Create storage account and the key which wiil store terraform state file.
2. We need to secure the storage account keys in key vault. It will get the storage key and put them in key vault. you dont have to release the key to anyone, secured and it automates the whole thing
3. Replace token in terraform file.
4. Install Terraform: Remember when your terraform version change your code has to change
5. Terraform : init
6. Terraform : plan
7. Terraform : Apply - auto-approve
we have to login into the portal, we have to find different ways to allow authentication into our portal to deploy those terraform.
