
---

# 🧪 Lab 9: Ansible Lab - Jinja2 Templates

**Objective:** Learn how to use Jinja2 templates in Ansible to generate dynamic configuration files.

---

## 🧩 1. **What are Jinja2 Templates?**

* Jinja2 is a **templating language** used by Ansible to dynamically generate text files.
* Templates allow you to insert variables, run loops, and apply conditions.
* File extension: `.j2`

**Example:**
`Hello {{ name }}` → becomes → `Hello Azhar` when rendered.

---

## 🧰 2. **Lab Setup**

Create a new working directory:

```bash
mkdir ansible-jinja2-lab
```

```bash
cd ansible-jinja2-lab
```

---

## 🧾 3. **Create the Jinja2 Template**

Create the template directory first:

```bash
mkdir templates
```

**File:** `templates/welcome.j2`

```bash
cat > templates/welcome.j2 << 'EOF'
Hello {{ user_name }} 👋

Welcome to the {{ env_type }} environment.
This message was generated on {{ ansible_date_time.date }}.

Server Details:
- Hostname: {{ ansible_hostname }}
- OS: {{ ansible_facts['os_family'] }}
- IP Address: {{ ansible_default_ipv4.address }}

{% if env_type == "Production" %}
⚠️  Be careful! This is a production server.
{% else %}
🧪  Safe to test and experiment here.
{% endif %}
EOF
```

---

## 🧾 4. **Create the Playbook**

**File:** `playbook.yml`

```bash
cat > playbook.yml << 'EOF'
---
- name: Jinja2 Template Demonstration
  hosts: localhost
  connection: local
  vars:
    user_name: Azhar
    env_type: Development

  tasks:
    - name: Generate welcome file using Jinja2 template
      template:
        src: templates/welcome.j2
        dest: /tmp/welcome_message.txt

    - name: Display rendered file content
      debug:
        msg: "{{ lookup('file', '/tmp/welcome_message.txt') }}"
EOF
```

---

## ▶️ 5. **Run the Playbook**

```bash
ansible-playbook playbook.yml
```

---

## 🧩 6. **Expected Output**

**Console Output:**

*Expected output:*
```
TASK [Display rendered file content] ******************************************
ok: [localhost] => {
    "msg": "Hello Azhar 👋\n\nWelcome to the Development environment.\nThis message was generated on 2025-11-11.\n\nServer Details:\n- Hostname: Control-Node\n- OS: RedHat\n- IP Address: 172.31.25.104\n\n🧪  Safe to test and experiment here."
}
```

**Rendered File:**

```bash
cat /tmp/welcome_message.txt
```

*Expected rendered content:*

*Hello Azhar 👋*

*Welcome to the Development environment.*  
*This message was generated on 2025-11-11.*

*Server Details:*  
*- Hostname: Control-Node*  
*- OS: RedHat*  
*- IP Address: 172.31.25.104*

*🧪 Safe to test and experiment here.*

---

## 🧠 7. **Key Takeaways**

| Feature         | Example                   | Description                         |
| --------------- | ------------------------- | ----------------------------------- |
| Variable        | `{{ variable }}`          | Inserts values dynamically          |
| Condition       | `{% if %}` / `{% else %}` | Adds logic to templates             |
| Loop            | `{% for x in list %}`     | Repeats content                     |
| Facts           | `{{ ansible_hostname }}`  | Uses system data                    |
| Template Module | `template:`               | Renders `.j2` files to remote nodes |

---

## ✅ **Lab Summary**

You learned how to:

1. Create and use Jinja2 templates in Ansible
2. Use variables, facts, and conditionals inside templates
3. Render templates dynamically and verify output
4. Build comprehensive configuration files using Jinja2 templates

---

## 🎉 **Congratulations!**

**🏆 Outstanding Achievement!** You've successfully used Jinja2 templates in Ansible!

**🎯 Skills Unlocked:**
- ✅ Dynamic file generation with Jinja2
- ✅ Variable substitution and fact integration  
- ✅ Conditional logic in templates
- ✅ Production-ready configuration management

**🚀 You're now ready to:**
- Build environment-specific deployments
- Automate complex configuration scenarios
- Apply template-driven infrastructure as code

**Keep up the excellent work! Your Ansible automation journey is accelerating!** 🌟

---

