# Lab 4: Working with Variables in Playbooks

## Objective
Learn to define and use variables in Ansible playbooks for dynamic configuration.

## Prerequisites
- Completed Labs 1-3
- Basic understanding of playbook structure

## Lab Steps

### Step 1: Create Playbook with Variables

Create `variables-demo.yml`:
```yaml
---
- name: Variables demo playbook
  hosts: all
  become: yes
  vars:
    package_name: httpd
    service_name: httpd
    web_root: /var/www/html
    custom_message: "Hello from Ansible Variables Lab"
  tasks:
    - name: Install package using variable
      yum:
        name: "{{ package_name }}"
        state: present
    
    - name: Create custom index file with variable content
      copy:
        content: |
          <html>
          <body>
          <h1>{{ custom_message }}</h1>
          <p>Package installed: {{ package_name }}</p>
          <p>Service running: {{ service_name }}</p>
          </body>
          </html>
        dest: "{{ web_root }}/index.html"
    
    - name: Start and enable service using variable
      service:
        name: "{{ service_name }}"
        state: started
        enabled: yes
```

### Step 2: Execute Variables Playbook

Run the playbook:
```bash
ansible-playbook variables-demo.yml
```

Check the generated web page:
```bash
ansible all -a "cat /var/www/html/index.html"
```

### Step 3: Using External Variable Files

Create `vars.yml` with variables:
```yaml
---
apache_package: httpd
apache_service: httpd
document_root: /var/www/html
welcome_message: "Welcome to External Variables Demo"
server_admin: admin@example.com
```

Create `external-vars-playbook.yml`:
```yaml
---
- name: External variables demo
  hosts: all
  become: yes
  vars_files:
    - vars.yml
  tasks:
    - name: Install Apache using external variable
      yum:
        name: "{{ apache_package }}"
        state: present
    
    - name: Configure Apache with external variables
      template:
        src: httpd.conf.j2
        dest: /etc/httpd/conf/httpd.conf
      notify: restart apache
    
    - name: Start Apache service
      service:
        name: "{{ apache_service }}"
        state: started
        enabled: yes
  
  handlers:
    - name: restart apache
      service:
        name: "{{ apache_service }}"
        state: restarted
```

### Step 4: Command Line Variables

Run playbook with command line variables:
```bash
ansible-playbook variables-demo.yml -e "package_name=nginx service_name=nginx"
```

Run with extra variables from file:
```bash
ansible-playbook variables-demo.yml -e "@extra_vars.yml"
```

### Step 5: Host and Group Variables

Create host-specific variables in inventory:
```bash
sudo vi /etc/ansible/hosts
```

Add host variables:
```text
[webservers]
node1 apache_port=8080 server_name=web1.example.com
node2 apache_port=8081 server_name=web2.example.com

[webservers:vars]
apache_package=httpd
apache_service=httpd
```

## Key Concepts Learned
- Variable definition in playbooks using `vars:`
- External variable files with `vars_files:`
- Command line variables with `-e` option
- Host and group variables in inventory
- Variable precedence and override behavior
- Jinja2 templating syntax `{{ variable_name }}`

## Troubleshooting
- Ensure proper YAML syntax for variable definitions
- Check variable names for typos and case sensitivity
- Verify file paths for external variable files
- Use `ansible-playbook --list-hosts` to verify inventory parsing