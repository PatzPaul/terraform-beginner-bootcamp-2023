# Terraform Beginner Bootcamp 2023 - Week 1️⃣:

## Root Module Structure 

Our root module structure is as follows:
```
PROJECT_ROOT
│
├── main.tf                 # everything else.
├── variables.tf            # stores the structure of input variables
├── terraform.tfvars        # the data of variables we want to load into our terraform project
├── providers.tf            # defined required providers and their configuration
├── outputs.tf              # stores our outputs
└── README.md               # required for root modules
```
[Standard Module Structure](https://developer.hashicorp.com/terraform/language/modules/develop/structure)

## Terraform and input variables 

### Terraform Cloud Variables


In Terraform, we can set two kinds of variables:
- **Environment Variables:** These are set in your bash terminal, e.g., AWS credentials.
- **Terraform Variables:** These are typically set in your `tfvars` file.

We can set Terraform Cloud Variables to be sensitive so they are not shown visibly in the UI.


### Loading Terraform Input Variables 
[Terraform Input Variables](https://developer.hashicorp.com/terraform/language/values/variables)

### var flags 
We can use the `-var` flag to set an input variable or override a variabe in the tfvars file eg.

```bash
terraform -var user_ud="my-user_id"
```
### var -file flags 
The -var-file flag allows us to specify a file containing variable definitions. This is useful when you have a set of variables that you want to load from a file. Eg

```bash
terraform plan -var-file="path/to/variables.tfvars"
```

### terraform.tfvars
This is the default file to load in terraform variables in bulk

### auto.tfvars
This file is another way to automatically load variable values. <strong>Terraform</strong> automatically loads variables from a file named auto.tfvars if it exists in the current directory. It's useful for providing default values for variables.
 
### order of terraform variables 

- TODO: document which terraform variables takes presendence

## Dealing with Configuration Drift 

## What happens if we lose our state file?

If you lose your statefile, you most likely have to tear down all your cloud infastructure manually.

You can use terraform port but won't for all cloud resources. You need check the terraform providers documentation for which resourcs support import.

### Fix Missing Resources with Terraform Import
[Terraform Import](https://developer.hashicorp.com/terraform/cli/import)

### Fix Manual Configuration

If Someone goes and deletes or modifies cloud resource manually through ClickOps

If we run Terraform plan it will attempt to put our infastructure back into the expected state fixing configuration Drift

