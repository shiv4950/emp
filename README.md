
# 1. Inventory File (inventory.ini)
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

