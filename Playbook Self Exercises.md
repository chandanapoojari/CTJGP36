
---

## **🔹 1️⃣ Gather System Facts**
📜 **Playbook: `gather_facts.yml`**  
**Purpose:** Collect system details such as OS version, hostname, CPU info, and memory.  
```yaml
- name: Gather System Facts
  hosts: all
  gather_facts: yes
  tasks:
    - name: Show system facts
      debug:
        msg:
          - "OS: {{ ansible_distribution }} {{ ansible_distribution_version }}"
          - "Hostname: {{ ansible_hostname }}"
          - "CPU Cores: {{ ansible_processor_cores }}"
          - "Total Memory: {{ ansible_memtotal_mb }} MB"
```
✅ **Self-check:**  
- Run `ansible-playbook gather_facts.yml` and verify the output.

---

## **🔹 2️⃣ Create a User and Add to a Group **
📜 **Playbook: `create_user.yml`**  
**Purpose:** Create a user **devops_user**, add to **sudoers**, and set a password.  
```yaml
- name: Create a new user and add to sudo
  hosts: all
  become: yes
  tasks:
    - name: Create a user
      user:
        name: devops_user
        shell: /bin/bash
        groups: wheel
        append: yes
        password: "{{ 'password' | password_hash('sha512') }}"

    - name: Ensure sudo access without a password
      lineinfile:
        path: /etc/sudoers.d/devops_user
        line: "devops_user ALL=(ALL) NOPASSWD: ALL"
        create: yes
        validate: 'visudo -cf %s'
```
✅ **Self-check:**  
- Run `ansible-playbook create_user.yml`  
- SSH into the system and switch to `devops_user`:  
  ```sh
  su - devops_user
  sudo whoami  # Should return 'root'
  ```

---

## **🔹 3️⃣ Create a Group and Assign Users**
📜 **Playbook: `create_group.yml`**  
**Purpose:** Create a group **devops** and add users **user1** and **user2**.  
```yaml
- name: Create a group and add users
  hosts: all
  become: yes
  tasks:
    - name: Create a group called devops
      group:
        name: devops
        state: present

    - name: Create users and add to devops group
      user:
        name: "{{ item }}"
        groups: devops
        append: yes
      loop:
        - user1
        - user2
```
✅ **Self-check:**  
- Run `ansible-playbook create_group.yml`  
- Check group members:  
  ```sh
  grep devops /etc/group
  ```

---

## **🔹 4️⃣ Create a Directory and Set Permissions**
📜 **Playbook: `create_directory.yml`**  
**Purpose:** Create `/opt/devops_data` and set ownership to **devops_user**.  
```yaml
- name: Create a directory and set permissions
  hosts: all
  become: yes
  tasks:
    - name: Create directory
      file:
        path: /opt/devops_data
        state: directory
        owner: devops_user
        group: devops
        mode: '0755'
```
✅ **Self-check:**  
- Run `ansible-playbook create_directory.yml`  
- Check ownership:  
  ```sh
  ls -ld /opt/devops_data
  ```

---

## **🔹 5️⃣ Create a File with Content**
📜 **Playbook: `create_file.yml`**  
**Purpose:** Create a file **/tmp/info.txt** with system info.  
```yaml
- name: Create a file with system details
  hosts: all
  become: yes
  tasks:
    - name: Generate system info file
      copy:
        content: |
          System Information:
          Hostname: {{ ansible_hostname }}
          OS: {{ ansible_distribution }} {{ ansible_distribution_version }}
        dest: /tmp/info.txt
        mode: '0644'
```
✅ **Self-check:**  
- Run `ansible-playbook create_file.yml`  
- View the file:  
  ```sh
  cat /tmp/info.txt
  ```

---

## **🔹 6️⃣ Start, Stop, and Enable a Service**
📜 **Playbook: `manage_services.yml`**  
**Purpose:** Install **Apache (httpd)**, ensure it is running, and enable it at boot.  
```yaml
- name: Install and Manage Apache Service
  hosts: all
  become: yes
  tasks:
    - name: Install Apache (httpd)
      yum:
        name: httpd
        state: present

    - name: Start and enable httpd
      service:
        name: httpd
        state: started
        enabled: yes
```
✅ **Self-check:**  
- Run `ansible-playbook manage_services.yml`  
- Check service status:  
  ```sh
  systemctl status httpd
  ```

---

## **🔹 7️⃣ Delete a User and Their Home Directory**
📜 **Playbook: `delete_user.yml`**  
**Purpose:** Remove **user1** and delete the home directory.  
```yaml
- name: Delete a user and remove home directory
  hosts: all
  become: yes
  tasks:
    - name: Delete user1
      user:
        name: user1
        state: absent
        remove: yes
```
✅ **Self-check:**  
- Run `ansible-playbook delete_user.yml`  
- Verify user deletion:  
  ```sh
  id user1  # Should return "no such user"
  ```

---
