# Module 2: Ad-Hoc Commands

**Objective:** To move beyond simple connectivity tests and use Ansible to perform useful, real-world tasks on your managed nodes using simple one-liner commands.

---

### 2.1 What are Ad-Hoc Commands?

Think of Ad-Hoc commands as "quick and dirty" Ansible. They are simple, one-liner commands you run directly from your terminal to perform a quick task without needing to write a full playbook file.

They are perfect for situations like:
*   "I need to quickly reboot all web servers."
*   "What's the disk space on the database servers?"
*   "Is the `nginx` package installed on node1?"
*   "I need to copy a small file to all my servers right now."

While playbooks are for complex, repeatable, version-controlled orchestration (the "recipe card"), ad-hoc commands are for immediate, simple actions (a quick verbal instruction).

### 2.2 The Ad-Hoc Command Structure

All ad-hoc commands follow a simple and consistent structure:

`ansible <host-pattern> [options]`

The most common options you will use are:
*   `-i <inventory_file>`: Specifies the inventory file to use.
*   `-m <module_name>`: The **m**odule you want to use.
*   `-a "<module_arguments>"`: The **a**rguments or options for that module.
*   `-b` or `--become`: Tells Ansible to use privilege escalation (e.g., `sudo`).

**Example:**
`ansible servers -i inventory -m ansible.builtin.user -a "name=jane state=present" -b`

### 2.3 Essential Modules for Ad-Hoc Tasks

Modules are the heart of Ansible; they are the "tools" that do the actual work. For ad-hoc commands, a few modules cover 90% of use cases.

*   **`ansible.builtin.command`**: Executes a simple command on the remote node. **Important:** It does *not* go through a shell, so shell features like environment variables (`$HOME`), pipes (`|`), and redirects (`>`) will not work. It's safer and more predictable for simple commands.

*   **`ansible.builtin.shell`**: Also executes a command, but it *does* run it through the node's default shell (e.g., `/bin/sh` or `/bin/bash`). This allows you to use pipes, redirects, and other shell features. **Use this when the `command` module isn't enough.**

*   **`ansible.builtin.apt`** (for Debian/Ubuntu) or **`ansible.builtin.yum`** (for Red Hat/CentOS): The correct way to manage system software packages. These modules are idempotent.

*   **`ansible.builtin.service`**: The correct way to control system services (starting, stopping, restarting, enabling). This module is also idempotent.

*   **`ansible.builtin.copy`**: Used to copy files from your control node to the managed nodes.

*   **`ansible.builtin.file`**: Used to manage files and directories on the managed nodes (e.g., create a directory, set permissions, delete a file).

### 2.4 Privilege Escalation: Becoming `root` with `become`

Many tasks, like installing software or restarting system services, require administrator (or `root`) privileges. You can't do this as a regular, unprivileged user.

Ansible handles this with a concept called **`become`**. It's the equivalent of prefixing a command with `sudo`.

When you add the `--become` (or its shorter alias `-b`) flag to your Ansible command, you are telling Ansible: "For this specific task, please use the configured `become_method` (like `sudo`) to gain root privileges before you run it."

This works in our lab because we configured the `ansible` user with passwordless `sudo` rights during the bootstrap process.