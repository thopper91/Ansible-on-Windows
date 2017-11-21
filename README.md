# Ansible-on-Windows
The repository will assist you in using Ansible for a windows host machine. The modules are different and the required installation on the Linux machine is also different

I have create a set of Wiki documents to potential help with the setup and Ansible structure:

[Home of the Wikis](../wiki)

## Pre-requisites
This can be found in the following documents:



## Folder Structure
The folder structure in this project is exactly how it should be. If preferred to have ALL the ansible code in 1 playbook, you require to have those playbooks at this level and for the playbook to look something similar to the below. It is worth noting, splitting them as I have is seen as best practice:
```yaml
---
- hosts: windows #The hosts file in inventory
  vars:
    Tool: 'test.msi'
    user: 'hoppert'
  tasks:
    - name: Install "{{ Tool }}" and wait for it to complete
      win_msi:
        path: C:\Users\{{ user }}\Documents\{{ Tool }}
        wait: yes
```
## Playbooks
All playbooks are stored in the roles folder, the playbooks in the master folder are the ones to be executed. If any variables have been used, these can be found in the roles section, find the playbook desired and if it has a 'tasks' and 'vars' folder, then variables was used.

Playbook | Windows Ansible Module used | Variables?
:---: | :---: | :---:
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
