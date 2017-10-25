# Ansible-on-Windows
The repository will assist you in using Ansible for a windows host machine. The modules are different and the required installation on the Linux machine is also different

## Pre-requisites
Before going any further, the following will need to be completed:
- Have a Linux Box (I am using Ubuntu)
- Have a Windows Control Machine (Using Server 2016)

Now for the steps to setup the environment

### On Linux Box
1) Install Ansible, Pip and Python on the machine
```json
### Ansible ###
$ sudo apt-get install libssl-dev
$ sudo pip install ansible

### Python & Pip ###
$ sudo apt-get install python python-pip

### Check installed correctly and version. Ansible = pip list ###
$ pip list
$ python --version
$ pip --version
```
2) Install winrm ```$ pip2 install "pywinrm>=0.2.2" ```
3) Install Kerberos and its dependencies. Kerberos is the preferred option compared to NTLM to use when using an Active Directory account but it requires a few extra steps to set up on the Ansible control host. There are more options to [install Kerberos](http://docs.ansible.com/ansible/latest/intro_windows.html#kerberos), however as I am using Ubuntu (dependencies first!):
 - Dependencies ``` sudo apt-get install python-dev libkrb5-dev krb5-user ```
 - Install Kerberos ``` $ sudo pip install pywinrm[kerberos] ```
4) Now to configure Kerberos. This part is a little tricky but bare with us! We need to edit the file in /etc/krb5.conf, first need to edit the section of the file which starts with [realms]. Add the full domain name (capitals) and the qualified names of the primary and secondary Active Directory domain controllers; should look like this:
```
[realms]

MY.DOMAIN.COM = {
  kdc = domain-controller01.my.domain.com
  kdc = domain-controller02.my.domain.com
}
```
Scroll down to the section that starts with [domain_realm]. Essentially its the .my.domain.com from the domain controllers and the domain name in capitals:
```
[domain_realm]
    .mydomain.com = MY.DOMAIN.COM
```
5) Test the Kerberos connection: ``` $ kinit username@MY.DOMAIN.COM ```
6) Install CredSSP. CredSSP authentication can be used to authenticate with both domain and local accounts. It allows credential delegation to do second hop authentication on a remote host by sending an encrypted form of the credentials to the remote host using the CredSSP protocol. ```$ sudo pip install pywinrm[credssp]```
7) Create Inventory hosts file and set it up with the .cfg file. The .cfg file is already setup in this repository, will just need to update the [Inventory hosts](../master/inventory/hosts) file stored in the Inventory folder. This is fulfilled in native Ansible
8) Next, create the group_vars folder along with a file within it. This file will hold all of the group variables that will be used when connecting to a Windows host. There is already a [folder](../master/group_vars) & [file](../master/group_vars/windows.yml) here, the only things ideally that need changing are the user and password variables.

### On Windows Control Machine
Now we need to pop over to the Windows control machine, and will need to download/create the [Powerscript file](../master/ConfigureRemotingforAnsible.ps1) which is in this GitHub. The following steps are to be inputted into the Command Prompt (cmd):

9) ``` winrm set winrm/config/service @{AllowUnencrypted="false"} ``` //True if using HTTP 5985, False if using HTTPS 5986
10) Execute the powershell "ConfigureRemotingforAnsible.ps1"

Admins may want to choose to modify default settings, the following can be done (NOTE: I ran all of the below):
- Customize the expiration date of generated certificate ``` powershell.exe -File ConfigureRemotingforAnsible.ps1 -CertValidityDays 100  ```
- Enable CredSSP as an authentication option ``` powershell.exe -File ConfigureRemotingforAnsible.ps1 -EnableCredSSP  ```
- Force a new SSL certificate to be attached to an already existing winrm listener ``` powershell.exe -File ConfigureRemotingforAnsible.ps1 -ForceNewSSLCert  ```
- Switch to configure winrm to listen on public zoon interfaces ``` powershell.exe -File ConfigureRemotingforAnsible.ps1 -SkipNetworkProfileCheck  ```

### On Linux Box
11) Run the ping command to ensure the Linux box and Windows Control Machine are connected and talking together, the windows in command is the group_vars file:  ``` $ ansible windows -m win_ping ```

Now the environment should be complete and setup successfully, any issues then please refer to the Ansible Document: https://docs.ansible.com/ansible/latest/intro_windows.html#id5

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
