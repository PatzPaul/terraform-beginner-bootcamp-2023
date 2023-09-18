# Terraform Beginner Bootcamp 

## Semantic Versioning 2.0.0 :mage:

This project is going to utilize semantic versioning for its tagging 
[semver.org](https://semver.org/)

the general format:
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
- This will keep the Gitpof Task File([.gitpod.yml](.gitpod.yml)) tidy. 
- This allows us to easily debug and execute manually Terraform CLI install 
- This will allow better portability for other projects that need to install Terraform CLI.


### Shebang 
A Shebang (pronounced Sha-bang) tells the bash script what program that will interpret the script.
ChatGPT recommended this format for bash: `#!/usr/bin/env bash`

- for portability for different OS distributions 
- will search the user's PATH for the bash executable
https://en.wikipedia.org/wiki/Shebang_(Unix)

## Execution Considerations
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

### Github Lifecylce(BEfore, Init, Command)
we need to be careful when using the Init because it will not rerun if we restart an existing workspace.

https://www.gitpod.io/docs/configure/workspaces/tasks


### Working Env VArs 

We can list out all Environment Variables (Env Vars) Using the `env` command 

we can filter specific env vars using grep eg. `env | grep AWS_`

### Setting and Unsetting ENV Vars 

In the terminal we can set using `export HELLO='world'`

In the terminal we unset using `unset HELLO`

We can set an env var temporarily when just running a command
```sh
HELLO='world' ./bin/print_message
``` 
Within a bash script we can set env without writing export eg.

```sh

HELLO='world'
echo $HElLO
```

### Printing Vars

We can print an env var using echo eg. `echo $HELLO`

### Scoping of Env Vars 

When you open up new bash terminals in VSCode it will not be aware of env vars that you have set in another window 

If you want to Env Vars to persist accross all future bash terminals that are open you need to set env vars in your `bash profile` 

### Persisting  Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod secrets storage.

```sh
gp env HELLO='world'
```

All future workspaces launched will set the Env Vars for all bash terminals opened in those workspaces.