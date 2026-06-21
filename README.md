# Red-Hat-Enterprise-Linux-Automation-with-Ansible
# Introduction to Ansible
Ansible environments have three main components:
Control node
A system on which Ansible is installed. You run Ansible commands such as ansible or ansible-inventory on a control node.
Inventory
A list of managed nodes that are logically organized. You create an inventory on the control node to describe host deployments to Ansible.
Managed node
A remote system, or host, that Ansible controls.

Ansible uses simple, human-readable scripts called playbooks to automate your tasks. You declare the desired state of a local or remote system in your playbook. Ansible ensures that the system remains in that state.

As automation technology, Ansible is designed around the following principles:

Agent-less architecture
Low maintenance overhead by avoiding the installation of additional software across IT infrastructure.

Simplicity
Automation playbooks use straightforward YAML syntax for code that reads like documentation. Ansible is also decentralized, using SSH with existing OS credentials to access remote machines.

Scalability and flexibility
Easily and quickly scale the systems you automate through a modular design that supports a large range of operating systems, cloud platforms, and network devices.

Idempotence and predictability
When the system is in the state your playbook describes, Ansible does not change anything, even if the playbook runs multiple times.

# Install Red Hat Ansible Automation Platform on control node
Ansible is an agentless automation tool that you install on a single host (referred to as the control node).
From the control node, Ansible can manage an entire fleet of machines and other devices (referred to as managed nodes) remotely with SSH, Powershell remoting, and numerous other transports, all from a simple command-line interface with no databases or daemons required.
For your control node (the machine that runs Ansible), you can use nearly any UNIX-like machine with Python installed.
I am using Red Hat Enterprise Linux 10.0 installed on Oracle VirtualBox Manager. on all RHEL python is pre-installed.
The managed node (the machine that Ansible is managing) does not require Ansible to be installed, but requires Python to run Ansible-generated Python code. The managed node also needs a user account that can connect through SSH to the node with an interactive POSIX shell.

<img width="1216" height="620" alt="au-1" src="https://github.com/user-attachments/assets/65e0743d-ebf4-4521-b6a7-0c63d9d9e4b7" />

# Installing Ansible with pip
 Ensuring pip is available:
verify whether pip is already installed for your preferred Python:
python3 -m pip -V

<img width="1220" height="625" alt="au-2" src="https://github.com/user-attachments/assets/26cea6d3-8a29-48da-829d-6ac8471823a5" />

Installing Ansible:
Use pip in your selected Python environment to install the full Ansible package for the current user:
python3 -m pip install --user ansible

Confirming your installation:
You can test that Ansible is installed correctly by checking the version:
ansible --version

<img width="1218" height="625" alt="au-3" src="https://github.com/user-attachments/assets/df73ba57-08df-478a-895a-ac9313644b22" />


 

