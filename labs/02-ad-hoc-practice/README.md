# Lab: Ad-Hoc Command Practice

**Objective:** Use ad-hoc commands to perform a series of real-world tasks on the lab environment.

### Prerequisites

*   A running lab environment, created by the playbook in `labs/01-bootstrap-environment`.
*   An `inventory` file should exist in the `01-bootstrap-environment` directory.

### Instructions

All commands should be run from the `labs/02-ad-hoc-practice` directory. We will reference the inventory file using a relative path (`../01-bootstrap-environment/inventory`).

---

#### **Task 1: Information Gathering (Safe Commands)**

Let's run some simple commands to get information from our nodes. These commands don't change anything.

1.  **Check connectivity to all nodes.**
    ```bash
    ansible all -i ../01-bootstrap-environment/inventory -m ping
    ```

2.  **Check the kernel version on `ansible-node1` only.**
    ```bash
    ansible ansible-node1 -i ../01-bootstrap-environment/inventory -m command -a "uname -r"
    ```

3.  **Check the free memory on all servers.**
    ```bash
    ansible all -i ../01-bootstrap-environment/inventory -m command -a "free -m"
    ```

4.  **Use the `shell` module to see disk usage for the root directory (`/`).**
    ```bash
    ansible all -i ../01-bootstrap-environment/inventory -m shell -a "df -h /"
    ```

---

#### **Task 2: Managing Files and Directories**

Let's create a directory and a file. These tasks require `sudo` rights, so we will use the `-b` flag.

1.  **Create a `/tmp/testdir` directory on all nodes.**
    ```bash
    ansible all -i ../01-bootstrap-environment/inventory -m file -a "path=/tmp/testdir state=directory mode=0755" -b
    ```
    *Run this command a second time. Notice how it reports "SUCCESS" (green) instead of "CHANGED" (yellow)? That's idempotency!*

2.  **Delete the directory we just created.**
    ```bash
    ansible all -i ../01-bootstrap-environment/inventory -m file -a "path=/tmp/testdir state=absent" -b
    ```

---

#### **Task 3: Managing Packages and Services**

Let's install a useful utility (`htop`) and manage a core service (`cron`).

1.  **Update the `apt` package cache on all nodes.** This is the equivalent of `sudo apt-get update`.
    ```bash
    ansible all -i ../01-bootstrap-environment/inventory -m apt -a "update_cache=yes" -b
    ```

2.  **Install the `htop` package on all nodes.** `htop` is an interactive process viewer.
    ```bash
    ansible all -i ../01-bootstrap-environment/inventory -m apt -a "name=htop state=present" -b
    ```

3.  **Install the `cron` package on all nodes.** `cron` is a task scheduler.
    ```bash
    ansible all -i ../01-bootstrap-environment/inventory -m apt -a "name=cron state=present" -b
    ```

4.  **Check if the `cron` service is running.**
    ```bash
    ansible all -i ../01-bootstrap-environment/inventory -m service -a "name=cron state=started"
    ```

5.  **Restart the `cron` service on all nodes.**
    ```bash
    ansible all -i ../01-bootstrap-environment/inventory -m service -a "name=cron state=restarted" -b
    ```