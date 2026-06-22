# Implementing an Ansible playbook:  
Create an inventory of managed hosts write a simple Ansible playbook, and run the playbook to automate tasks on those hosts.  
**I have setup my practice lab using Oracle VirtualBox Manager.**  
## My Lab Setup
**Ansible Control Node:**  
**Host Name:  master.example.com**  
**Operating System: Red Hat Enterprise Linux 10.0**  
**IP address:  192.168.56.20/24 Default GW: 192.168.56.1**  


**Managed Nodes:**  
**web:**  
**Host Name:  web.example.com**  
**Operating System: Red Hat Enterprise Linux 10.0**  
**IP address:  192.168.56.116/24 Default GW: 192.168.56.1**  

**db:**  
**Host Name:  db.example.com**  
**Operating System: Red Hat Enterprise Linux 10.0**  
**IP address:  192.168.56.117/24 Default GW: 192.168.56.1**  

### What is inventory:  
Inventories organize managed nodes in centralized files that provide Ansible with system information and network locations.  
Using an inventory file, Ansible can manage a large number of hosts with a single command.  
you will need the IP address or fully qualified domain name of your managed nodes.  

### How to setup SSH Key-Based Authentication:  
You must also ensure that your public SSH key is added to the authorized_keys file on each host.  
you have to have same user on managed noteds and python installed. I am using root user account here.  
**Generate the SSH Key Pair and Copy the Public Key to managed nodes:**  

```
[root@master ~]# ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/root/.ssh/id_ed25519):
/root/.ssh/id_ed25519 already exists.
Overwrite (y/n)? y
Enter passphrase for "/root/.ssh/id_ed25519" (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_ed25519
Your public key has been saved in /root/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:YZUp6gXXMWEjHGT4SFh7Uiced5duMzDhOOAQYmRv/mg root@master.example.com
The key's randomart image is:
+--[ED25519 256]--+
|   .=o+=XoX=o... |
|   o.o+B+X+*o..  |
|     .=**.o .+   |
|     oo+o. .  =  |
|     ...S    . o |
|      .o         |
|      E .        |
|     .           |
|                 |
+----[SHA256]-----+
[root@master ~]#
[root@master ~]# ssh-copy-id root@192.168.56.116
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_ed25519.pub"
The authenticity of host '192.168.56.116 (192.168.56.116)' can't be established.
ED25519 key fingerprint is SHA256:AXjVOyRCJEaOpHKzuIYFHPROmf3b1PhrcqgwqWze0xQ.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@192.168.56.116's password:

Number of key(s) added: 1

Now try logging into the machine, with: "ssh 'root@192.168.56.116'"
and check to make sure that only the key(s) you wanted were added.

[root@master ~]# ssh-copy-id root@192.168.56.117
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_ed25519.pub"
The authenticity of host '192.168.56.117 (192.168.56.117)' can't be established.
ED25519 key fingerprint is SHA256:AXjVOyRCJEaOpHKzuIYFHPROmf3b1PhrcqgwqWze0xQ.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:10: 192.168.56.116
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@192.168.56.117's password:

Number of key(s) added: 1

Now try logging into the machine, with: "ssh 'root@192.168.56.117'"
and check to make sure that only the key(s) you wanted were added.

[root@master ~]#
```

**Test the Connection:**  

```
[root@master ~]#
[root@master ~]# ssh root@192.168.56.116 hostname
web.example.com
[root@master ~]#
[root@master ~]# ssh root@192.168.56.117 hostname
db.example.com
[root@master ~]#
[root@master ~]#

```
**Confirm Python is installed:**  
```
[root@master ~]#
[root@master ~]# ssh root@192.168.56.116 python --version
Python 3.12.9
[root@master ~]#
[root@master ~]# ssh root@192.168.56.117 python --version
Python 3.12.9
[root@master ~]#
[root@master ~]#
```

### Continue building an inventory:  

Create a file named inventory.ini in the ansible directory, Add a new [myhosts] group to the inventory.ini file and specify the IP address.  
Verify your inventory.   
Ping the myhosts group in your inventory.  
```
[root@master ~]#
[root@master ~]# cd ansible/
[root@master ansible]# pwd
/root/ansible
[root@master ansible]# vim inventory.ini
[root@master ansible]#
[root@master ansible]# ansible-inventory -i inventory.ini --list
{
    "_meta": {
        "hostvars": {},
        "profile": "inventory_legacy"
    },
    "all": {
        "children": [
            "ungrouped",
            "myhosts"
        ]
    },
    "myhosts": {
        "hosts": [
            "192.168.56.116",
            "192.168.56.117"
        ]
    }
}
[root@master ansible]#
[root@master ansible]# ansible myhosts -m ping -i inventory.ini
[WARNING]: Host '192.168.56.116' is using the discovered Python interpreter at '/usr/bin/python3.12', but future installation of another Python interpreter could cause a different interpreter to be discovered. See https://docs.ansible.com/ansible-core/2.21/reference_appendices/interpreter_discovery.html for more information.
192.168.56.116 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
}
[WARNING]: Host '192.168.56.117' is using the discovered Python interpreter at '/usr/bin/python3.12', but future installation of another Python interpreter could cause a different interpreter to be discovered. See https://docs.ansible.com/ansible-core/2.21/reference_appendices/interpreter_discovery.html for more information.
192.168.56.117 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
}
[root@master ansible]#
```
Congratulations, you have successfully built an inventory.  

 



