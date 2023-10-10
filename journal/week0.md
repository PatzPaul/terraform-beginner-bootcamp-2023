
# Terraform Beginner Bootcamp 2023 - Week 0️⃣:
 
- [Semantic Versioning 2.0.0](#semantic-versioning-200)
- [Install the Terraform CLI](#install-the-terraform-cli)
    * [Considerations with the Terraform CLI changes](#considerations-with-the-terraform-cli-changes)
    * [Considerations for Linux Distribution](#considerations-for-linux-distribution)
    * [Refactoring into Bash Scripts](#refactoring-into-bash-scripts)
        + [Shebang Considerations](#shebang-considerations)
        + [Execution Considerations](#execution-considerations)
        + [Linux Permissions Considerations](#linux-permissions-considerations)
- [Gitpod Lifecycle](#gitpod-lifecycle)
    * [Working Env Vars](#working-env-vars)
        + [Setting and Unsetting Env Vars](#setting-and-unsetting-env-vars)
        + [Printing Vars](#printing-vars)
        + [Scoping of Env Vars](#scoping-of-env-vars)
        + [Persisting  Env Vars in Gitpod](#persisting--env-vars-in-gitpod)
_ [AWS CLI Installation](#aws-cli-installation)
- [Terraform Basics](#terraform-basics)
    * [Terraform Registry](#terraform-registry)
    * [Terraform Console](#terraform-console)
        + [Terraform Init](#terraform-init)
        + [Terraform Plan](#terraform-plan)
        + [Terraform Apply](#terraform-apply)
        + [Terraform Destroy](#terraform-destroy)
        + [S3 Bucket Creation](#s3-bucket-creation)
        + [Terraform Lock Files](#terraform-lock-files)
        + [Terraform State Files](#terraform-state-files)
        + [Terraform Directory](#terraform-directory)
        + [Terraform Cloud login Issues and Gitpod workspaces](#terraform-cloud-login-issues-and-gitpod-workspaces)
        + [Terraform Set up for Terraform Alias](#terraform-set-up-for-terraform-alias)


## Semantic Versioning 2.0.0 

This project is going to utilize semantic versioning for its tagging 
[semver.org](https://semver.org/)

The general format:
**MAJOR.MINOR.PATCH**,eg. `1.0.1`


- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI

### Considerations with the Terraform CLI changes 
The Terraform CLI installation instructions have changed due to gpg keyring changes. So we needed to refer to the latest install CLI instructions via Terraform Documentation and change the scripting for install.

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli#install-cli)


### Considerations for Linux Distribution 
This project is built against Ubuntu.
Please consider checking your Linux distribution and change accordingly to distribution needs.

[How to Check OS Version in Linux](
https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)
Example of Checking OS version
```
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.3"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://buugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

### Refactoring into Bash Scripts

While fixing the Terraform CLI gpg depreciation issues we notice that bash scripts were a considerable amount more code. So we decided to create a bash script to install the Terraform CLI. 
This bash script is located here: ([./bin/install_terraform///-cli](./bin/install_terraform_cli))
- This will keep the Gitpod Task File([.gitpod.yml](.gitpod.yml)) tidy. 
- This allows us to easily debug and execute manually Terraform CLI install 
- This will allow better portability for other projects that need to install Terraform CLI.


#### Shebang Considerations
A Shebang (pronounced Sha-bang) tells the bash script what program that will interpret the script.
ChatGPT recommended this format for bash: `#!/usr/bin/env bash`

- for portability for different OS distributions 
- will search the user's PATH for the bash executable
https://en.wikipedia.org/wiki/Shebang_(Unix)

#### Execution Considerations
when executing the bash script we can use the `./` shorthand notation to execute the bash script.
eg. `./bin/install_terraform_cli`

If we are using a script in .gitpod/yml we need to point the script to a program to interpret it. eg.`source .bin/install_terraform_cli`

#### Linux Permissions Considerations 

In order to make our bash scripts executable we need to change linux permissions for teh fix to be executable at the user mode.

```sh
chmod u+x ./bin/install_terraform_cli
```

Alternatively:
```sh
chmod 744 ./bin/install_terraform_cli
```
https://en.wikipedia.org/wiki/Chmod

## Gitpod Lifecycle
(Before, Init, Command) we need to be careful when using the Init because it will not rerun if we restart an existing workspace.

https://www.gitpod.io/docs/configure/workspaces/tasks


### Working Env Vars 

We can list out all Environment Variables (Env Vars) Using the `env` command 

we can filter specific env vars using grep eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars 

In the terminal, we can set using `export HELLO='world'`

In the terminal, we unset using `unset HELLO`

We can set an env var temporarily when just running a command
```sh
HELLO='world' ./bin/print_message
``` 
Within a bash script, we can set env without writing export eg.

```sh

HELLO='world'
echo $HElLO
```

#### Printing Vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars 

When you open up new bash terminals in VSCode it will not be aware of env vars that you have set in another window 

If you want to Env Vars to persist across all future bash terminals that are open you need to set env vars in your `bash_profile` 

#### Persisting  Env Vars in Gitpod

We can persist env vars into Gitpod by storing them in Gitpod secrets storage.

```sh
gp env HELLO='world'
```

All future workspaces launched will set the Env Vars for all bash terminals opened in those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.


## AWS CLI Installation

AWS CLI is installed for the project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting Started Install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

[AWS CLI Env Vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials is configured correctly by running the following AWS CLI command:

```
aws sts get-caller-identity
```
If successful you should receive a json payload return that looks like this 

```json
{
"UserId": "AIDAY7SY8Y7X56XCK07W4",
"Account": "331972123423",
"Arn":"arn:aws:iam::331972123423:user/terraform-beginner-bootcamp"
}
```

we'll need to generate AWS CLI credentials from IAM user in order to use AWS CLI.

## Terraform Basics 

### Terraform Registry

Terraform sources their providers and modules from the terraform registry which is located at [registry.terraform.io](https://registry.terraform.io/)

- **Providers** is an interface to APIs that will enable us to create resources in terraform such as a storage bucket.

- **Modules** are a way to make large amount of terraform code modular, portable and sharable

[Random Terraform Provider](https://registry.terraform.io/providers/hashicorp/random)

### Terraform Console

We can see a list of all the Terraform commands by simply typing `terraform`

#### Terraform Init

At the start of a new terraform project we will run `terraform init` to download the binaries for the terraform providers that we'll use in this project 

#### Terraform Plan

This will generate out a changeset, about the state of our infastructure and what will be changed.
We can output this changeset ie. "plan" to be passed to an apply, but often you can just ignore outputting.

#### Terraform Apply 

`terraform apply`

This will run a plan and pass the changeset to be execute by terraform.Apply should prompt yes or no.

If we want to automatically approve and apply we can provide the auto approve flag eg. `terraform apply --auto-approve`

#### Terraform Destroy 
`terraform destroy`
This will destroy all resources such as recently generated storage buckets 

#### S3 Bucket Creation 

`S3 bucket`
When we tried generating a random bucket name we ran into an issue that needed resolving and the implementation was put on using lowercase`true` and making sure to reference no uppercase `false` just in case


#### Terraform Lock Files 
`.terraform.lock.hcl` contains the locked versioning for the providers or modules that should be used with this project.

The Terraform Lock File should be committed to your Version Control System (VSC) eg. Github

#### Terraform State Files

`.terraform.tfstate` contain information about the current state of your infrastructure.

This file **should not be commited** to your VCS.

This file can contain sensitive data 

If you lose this file, you lose knowing the state of your infrastructure.

`.terraform.tfstate.backup` is the previous state file state 

#### Terraform Directory

`.terraform` directory contains binaries of terraform providers.

#### Terraform Cloud login Issues and Gitpod workspaces 

When Attempting to run `terraform login` it will launch bash a wiswig view to generate a token however it is not functioning properly with Gitpod VsCode in the browser.

The workaround is manually generate a token in Terraform Cloud 


https://app.terraform.io/app/settings/tokens?source=terraform-login

Then create and open the file manually here:

```sh
touch /home/gitpod/.terraform.d/credentials.tfrc.json
open /home/gitpod/.terraform.d/credentials.tfrc.json
```
Provide the following code (replace your token in the file):

```json
{
    "credentials":{
        "app.terraform.io":{
            "token":"YOUR-TERRAFORM-CLOUD-TOKEN"
        }
    }
}
```

We have automated this workaround with the following bash script [bin/generate_tfrc_credentials](bin/generate_tfrc_credentials)

#### Terraform Set up for Terraform Alias

When running the `terraform` command we want to use an alias `tf` by setting it in the .bash_profile

Create a bash file `set_tf_alias` in the `./bin` directory to handle this when running workspace
```sh
#!/usr/bin/env bash

# Check if the alias already exists in the .bash_profile
grep -q 'alias tf="terraform"' ~/ .bash_profile

# $? is a special variable in bash that holds the exit status of the last 

if [ $? -ne 0]; then
    # if the alias does not exist append it 
    echo 'alias tf="terraform"' >> ~/ .bash_profile
    echo "Alias added successfully"
else
    # Inform the user if the alias already exists 
    echo "Alias already exists in .bash_profile."
fi

source ~/ .bash_profile
```
Proceed to make the file executable 
```sh
chmod u+x ./bin/set_tf_alias
```
then proceed to add the following in `gitpod.yml` file

```yml
source ./bin/set_tf_alias
```
