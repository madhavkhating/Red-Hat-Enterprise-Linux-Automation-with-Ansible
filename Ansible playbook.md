# First Ansible Playbook:  
write a simple Ansible playbook, and run the playbook to automate tasks on managed nodes.  
Playbooks are automation blueprints, in YAML format, that Ansible uses to deploy and configure managed nodes.  

**Playbook**  
A list of plays that define the order in which Ansible performs operations, from top to bottom, to achieve an overall goal.  

**Play**  
An ordered list of tasks that maps to managed nodes in an inventory.  

**Task**  
A reference to a single module that defines the operations that Ansible performs.  

**Module**  
A unit of code or binary that Ansible runs on managed nodes.  

create and run a playbook that pings your hosts.  

```
[root@master ansible]# vim pinghosts.yaml
[root@master ansible]#
[root@master ansible]# cat pinghosts.yaml
- name: My first play
  hosts: myhosts
  tasks:
   - name: Ping my hosts
     ansible.builtin.ping:
[root@master ansible]#
[root@master ansible]# ansible-playbook -i inventory.ini pinghosts.yaml

PLAY [My first play] ******************************************************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************
[WARNING]: Host '192.168.56.116' is using the discovered Python interpreter at '/usr/bin/python3.12', but future installation of another Python interpreter could cause a different interpreter to be discovered. See https://docs.ansible.com/ansible-core/2.21/reference_appendices/interpreter_discovery.html for more information.
ok: [192.168.56.116]
[WARNING]: Host '192.168.56.117' is using the discovered Python interpreter at '/usr/bin/python3.12', but future installation of another Python interpreter could cause a different interpreter to be discovered. See https://docs.ansible.com/ansible-core/2.21/reference_appendices/interpreter_discovery.html for more information.
ok: [192.168.56.117]

TASK [Ping my hosts] ******************************************************************************************************************************************
ok: [192.168.56.116]
ok: [192.168.56.117]

PLAY RECAP ****************************************************************************************************************************************************
192.168.56.116             : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
192.168.56.117             : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

[root@master ansible]#
```

Congratulations, you have started using Ansible!  



