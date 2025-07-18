# Module 1: Lab Setup

This lab contains a single playbook, `bootstrap-lab-environment.yml`, designed to create a complete, multi-node Ansible lab environment on your local machine using Podman.

### Prerequisites

*   macOS with Homebrew
*   Podman: `brew install podman`
*   sshpass: `brew install sshpass`
*   Ansible: `brew install ansible`
*   Ansible Podman Collection: `ansible-galaxy collection install containers.podman`

### How to Run

1.  Ensure your Podman machine is running: `podman machine start`
2.  From this directory (`labs/module-01-lab-setup`), run the bootstrap playbook:
    ```bash
    ansible-playbook bootstrap-lab-environment.yml
    ```
This will create three containers (`ansible-node1`, `ansible-node2`, `ansible-node3`) and a local `inventory` file in this directory, ready for use in subsequent modules.