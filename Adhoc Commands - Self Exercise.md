These commands are run from the Ansible **control node** and execute tasks on managed nodes without needing a playbook.

---

## 🔹 **Basic Connection & Ping**
Check connectivity to all hosts in the inventory:  
```bash
ansible all -m ping
```
Check connectivity to a specific host/group:  
```bash
ansible webservers -m ping
```

---

## 🔹 **Gather System Information**
Get system uptime:  
```bash
ansible all -m command -a "uptime"
```
Check memory usage:  
```bash
ansible all -m shell -a "free -m"
```
Check disk usage:  
```bash
ansible all -m shell -a "df -h"
```

---

## 🔹 **User & Group Management**
Create a new user:  
```bash
ansible all -m user -a "name=devops state=present"
```
Delete a user:  
```bash
ansible all -m user -a "name=devops state=absent"
```
Add a user to a group:  
```bash
ansible all -m user -a "name=devops groups=sudo append=yes"
```

---

## 🔹 **File & Directory Operations**
Create a directory:  
```bash
ansible all -m file -a "path=/opt/testdir state=directory mode=0755"
```
Create an empty file:  
```bash
ansible all -m file -a "path=/opt/testfile.txt state=touch"
```
Remove a file:  
```bash
ansible all -m file -a "path=/opt/testfile.txt state=absent"
```

---

## 🔹 **Package Management**
Install a package (e.g., `nginx`):  
```bash
ansible all -m yum -a "name=nginx state=present" -b    # For RHEL/CentOS
```
Remove a package:  
```bash
ansible all -m apt -a "name=nginx state=absent" -b
```

---

## 🔹 **Service Management**
Start a service:  
```bash
ansible all -m service -a "name=nginx state=started" -b
```
Stop a service:  
```bash
ansible all -m service -a "name=nginx state=stopped" -b
```
Restart a service:  
```bash
ansible all -m service -a "name=nginx state=restarted" -b
```
Check service status:  
```bash
ansible all -m systemd -a "name=nginx state=started" -b
```

---

## 🔹 **Managing Files & Permissions**

Change file permissions:  
```bash
ansible all -m file -a "path=/tmp/myfile.txt mode=0644 owner=root group=root"
```

---

## 🔹 **Execute Commands & Scripts**
Run a simple command:  
```bash
ansible all -m command -a "hostname"
```
Run a shell script on remote hosts:  
```bash
ansible all -m script -a "/home/user/myscript.sh"
```
---

## 🔹 **Reboot & Shutdown**
Reboot all servers:  
```bash
ansible all -m reboot -b
```
Shutdown a server:  
```bash
ansible all -m command -a "shutdown -h now" -b
```
---

