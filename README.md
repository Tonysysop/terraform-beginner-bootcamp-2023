# Terraform Beginner Bootcamp 2023

# Week- 0 

The terraform installation script within the gitpod.yaml file encountered an issue. The terraform task in gitpod.yaml reached a point where it required user input. The objective is to ensure that the script runs smoothly without any need for user interaction.

below is the yaml task.

```yaml

tasks:
  - name: terraform
    init: |
      sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
      curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
      sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
      sudo apt-get update && sudo apt-get install terraform
```
The script also had  some key deprecation which was also an issue in the during the installation.

## Reinstalltion Terraform with the Latest Version.

Going through the the terraform installation instruction which can be found in (terraform.io) a new set of installation guild was found.

## Install Terraform CLI

### Considerations with the Terraform CLI Changes

The terraform CLI Installation have changed due to a gpg keystring being depreciated so i had to refer to the terraform cli installation guild to see the latest installtions guild

[Install Terraform ClI](https://developer.hashicorp.com/terraform/downloads)


### Consideration for Linux Distribution
This project is built against Ubuntu.
Please consider checking your linux distribution and change according to your distribution needs.
[How to Check OS Version In Linux](https://www.tecmint.com/check-linux-os-version/#:~:text=The%20best%20way%20to%20determine,on%20almost%20all%20Linux%20systems.)

### Refactoring into bash scripts.
While fixing the terraform gpg deprecation issues i noticed the new terraform installation bash-script had a considerable amount of 
more code so we had to pass it as a bash script called "Install_terraform_cli" 

This bash script can be found here : [install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod.yaml task file ([.gitpod.yaml](./gitpod.yaml)) tidy
- This will make it easier for me to debug and execute the terraform cli script
- This will allow better portability for other project that need to install terraform cli

#### Shebang
A shebang (pronounce sha-bang) tells the bash script what program to use when interpretting the script eg : `#!/usr/bin/bash`

ChatGpt Recommended this format for bash: `#!/usr/bin/env bash`

- for portability for different OS distribution
- Will search user's PATH for the base executable

#### Executuion Considerations.
WHen executing a bash script we can use `./` shorthand notation to execute the bash script.
e.g `./bin/install_terraform_cli`
If we are using a script in gitpod.yam, we need to point thwe script to a program that can interprete it.
e.g `source ./bin/install_terraform_cli`

#### Linux Permission COnsiderations

In order to make the bash script executable, we need to change linix permission for the script to be executable at the user's level.

```sh
chmod u+x ./bin/install_terraform_cli
```

we could also alternatively :
```sh
chmod 744 ./bin/install_terraform_cli
```

https://en.wikipedia.org/wiki/chmod

### Gitpod lifecycle (Before, Init, command)

We need to be careful when using the Init because it will not run if we restart an existing workspace. 

https://gitpod.io/docs/configure/workspaces/tasks


### Working with Env Vars

we can list out all environment variables (ENV Vars) using the `env` command 

We can filter specific env using grep eg. `env | grep AWS_`

#### Setting and Unsetting Env VArs

in the terminal we can set env using `export HELLO='World'`

In the terminal we can unset an env using : `unset HELLO`

we can set an env var temporarily when running a command 

```sh
HELLO='World' ./bin/print_message
```

With a bash script we can set env without writting export eg .

```sh
#!/usr/bin/env bash

HELLO='World'

echo $HELLO
```
#### Printing Env Vars

We can print an env var using echo  eg. `echo $HELLO`

#### Scoping OF Env Vars

When you open up new bash terminals in VSCODE it will not be aware of env vars that you have set in another window.

if you want env var to persist accross future bash terminal that are open you would need to set the env vars inside your bash profile eg . `.bash_profile`

#### Persisting ENv in Gitpod

We can persist env vars in gitpod by storing them in gitpod secret storage.

```sh
gp env HELLO='World'
```
All future workspaces launched will set the env vars for all the bash terminal opennin those workspaces 

You can also set env in the `.gitpod.yam` but this can only contain non-sensitive.

