# terraform
Terraform

Terraform: Everything works straight through a command line tool,
which mean I can easily easily embed this to my CI/CD pipeline
How to secure deployments...we canot deply form our machine there should be some pipelien which should 
take of this we need to secure that state file as we need to secure our cloud
we need to put out state file into storage account and lock down secuity case to it and access to it
There should be a script file which will tell state file not to write locally write to a storage account
state file is very important keep safe
terraform.tfvars
State file is gonna sit inside azure storage, we also have instance of key vault
The key vault is securing my secrets for the storage accounts
We take the key that we get when we create storage account and we insert into key vault and keep your 
cloud secure. It is not open and available to anyone in the organizations 
They have access to Azure however they dont have access to that resource
you can lock it down except for the processes and person needs to 
Terraform modules deploying reusable code: you can think of a module as a seperate funtion or
deploybale unit because main.tf file gets very long amd complex for big environments
it meakes it repeatable and redeployable notnujst one data center one environment, multiple environment
it is very much like calling a method  call module in this case and tell terraform to deploy that
We add resource to the module
add it module by module and take it away module by module
DO they have same environments for dev test and production? Manage them seperatly but have the same
infrastructure and we have three different state files
The power of terraform lies in variables, modules and workspaces
