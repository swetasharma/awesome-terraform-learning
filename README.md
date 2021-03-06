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

resource "aws_vpc" "environment-example-two" {
  cidr_block = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support = true
  tags {
    Name = "terraform-aws-vpc-example-two"
  }
}

resource "aws_subnet" "subnet1" {
  cidr_block = "${cidrsubnet(aws_vpc.environment-example-two.cidr_block, 3, 1)}"
  vpc_id = "${aws_vpc.environment-example-two.id}"
  availability_zone = "us-west-2a"
}

resource "aws_subnet" "subnet2" {
  cidr_block = "${cidrsubnet(aws_vpc.environment-example-two.cidr_block, 2, 2)}"
  vpc_id = "${aws_vpc.environment-example-two.id}"
  availability_zone = "us-west-2b"
}

resource "aws_security_group" "subnetsecurity" {
  vpc_id = "${aws_vpc.environment-example-two.id}"
  ingress {
    cidr_blocks = [
      "${aws_vpc.environment-example-two.cidr_block}"
    ]
    from_port = 80
    protocol = "tcp"
    to_port = 80
  }
}

```

2. `terraform plan` Shows diff between existing infrastructure and what state we wanted to be in based on our configuration.
3. `terraform apply` runs the diff.
4. `terraform validate`
5. `terraform format`
6. `terraform state`
7. `terraform graph` for this command install graphviz, this is great little tool to see how everything is literally related to each other with a graphical context.
8. `terraform output` Works with pulling an output variable and returning the assigned value to that output variable based on the state terraform import

## How to secure deployments
- We cannot deploy from our machine there should be some pipeline which should take care of this we need to secure that state file as we need  to secure our cloud.
- We need to put out state file into storage account and lock down security case to it and access to it.
- There should be a script file which will tell state file not to write locally write to a storage account *state file is very important keep safe*
- terraform.tfvars
- State file is gonna sit inside azure storage, we also have instance of key vault.
- The key vault is securing my secrets for the storage accounts.
- We take the key that we get when we create storage account and we insert into key vault and keep your cloud secure. It is not open and available to anyone in the organizations. They have access to Azure however they dont have access to that resource, you can lock it down except for the processes and person needs to.
- store your state in a secured location such as an S3 bucket where the state files are encrypted at rest.You store your Terraform configs in a git repo and the state in a remote backend

## Terraform modules deploying reusable code
- The power of terraform lies in variables, modules and workspaces.
- You can think of a module as a seperate function or deploybale unit because main.tf file gets very long amd complex for big environments. It meakes it repeatable and redeployable not just one data center or one environment, multiple environment
- It is very much like calling a method. Call module in this case and tell terraform to deploy that.
- We add resource to the module. Add it module by module and take it away module by module.
- Do they have same environments for dev test and production? Manage them seperatly but have the same infrastructure and we have three different state files.

## How to use terraform in your pipelines
- Everything works straight through a command line tool, which mean I can easily easily embed this to my CI/CD pipeline.
- If you have infrastructure as code written in terraform to put it into the pipeline, run terraform tasks and when that done you deploy your application using infrastructure.
- Provision infrastructure through my pipeline using terraform.
- We can deploy on prem, we can deploy other cloud, we can deploy microsoft azure with ease
- Everything in pipeline starts from the build. I checkin code and cick off the build.
- In release pipeline, it will change the infrastructure first and then deploy the code.
- Steps of pipeline: you can automate all the below steps in your pipeline.
1. Create storage account which wiil store terraform state file.
2. We need to secure the storage account keys in key vault. It will get the storage key and put them in key vault. you dont have to release the key to anyone, secured and it automates the whole thing.
3. Replace token in terraform file.
4. Install Terraform : Remember when your terraform version change your code has to change
5. ```Terraform init```
6. ```Terraform plan```
7. ```Terraform : Apply - auto-approve```
- We have to login into the portal, we have to find different ways to allow authentication into our portal to deploy those terraform.





### Terraform Commands:

IAM - Create Users: this is the account which terraform will use to call aws / usrename and password for the user in terms  of api
build config file based on csv file we downloaded
code to create s3 bucket
download and install the additional terraform plugin it needs for working with aws


Terraform is infrastructure management tool
code state plan

terraform uses provider to apply the plan
directed acrylic graph
s3->dns->ec2

1. `terraform plan --help`
2. `terraform plan`
3. `terraform plan -destroy`
4. `terraform plan -destroy -out=example.plan`
5. `terraform show example.plan`
6. `terraform.tfstate  - all the infrastructure`
7. `terraform state`
8. `terraform state show aws_security_group.prod_web`
9. `terraform show -json`
10. `terraform graph`


aws security group can function as a firewall

Terraform style:
2 spaces rather than a tab
component environment region and whatever descriptor make sense
networking-prod-us-west-aws-
