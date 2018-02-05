# Ansible-on-Windows
The repository will assist you in using Ansible for a windows host machine. The modules are different and the required installation on the Linux machine is also different

I have create a set of Wiki documents to potential help with the setup and Ansible structure: [Home of the Wikis](https://github.com/thopper91/Ansible-on-Windows/wiki)

## Pre-requisites
This can be found in the following documentation: [Required pre-requisites](https://github.com/thopper91/Ansible-on-Windows/wiki/Pre-requisites)

## Folder Structure
This can be found in the following documentation: [Ansible folder and file structure](https://github.com/thopper91/Ansible-on-Windows/wiki/Ansible-Structure---Folder-and-Files)

### Host Groups

**NOTE:** When it comes to creating host groups, they HAVE to be named _'windows'_ otherwise Ansible will not read it. One way to get around this is having ALL IPs & Domain names in the same group (like below) or having multiple host files with a select few IP and domain names in them. The second option does mean you will have to ensure you choose the correct invesntory file

```
[windows]
0.0.0.1
0.0.0.2
```

## Notes
### Users
Also, in order for the below to run successfully you will need to ensure the user is part of the domain admin user group or has admin access otherwise Kerberos will flag as an error. What has been done (however) is a specific Ansible on windows user has been created for the work we want to complete

### Run playbook
In order to run the playbooks in this repository, it has been found to be consistently effective to get Ansible to prompt the user for a password. If this password is added to _group_vars_ then it is known to provide the user with an error. So it is best to run the following:
```yml
$ ansible-playbook -i ./inventory/HOSTNAME FILENAME.yml --ask-pass
```
_HOSTNAME:_ The host file wanted to be executed

_FILENAME:_ The Ansible Playbook to be executed

## Playbooks
All playbooks are stored in the roles folder, the playbooks in the master folder are the ones to be executed. If any variables have been used, these can be found in the roles section, find the playbook desired and if it has a 'tasks' and 'vars' folder, then variables was used.

Playbook | Windows Ansible Module used | Variables?
:---: | :---: | :---:
Add user to all ilos | win_copy, win_psmodule, win_shell | *Yes*
Disable Windows Features | win_feature | *No*
Enable Windows Features | win_feature | *No*
Execute Powershell scripts | win_shell | *Yes*
Gather Facts on Windows Hosts | win_command | *No*
Gather IP details | win_command | *No*
Install MSI's | win_get_url, win_msi | *Yes*
Install Windows Updates (mass) | win_updates | *No*
Install Windows Updates (single) | win_updates | *No*
Log Available Windows Updates | win_updates | *Yes*
Pause | `NA` | *No*
Restart Windows Services | win_service | *Yes*
Start Windows Services | win_service | *Yes*
Stop Windows Services | win_service | *Yes*
Uninstall MSI's | win_msi | *Yes*
