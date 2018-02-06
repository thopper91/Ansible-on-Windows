# Ansible-on-Windows
The repository will assist you in using Ansible for a windows host machine. The modules are different and the required installation on the Linux machine is also different

I have create a set of Wiki documents to potential help with the setup and Ansible structure: [Home of the Wikis](https://github.com/thopper91/Ansible-on-Windows/wiki)

## Pre-requisites
This can be found in the following documentation: [Required pre-requisites](https://github.com/thopper91/Ansible-on-Windows/wiki/Pre-requisites)

## Folder Structure
This can be found in the following documentation: [Ansible folder and file structure](https://github.com/thopper91/Ansible-on-Windows/wiki/Ansible-Structure---Folder-and-Files)

## Lessons Learned
This can be found in the following docummentation: [Lessons Learned](https://github.com/thopper91/Ansible-on-Windows/wiki/Lessons-Learned)

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
