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

### Breakdown of the Syntax
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

## **🔹 Understanding `state:` in Ansible**
In Ansible, the `state:` parameter defines the **desired condition** of a resource. It tells Ansible **what should happen** to a package, file, service, or other system resource.

---

## **✅ Common `state:` Values by Module**
Different Ansible **modules** use different `state:` options. Below are some commonly used ones:

### **1️⃣ `state:` in Package Management (`apt`, `yum`, `dnf`, `package`)**
Used to install or remove packages.

```yaml
- name: Install a package (nginx)
  apt:
    name: nginx
    state: present  # Ensures nginx is installed
```
| **State**  | **Meaning** |
|------------|------------|
| `present`  | Ensures the package is installed. |
| `latest`   | Installs the latest version of the package. |
| `absent`   | Removes the package if installed. |

---

### **2️⃣ `state:` in File Management (`file` module)**
Controls files, directories, and symbolic links.

```yaml
- name: Create a directory
  file:
    path: /home/user/mydir
    state: directory  # Ensures this is a directory
    mode: '0755'
```

| **State**      | **Meaning** |
|---------------|------------|
| `file`        | Ensures the path is a file. |
| `directory`   | Ensures the path is a directory. |
| `touch`       | Creates an empty file if it doesn’t exist. |
| `absent`      | Deletes the file or directory if it exists. |
| `link`        | Ensures the path is a symbolic link. |

---

### **3️⃣ `state:` in Service Management (`service` module)**
Used to manage services.

```yaml
- name: Ensure Nginx is running
  service:
    name: nginx
    state: started  # Starts the service if not running
```

| **State**     | **Meaning** |
|--------------|------------|
| `started`    | Ensures the service is running. |
| `stopped`    | Ensures the service is stopped. |
| `restarted`  | Forces the service to restart. |
| `reloaded`   | Reloads the service configuration. |

---

### **4️⃣ `state:` in Copying Files (`copy` module)**
Used to copy files from local to remote.

```yaml
- name: Copy a file
  copy:
    src: /local/path/file.txt
    dest: /remote/path/file.txt
    state: present
```

| **State**  | **Meaning** |
|------------|------------|
| `present`  | Ensures the file is copied to the destination. |
| `absent`   | Ensures the file is deleted if it exists. |

---

### **5️⃣ `state:` in Git Repository Management (`git` module)**
Controls Git repository states.

```yaml
- name: Clone a Git repository
  git:
    repo: "https://github.com/example/repo.git"
    dest: /home/user/myrepo
    version: main
    state: present  # Ensures the repo is cloned
```

| **State**  | **Meaning** |
|------------|------------|
| `present`  | Ensures the repository is cloned. |
| `absent`   | Deletes the repository if it exists. |
| `latest`   | Ensures the latest version is pulled. |

---

### **6️⃣ `state:` in User Management (`user` module)**
Used to manage users.

```yaml
- name: Ensure user "john" exists
  user:
    name: john
    state: present
```

| **State**  | **Meaning** |
|------------|------------|
| `present`  | Ensures the user exists. |
| `absent`   | Deletes the user. |

---

## **🔹 Key Takeaways**
✅ `state:` defines the **desired state** of a resource.  
✅ **Different modules** support different `state:` values.  
✅ **Common states:** `present`, `absent`, `latest`, `started`, `stopped`, etc.  
✅ **Idempotency:** Ansible will only make changes if the state **does not match** the current system state.  

Would you like a hands-on **practice task** using `state:`? 🚀
