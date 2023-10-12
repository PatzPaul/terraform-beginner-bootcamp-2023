# Terraform Beginner Bootcamp 2023 - Week 1️⃣:

## Fixing tags
[How to delete local and remite tags on Git](https://devconnected.com/how-to-delete-local-and-remote-tags-on-git/)

locally delete a tag
```sh
git tag -d <tag_name>
```
Remotely delete a tag
```sh
git push --delete origin tagname
```

Checkout the commit you want to retag, grab the sha from the Github history

```sh
git checkout <SHA>
git tag M.M.P
git push --tags
git checkout main
```

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

```sh
terraform -var user_ud="my-user_id"
```
### var -file flags 
The -var-file flag allows us to specify a file containing variable definitions. This is useful when you have a set of variables that you want to load from a file. Eg

```sh
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

## Fix using Terraform Refresh

```sh
terraform apply -refresh-only -auto-approve
```

## Terraform Modules

### Terraform Module Structure

It is recommended to place modules in a `modules` directory when locally developing modules but you can name it whatever

### Passing Input Variables 

We can pass input variables to our module.
The module has to declare the terraform variables in its own variables.tf 

```tf
module "terrahouse_aws" {
  source = "./modules/terrahouse_aws"
  user_uuid = var.user_uuid
  bucket_name = var.bucket_name
}
```

### Modules Sources

Using the source we can import the module from various places eg:
- locally 
- Github
- Terraform Registry

```tf
module "terrahouse_aws" {
  source = "./modules/terrahouse_aws"
}
```

[Modules Sources](https:developer.hashicorp.com/terraform/language/modules/sources)

## Considerations when using ChatGPT to write Terraform

LLMS SUCH AS ChatGPT may not be trained on the latest documentation or information about Terraform.
Meaning it may likely produce older examples that could be depreciated. Often affecting providers.

## Working with files in Terraform

### Fileexists function 
This is built in terraform function to check the existance of a file 

```tf
condition = fileexists(var.error_html_filepath)
```
https://developer.hashicorp.com/terraform/language/functions/fileexists

### Filemd5

https://developer.hashicorp.com/terraform/language/functions/filemd5

### Path Variables
In terraform there is a special variable called `path` that allows us to reference local paths:
- path.module = get the path from the current module 
- path.root = get the path for the root module 
[Special Path Variable](https://developer.hashicorp.com/terraform/language/expressions/references#filesystem-and-workspace-info)

```tf
resource "aws_s3_object" "index_html" { 
  bucket = aws_s3_bucket.website_bucket.bucket 
  key = "index.html" 
  source = "${path.root}/public/index.html"
   }
```



## Terraform Locals
Locals allows us to define local variables and it can be very useful when we need to transform data into another format and have referenced a variable.

```tf
locals {
  s3_origin_id ="MyS3Origin"
}

```

[Local Values](https://developer.hashicorp.com/terraform/language/values/locals)


## Terraform Data Sources 
This allows us to source data from cloud resources.

This is useful when we are referencing cloud resources when we are importing them 

```tf
data "aws_caller_identity" "current" {}

output "account_id" {
  value =data.aws_caller_identuty.current-account_id
}
```

[Data Sources](https://developer.hashicorp.com/terraform/language/data-sources)

## Working with json 

We use the json encode to create the json polict inline with the hcl.


```tf
> jsonencode({"hello"="world"})
{"hello":"world"}
```

jsonencode 
[jsonencode](https://developer.hashicorp.com/terraform/language/functions/jsonencode)

## Changing the Lifecycle of Resources
[Meta Arguments Lifecyle](https://developer.hashicorp.com/terraform/language/meta-arguments/lifecyle)

## Terraform Data

Plain data values such as local values and input variables don't have any side-effects to plan against and so they aren;t valid in replace_triggered_by. You can use terraform_data's behavior of planning an action each time input changes to indirectly use a plain value to trigger replacement.
https://developer.hashicorp.com/terraform/language/resources/terraform-data