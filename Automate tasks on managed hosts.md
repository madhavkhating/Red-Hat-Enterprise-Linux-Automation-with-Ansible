# Automate tasks on managed hosts:  
In this playbook, we will automate package installation on our managed nodes.  
Managed Nodes:  
web: Host Name: web.example.com  IP address: 192.168.56.116/24  
db: Host Name: db.example.com    IP address: 192.168.56.117/24  

We will be installing Apache web server packages on Web host and MariaDB Database packages on Database host.  
first Verify your inventory and Ping hosts in your inventory to check if they are rechable.  

```
[root@master ansible]# ansible-inventory -i inventory.ini --list | grep -Po '(\d{1,3}\.){3}\d{1,3}'
192.168.56.116
192.168.56.117
[root@master ansible]#
```
```
[root@master ansible]# ansible all -m ping -i inventory.ini
192.168.56.117 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
192.168.56.116 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
[root@master ansible]#
[root@master ansible]#
```
```
**install-pkg.yaml**  
---
- name: Install and Configure Apache Web Server (httpd)
  hosts: webhosts
  become: true

  tasks:
  - name: Install Apache (httpd) package
    ansible.builtin.dnf:
      name: httpd
      state: present

  - name: Ensure Apache service is started and enabled
    ansible.builtin.service:
      name: httpd
      state: started
      enabled: true

- name: Install and Configure MariaDB Database Server
  hosts: dbhosts
  become: true

  tasks:
  - name: Install MariaDB Server package
    ansible.builtin.dnf:
      name:
        - mariadb-server
        - mariadb
      state: present

  - name: Ensure MariaDB service is started and enabled
    ansible.builtin.service:
      name: mariadb
      state: started
      enabled: true
```  
```  
**inventory.ini**  
[webhosts]
192.168.56.116

[dbhosts]
192.168.56.117
```

#### Run the Playbook:  
```
[root@master ansible]# ansible-playbook -i inventory.ini install-pkg.yaml --syntax-check

playbook: install-pkg.yaml
[root@master ansible]# ansible-playbook -i inventory.ini install-pkg.yaml

PLAY [Install and Configure Apache Web Server (httpd)] ********************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************
ok: [192.168.56.116]

TASK [Install Apache (httpd) package] *************************************************************************************************************************
changed: [192.168.56.116]

TASK [Ensure Apache service is started and enabled] ***********************************************************************************************************
changed: [192.168.56.116]

PLAY [Install and Configure MariaDB Database Server] **********************************************************************************************************

TASK [Gathering Facts] ****************************************************************************************************************************************
ok: [192.168.56.117]

TASK [Install MariaDB Server package] *************************************************************************************************************************
changed: [192.168.56.117]

TASK [Ensure MariaDB service is started and enabled] **********************************************************************************************************
changed: [192.168.56.117]

PLAY RECAP ****************************************************************************************************************************************************
192.168.56.116             : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
192.168.56.117             : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

[root@master ansible]# ssh root@192.168.56.116 "dnf list httpd"
Updating Subscription Management repositories.
Last metadata expiration check: 1:31:36 ago on Monday 29 June 2026 12:22:02 PM.
Installed Packages
httpd.x86_64        2.4.63-13.el10_2.1        @rhel-10-for-x86_64-appstream-rpms
[root@master ansible]# ssh root@192.168.56.117 "dnf list mariadb"
Updating Subscription Management repositories.
Last metadata expiration check: 1:09:44 ago on Monday 29 June 2026 12:45:15 PM.
Installed Packages
mariadb.x86_64      3:10.11.15-2.el10_1       @rhel-10-for-x86_64-appstream-rpms
[root@master ansible]#
```



