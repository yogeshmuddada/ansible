## **🔹 Basic Syntax and Indentation in Ansible**

---

## **✅ Basic Syntax of Ansible Playbooks**

```yaml
- name: Playbook Name  # Defines the playbook
  hosts: all           # Specifies the target hosts
  become: yes          # Run tasks as root (sudo)
  tasks:               # Defines the tasks to execute
    - name: Install Nginx  # Task 1
      apt:               
        name: nginx
        state: present

    - name: Create a file  # Task 2
      file:               
        path: /home/user/sample.txt
        state: touch
```

### ** Breakdown of the Syntax**
| Component   | Description |
|-------------|------------|
| `- name:`  | Name of the playbook (Optional but recommended) |
| `hosts:`  | Specifies which machines to run the tasks on |
| `become:`  | Runs tasks with elevated privileges (like `sudo`) |
| `tasks:`  | A list of tasks to execute |
| `- name:`  | Describes the task (for logging purposes) |
| `apt:`  | Module used (in this case, to install packages) |
| `file:`  | Module to create/manage files |
| `state:`  | Defines the desired state (e.g., `present`, `absent`, `touch`) |

---

## YAML Indentation Rules
1. **Use spaces, not tabs.** (Recommended: **2 or 4 spaces** per level)
2. **Each key-value pair must be properly aligned.**
3. **List items must start with `-`** (e.g., `- name:`).
4. **Nested structures require increased indentation.**

**Correct Indentation:**
```yaml
tasks:
  - name: Install a package
    apt:
      name: nginx
      state: present
```
