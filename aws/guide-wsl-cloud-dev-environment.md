# How to use WSL for cloud development
## How to set up
### Prerequisites
- OPTIONAL: install AWS CLI on Windows (you won't really need it on Windows, but there a some use cases where it is nice to have)
	- https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
- set up your AWS credentials
	- [credentials-management](credentials-management.md)
	- you can do this using IntelliJ (and the AWS Toolkit plugin) or any other text-editor (just create/edit the file `C:\Users\<your windows user name>/.aws/config`)
	- an initial config file could look like this:
```
[default]
sso_start_url = https://<some subdomain>.awsapps.com/start/
sso_region = eu-central-1
sso_account_id = <an account id>
sso_role_name = AWSAdministratorAccess
region = eu-central-1

[profile sandbox]
sso_start_url = https://<some subdomain>.awsapps.com/start/
sso_region = eu-central-1
sso_account_id = <another account id>
sso_role_name = AWSAdministratorAccess
region = eu-central-1

[profile dev]
sso_start_url = https://<some subdomain>.awsapps.com/start/
sso_region = eu-central-1
sso_account_id = <yet another account id>
sso_role_name = AWSAdministratorAccess
region = eu-central-1
```
- install WSL
	- https://learn.microsoft.com/de-de/windows/wsl/install
	- this guide assumes that you are using an Ubuntu distro for your WSL
	- there may be some differences if you use any other distro
	- run `sudo apt update` and `sudo apt upgrade`
	- get yourself nala (Optional)
		- https://github.com/volitank/nala
		- `sudo apt install nala`
		- its an front-end for `libapt-pkg` and makes it easier for newer users
		- now you can run run `sudo nala install ...` instead of `sudo apt install ...`
		- you can add the following lines to `.bash_aliases`, so that using `apt` will automatically use `nala` instead: (Optional)
```bash
alias sudo="sudo "
alias apt="nala"
```

### Set up AWS CLI
- first of all you will need unzip (if you have a fresh WSL), so run `sudo apt install unzip`
- install latest version of AWS CLI in WSL
	- https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
- synch all your AWS settings from Windows to your WSL using:
	- `ln -s /mnt/c/Users/<your windows user name>/.aws/ ~/`

### Set up ssh
- create or update using `sudo nano /etc/wsl.conf` and add the folowing code:
```bash
[automount]
options = "metadata,umask=022,fmask=111"
```
- close all active WSL sessions (use `wsl --shutdown` in PowerShell)
- open your WSL again
- symlink your ssh keys over to the WSL
	- `ln -s /mnt/c/Users/<your windows user name>/.ssh/ ~/`
- update permissions by running the following commands:
	- `chmod 700 ~/.ssh/`
	- `chmod 600 ~/.ssh/*

### Edit .bashrc
- open and edit your `.bashrc` file using `sudo nano .bashrc`
- optional: customize your command prompt
```bash
# displays the profile name
function get_aws_profile {
  local profile="${AWS_PROFILE:=default}"

  echo "(${profile})"
}

# alternatively you can display the account id
function get_aws_account {
    aws sts get-caller-identity --query 'Account' --output text
}

# add $(get_aws_profile) to PS1
# use [\033[00;33m\]$(get_aws_profile) to add some color
# example:
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00;33m\]$(get_aws_profile)\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

# you may also want your prompt to show your current git branch
get_git_branch() {
 git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

# add [\033[00;33m\]$(get_git_branch)
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00;33m\]$(get_aws_profile)\[\033[00m\]:\[\033[01;34m\]\w\[\033[00;33m\]$(get_git_branch)\[\033[00m\]\$'
```
- run `source ~/.bashrc`
- while setting up asdf-vm you will have to edit `.bashrc` again

### Set up asdf-vm
- install asdf
	- https://asdf-vm.com/guide/getting-started.html
- add asdf plugins
	- https://asdf-vm.com/manage/plugins.html
	- list of all plugins: https://github.com/asdf-vm/asdf-plugins
	- you will need to add (at least) those plugins (and their dependencies):
		- Terraform
		- Terraform-docs
		- TFLint
		- TFSec
		- Terragrunt
		- pre-commit
- e.g. add and install Terraform using asdf
	- add the plugin `asdf plugin add terraform
	- install a version `asdf install terraform latest`
- set up a global .tool-versions file (Optional)
	- https://asdf-vm.com/guide/getting-started.html#global
	- you can do this by simply adding running `asdf global terraform latest`

### Set up awsp (Optional)
- install awsp to quickly switch profiles
	- https://github.com/abyss/awsp-plus
	- you will need Node.js (and npm) for this
		- `sudo apt install nodejs npm`
		- (in a perfect world you would install this using asdf-vm, but in this world this won't work as installing stuff using npm won't properly work)
	- `npm install -g awspp`
	- awsp works fine if you only use sso logins, if you have to manage credentials you may want to have a look into
	- create/edit `.bash_aliases` and add an alias to quickly run awsp
```bash
alias awsp="source _awspp"
```

### Configure WSL-browser
- Links won't work in the WSL
- so when you don't want to copy&paste them every time you will need this
- install wslu
	- https://wslutiliti.es/wslu/install.html#ubuntu
- add this env-variable so that the WSL opens links from the WSL with your Windows standard browser
```bash
export BROWSER=wslview
```

### Configure IntelliJ (Optional)
- https://www.jetbrains.com/help/idea/how-to-use-wsl-development-environment-in-product.html
- Setttings > Tools > Terminal > Shell path
- set to `wsl.exe`
- install AWS Toolkit plugin  (Optional)
	- https://aws.amazon.com/de/intellij/
	- https://plugins.jetbrains.com/plugin/11349-aws-toolkit
	- (you may also want to edit the AWS Toolkit settings, so that it is not possible to open a terminal with env variable injection, as this won't work properly with the wsl)
- install the Nyan Progress Bar plugin (Optional)
	- https://plugins.jetbrains.com/plugin/8575-nyan-progress-bar
	- (its optional, but you will really miss out on something if you don't install it)

## How to use
### Cloning a repo
- use git (and ssh) to clone an existing project into your WSL
- you probably want to create a new directory for this using `mkdir <directory name>`
	- e.g. `mkdir projects`
	- navigate into you folder using `cd <directory name>`
- clone project `git clone git@<repo-url>`
- navigate into project directory
	- `ls` to show current directories content
	- `cd <project directory>`

### Open in IntelliJ
- you will find your projects in `/wsl$/Ubuntu/home/<wsl user name>/projects/`
- open it as you would do it with any other project

### Installing tool versions
- navigate into the project directory
- install the tools you will need using asdf-vm (they are stored in `.tools-version`)
	- `asdf install`

## Known bugs - and how to fix them
- Error when trying to run AWS commands from WSL
	- e.g.: `Signature expired: 20170517T062414Z is now earlier than 20170517T062840Z (20170517T063340Z - 5 min.)`
	- simply run `sudo hwclock -s`