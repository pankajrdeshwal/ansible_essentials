# 🧩 Lab 2: Exploring Ad-Hoc Commands  

## 🎯 Objective  
Learn to execute **Ansible ad-hoc commands** for performing quick administrative tasks on managed nodes — without writing a playbook.  

---

## 🧱 Prerequisites  
✅ Completed **Lab 1** (Ansible installation and configuration)  
✅ **Control node** and **managed nodes** are connected via SSH  

---

## ⚙️ Lab Steps  

### 🪶 Step 1: Configure Localhost in Inventory  

Open your inventory file:  
```bash
sudo vi /etc/ansible/hosts
```

Add this line (press **INSERT** to edit):  
```text
localhost ansible_connection=local
```
📝 *This tells Ansible to treat your local system as a managed node so you can test commands locally.*  

Save and exit using `ESCAPE + :wq!`  

---

### 🧩 Step 2: Common Ad-Hoc Command Examples  

#### 🧠 1. Check Memory Details of All Hosts  
```bash
ansible all -m command -a "free -h"
```
🔹 **`all`** → Targets all hosts from inventory  
🔹 **`-m command`** → Uses the command module  
🔹 **`-a "free -h"`** → Runs the Linux command `free -h` to show memory usage  

✅ **Alternative (simpler):**
```bash
ansible all -a "free -h"
```
If no module is provided, Ansible defaults to the `command` module.  

---

#### 👤 2. Create a User Named `ansible-new` on All Nodes  
```bash
ansible all -m user -a "name=ansible-new" --become
```
🔹 **`user` module** → Manages users  
🔹 **`name=ansible-new`** → Username to create  
🔹 **`--become`** → Runs as a privileged user (like `sudo`)  

---

#### 📜 3. List All Users on node1  
```bash
ansible node1 -a "cat /etc/passwd"
```
🔹 Displays system user information on `node1`.  

---

#### 📂 4. List Directories in `/home` on node2  
```bash
ansible node2 -a "ls /home"
```
🔹 Lists directories under `/home` on `node2`.  

---

#### 🔐 5. Change Permissions of `/home/ansible-new` on node1  
```bash
ansible node1 -m file -a "dest=/home/ansible-new mode=755" --become
```
🔹 **`file` module** → Manages file permissions and attributes  
🔹 **`mode=755`** → Full access for owner, read/execute for others  
🔹 **`--become`** → Needed for permission changes  

---

#### 📝 6. Create an Empty File in `/home/ansible-new` on node1  
```bash
ansible node1 -m file -a "dest=/home/ansible-new/demo.txt mode=600 state=touch" --become
```
🔹 **`state=touch`** → Creates the file if it doesn’t exist  
🔹 **`mode=600`** → Read/write for owner only  

---

#### ✏️ 7. Add a Line to the File  
```bash
ansible node1 -b -m lineinfile -a 'dest=/home/ansible-new/demo.txt line="This server is managed by Ansible"'
```
🔹 **`lineinfile` module** → Adds or updates a line in a file  
🔹 **`-b`** → Shortcut for `--become`  
🔹 Useful for adding configuration comments automatically  

---

### 📂 Step 3: File Operations  

#### 🪶 1. Create a Local Test File  
```bash
touch test.txt
```
Creates a file named `test.txt` on the control node.  

#### 🧾 2. Add Content to the File  
```bash
echo "This file will be copied to managed node using copy module" >> test.txt
```
Appends a line to the file (uses `>>` to avoid overwriting existing content).  

#### 📤 3. Copy the File to node1  
```bash
ansible node1 -m copy -a "src=test.txt dest=/home/ansible-new/test" -b
```
🔹 **`copy` module** → Transfers files from control node to managed node  
🔹 **`src`** → Local source path  
🔹 **`dest`** → Remote destination path  
🔹 **`-b`** → Runs with elevated privileges  

---

### 🧹 Step 4: Cleanup  

Remove localhost entry after testing:  
```bash
sudo vi /etc/ansible/hosts
```
Delete this line:  
```text
localhost ansible_connection=local
```
Save and exit using `:wq!`  

---

## 🧠 Key Concepts Learned  

✅ **Ad-hoc command syntax:**  
`ansible <pattern> -m <module> -a "<arguments>"`  

✅ **Common modules:**  
`command`, `user`, `file`, `lineinfile`, `copy`  

✅ **Privilege escalation:**  
Use `--become` or `-b` for root-level tasks  

✅ **Practical tasks covered:**  
Creating users, managing files, modifying permissions, copying files  

---

## 🛠️ Troubleshooting  

⚠️ **Permission errors?** → Add `--become` or check sudo access  
⚠️ **Host unreachable?** → Verify SSH connectivity  
⚠️ **Command skipped?** → Ensure correct inventory and host names  

---

✨ *You’ve now practiced using ad-hoc commands — the foundation for writing Ansible playbooks!*  
