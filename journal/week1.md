# Terraform Beginner Bootcamp 2023 - Week 1️⃣:
<<<<<<< HEAD
=======

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
>>>>>>> 20-create-table-of-contents-for-markdown
