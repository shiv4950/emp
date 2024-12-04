




 1. Inventory File (inventory.ini)
[production]
managed_node1 ansible_host=<managed_node1_IP>
managed_node2 ansible_host=<managed_node2_IP>

[production:vars]
ansible_user=ubuntu
ansible_python_interpreter=/usr/bin/python3
ansible_ssh_private_key_file=/path/to/your-key.pem

# a) Playbook to Retrieve and Display Current Date and Logged-In User
---
- name: Display date and logged-in user on all managed nodes
  hosts: production
  tasks:
    - name: Display current date
      command: date
      register: current_date

    - name: Show current date
      debug:
        msg: "Current date: {{ current_date.stdout }}"

    - name: Display logged-in user
      command: whoami
      register: logged_user

    - name: Show logged-in user
      debug:
        msg: "Logged-in user: {{ logged_user.stdout }}"

# b) Playbook to Update apt, Install Java, and Display Java Version
---
- name: Update apt, install Java, and display Java version
  hosts: production
  tasks:
    - name: Update apt package manager
      apt:
        update_cache: yes
        force_apt_get: yes

    - name: Install Java
      apt:
        name: openjdk-17-jdk
        state: present

    - name: Display installed Java version
      command: java -version
      register: java_version

    - name: Show Java version
      debug:
        msg: "Java version: {{ java_version.stderr_lines }}"

# c) Playbook to Install and Start Nginx
---
- name: Install and start Nginx web server
  hosts: production
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start Nginx service
      service:
        name: nginx
        state: started
        enabled: yes

# d) Playbook to Copy and Execute Shell Script
---
- name: Copy and execute shell script on all managed nodes
  hosts: production
  tasks:
    - name: Copy shell script to managed nodes
      copy:
        src: /path/to/local/script.sh
        dest: /tmp/script.sh
        mode: '0755'

    - name: Execute the script on managed nodes
      command: /tmp/script.sh

# e) Playbook to Add Multiple Users with Home Directories
---
- name: Add multiple users on all managed nodes
  hosts: production
  tasks:
    - name: Add user1
      user:
        name: user1
        state: present
        create_home: yes

    - name: Add user2
      user:
        name: user2
        state: present
        create_home: yes

    - name: Add user3
      user:
        name: user3
        state: present
        create_home: yes



 # ansible
 a) Launch Four EC2 Instances in AWS
Control Node:

Instance type: t2.micro
Storage: 15 GiB
Managed Nodes (server1, server2, dev1):

Instance type: t2.micro
Storage: 8 GiB each
Note: Ensure all instances use the same .pem file for SSH access.

b) Install Ansible on the Control Node
SSH into your control node:
bash
Copy code
ssh -i your-key.pem ubuntu@<control-node-IP>
Update packages and install Ansible:
bash
Copy code
sudo apt update
sudo apt install -y ansible
c) Transfer the .pem File to the Control Node
Transfer the .pem file from your local machine:
bash
Copy code
scp -i your-key.pem your-key.pem ubuntu@<control-node-IP>:/home/ubuntu
Set proper permissions for the key file:
bash
Copy code
chmod 400 /home/ubuntu/your-key.pem
d) Create an Ansible Inventory File
Create an inventory file (inventory.ini) on the control node:
bash
Copy code
nano inventory.ini
Add the following content to the file:
ini
Copy code
[production]
server1 ansible_host=<server1-IP>
server2 ansible_host=<server2-IP>

[production:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
ansible_ssh_private_key_file=/home/ubuntu/your-key.pem

[development]
dev1 ansible_host=<dev1-IP>

[development:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
ansible_ssh_private_key_file=/home/ubuntu/your-key.pem
Operations Using Ansible Ad-Hoc Commands

e) Verify Connections to Managed Nodes
Run the following to ping the groups:

bash
Copy code
ansible production -i inventory.ini -m ping
ansible development -i inventory.ini -m ping

f) Install Java 17 on Managed Nodes in the Production Group
bash
Copy code
ansible production -i inventory.ini -b -m apt -a "name=openjdk-17-jdk state=present update_cache=yes"

g) Gather System Information from Managed Nodes
Get the logged-in user:

bash
Copy code
ansible production -i inventory.ini -m command -a "whoami"
Get system uptime:

bash
Copy code
ansible production -i inventory.ini -m command -a "uptime"
Get CPU information:

bash
Copy code
ansible production -i inventory.ini -m command -a "lscpu"
Get available disk space:

bash
Copy code
ansible production -i inventory.ini -m command -a "df -h"
Get system memory usage:

bash
Copy code
ansible production -i inventory.ini -m command -a "free -h"

h) Display Inventory Information
bash
Copy code
ansible-inventory -i inventory.ini --list
i) Display Git Executable Path
On all servers in both the production and development groups:

bash
Copy code
ansible production:development -i inventory.ini -m command -a "which git"
Only on servers that are in both groups:

bash
Copy code
ansible production:&development -i inventory.ini -m command -a "which git"
Only on servers that are in the production group but not in the development group:

bash
Copy code
ansible production:!development -i inventory.ini -m command -a "which git"
This guide provides a comprehensive step-by-step approach to setting up and managing nodes using Ansible on Ubuntu. Let me know if you need any more details!

