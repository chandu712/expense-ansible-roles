# Ansible roles

It came from DRY(Don't repeat yourself) principle

- Variables
- functions
to arrange the code in proper structure without repeating, we use Ansible roles.

What is Ansible role?

Role is a proper ansible structure of variables, tasks, templates,handlers etc, using roles, we can reuse the code.

Roles let you automatically load related vars, files, tasks, handlers, and other Ansible artifacts based on a known file structure. After you group your content into roles, you can easily reuse them and share them with other users.

eg: create a folder with name roles, subfolder with name db(which is name of the role eg: roles-->db)

- tasks/  ---> we will mention all tasks here.
- handlers/  ---> we will mention all notifiers here
- templates/  ---> we will mention all templates here with variables
- files/ ---> we will mention all files here without variables
- vars/ ---> Variables related to roles are mentioned here
- defaults/ ---> low priority variables are here
- meta/  ---> dependencies are mentioned here
- library/          # roles can also include custom modules
- module_utils/     # roles can also include custom module_utils
- lookup_plugins/

If we are regularly using one part of code and don't have ansible modules or support from ansible, we can develop customised modules by using python. customised modules can be included under library/, module_utils/, lookup_plugins.

To execute ansible playbook, we need to run below command

ansibel-playbook -i inventory -e ansible_user=ec2-user -e ansible_password=devOps321 db.yaml

Instead of writing above command multiple times, we can use ansible config
every software is having configuration, the same way ansible also have ansible config.


Ansible supports several sources for configuring its behavior, including an ini file named ansible.cfg, environment variables, command-line options, playbook keywords, and variables. See Controlling how Ansible behaves: precedence rules for details on the relative precedence of each source.

The ansible-config utility allows users to see all the configuration settings available, their defaults, how to set them and where their current value comes from. See ansible-config for more information.

## The configuration file

Changes can be made and used in a configuration file which will be searched for in the following order:

ANSIBLE_CONFIG (environment variable if set)

ansible.cfg (in the current directory)

~/.ansible.cfg (in the home directory)



Ansible will process the above list and use the first file found, all others are ignored.

ansible --version ----> we can see from where configuration is getting loaded(config file).
config file = /etc/ansible/ansible.cfg
cp /etc/ansible/ansible.cfg /tmp/ansible.cfg
export ANSIBLE_CONFIG=/tmp/ansible.config  ---> setting environment variable(ANSIBLE_CONFIG)
if we run ansible --version command now, the configuration file will be loaded from /tmp/ansible.config

To Unset Environment variable(remove the env variable)
UNSET ANSIBLE_CONFIG

First preference will be given to ANSIBLE_CONFIG.
Second preference will be given to ansible.cfg(in the current directory)

since we have already unset the env variable, the next option ansible look for is ansible.cfg

mkdir test
cd test/
cp /tmp/ansible.cfg 
ansible --version
config file = /home/ec2-user/test/ansible.cfg
cd ( coming out of directory and going to home directory)

Third preference will be given to .ansible.cfg(hidden file), if there is no ANSIBLE_CONFIG,ansible.cfg
cp /tmp/ansible.cfg ~/.ansibel.cfg (~/ denotes home directory)
cd ~(going to home directory)
ansible --version
config file = /home/ec2-user/.ansible.cfg

If there is no config file in either of the above 3 locations, the configuration file will be loaded from /etc/ansible/ansible.cfg

Best recommendation is to place the file in current directory(expense-ansible-roles)

vim /etc/ansible/ansible.cfg
once the file is opened, we can see a github url , copy and paste in browser where an example is present from which we can set env variables etc

Instead of writing all variables in command line, we can set variables in ansible.cfg file
cd
ls -l

Clone the project in ansible server
cd expense-ansible-roles
ls -l
ansible --version
config file = /home/ec2-user/expense-ansible-roles/ansible.cfg
ansible-playbook db.yaml( not need to write username and password, we have already specified in config file)

Comments don't work in inventory.ini file

In templates folder, we need to create a file named backend.service.j2 and paste the backend service cofiguration to this file.
.j2 ---> jinja 2 template language
Ansibel templates can keep variables inside files and replace the values with variables.
excatly like copy command, we use ansible.builtin.template

connect to ansibel server
git pull
ansible-playbook backend.yaml

Ansible automatically loads main.yaml files

Ansibel handlers are Specialized tasks designed to respond to notifications
eg: when there is change in configuration only , we need to restart the server

handlers are notifiers in ansible, usually when ther is some change in configuration,  we can notify the paticular task to perform some action(Restart server)

In ansibel server
git pull
ansible-playbook frontend.yaml


## Deployment

1. Create a directory
2. Remove old code
3. Download new code


- delete existing directory
- create new directory
- download code
- extract code

We can create a role named "common" or any any name under roles and move the code which is common for all roles. To use the code, we need to import the role

Instead of running backend, frontend and db files separately, we can create one file(main.yaml) and send the value as an argument to component at runtime, based on the value passed, the corresponding file will run

- ansible-playbook main.yaml -e component=db


We need to write ansible playbooks in proper roles format only.
main.yaml files willl be automatically loaded corresponding to files we executed. if we give ddifferent names, then we need to specify explicitly.






