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

------------



## **The `debug` Module in Ansible**

The **`debug`** module in Ansible is primarily used for **displaying information** during playbook execution. It's very helpful for **troubleshooting** and **logging values** of variables, output of commands, or specific states of your system.

### **Key Purpose:**

-   **Display output**: Print out the values of variables or results.
-   **Conditional debugging**: Print output only under certain conditions.
-   **Error handling**: To display error messages or intermediate results during playbook execution.

----------

### **Syntax of the `debug` Module**

```yaml
- name: Example task
  debug:
    msg: "Hello, world!"

```

----------

### **Key Parameters:**

1.  **`msg:`**
    
    -   This parameter is used to print a **message**.
    -   You can use this to display **static** or **dynamic messages** (like variables or facts).
    
    **Example:**
    
    ```yaml
    - name: Display a message
      debug:
        msg: "This is a custom message!"
    
    ```
    
2.  **`var:`**
    
    -   Prints the **value of a variable** (e.g., `var: variable_name`).
    -   This is useful for debugging the values of variables or facts you’re working with.
    
    **Example:**
    
    ```yaml
    - name: Show the value of a variable
      debug:
        var: my_var
    
    ```
    
    If `my_var` contains `hello`, the output will be:
    
    ```
    my_var: hello
    
    ```
    
3.  **`verbosity:`**
    
    -   Allows you to control the **level of verbosity** of the output.
    -   The value of `verbosity` can be set to `0`, `1`, `2`, or `3`, controlling when the debug message will be shown based on the verbosity level during playbook execution.
    
    **Example:**
    
    ```yaml
    - name: Show detailed information
      debug:
        msg: "This will show only on higher verbosity levels."
        verbosity: 2
    
    ```
    
4.  **`failed_when:`**
    
    -   This is useful to define a condition that will cause the task to fail, which is often used for debugging a particular condition or issue.
    
    **Example:**
    
    ```yaml
    - name: Show a message when a condition is met
      debug:
        msg: "This condition failed!"
      failed_when: my_var != "expected_value"
    
    ```
    

----------

### **Common Use Cases for the `debug` Module**

1.  **Displaying Variables During Execution:** It’s common to use `debug` to output the values of variables at various points in your playbook to ensure the correct values are being passed along.
    
    **Example:**
    
    ```yaml
    - name: Display a variable for debugging
      debug:
        var: ansible_facts
    
    ```
    
2.  **Conditional Debugging:** Sometimes you might want to only display debug messages when certain conditions are met.
    
    **Example:**
    
    ```yaml
    - name: Debug only if a variable is set
      debug:
        msg: "The variable my_var is defined!"
      when: my_var is defined
    
    ```
    
3.  **Print the Result of a Task:** You can use `debug` to print the results of tasks, such as the output of a command or the result of a lookup.
    
    **Example:**
    
    ```yaml
    - name: Get disk usage
      command: df -h
      register: disk_usage
    
    - name: Debug disk usage output
      debug:
        var: disk_usage.stdout
    
    ```
    
4.  **Error Handling with `fail` and `debug`:** Use `debug` along with the `fail` module to output error messages before deliberately failing a task.
    
    **Example:**
    
    ```yaml
    - name: Check if a file exists
      stat:
        path: /tmp/myfile.txt
      register: file_stat
    
    - name: Display error if the file doesn't exist
      debug:
        msg: "The file does not exist!"
      when: not file_stat.stat.exists
    
    - name: Fail if the file doesn't exist
      fail:
        msg: "File not found!"
      when: not file_stat.stat.exists
    
    ```
    

----------

### **Example Playbook Using `debug`:**

```yaml
---
- name: Ansible Debug Example
  hosts: localhost
  vars:
    my_var: "Hello, Ansible!"
    my_list:
      - item1
      - item2
      - item3
  tasks:
    - name: Display a simple message
      debug:
        msg: "This is a custom debug message."

    - name: Display a variable using `var`
      debug:
        var: my_var

    - name: Display the list content
      debug:
        var: my_list

    - name: Conditional debugging
      debug:
        msg: "This will only show if the variable my_var is defined!"
      when: my_var is defined

```

----------

### **Sample Output:**

```yaml
TASK [Display a simple message] ***
ok: [localhost] => (item=None) =>
  msg: This is a custom debug message.

TASK [Display a variable using `var`] ***
ok: [localhost] => (item=None) =>
  my_var: Hello, Ansible!

TASK [Display the list content] ***
ok: [localhost] => (item=None) =>
  my_list:
  - item1
  - item2
  - item3

TASK [Conditional debugging] ***
ok: [localhost] => (item=None) =>
  msg: This will only show if the variable my_var is defined!

```

----------
