# ansible-elixir

Ansible role to manage elixir nodes

## Table of Contents

- [Introduction to Ansible](#introduction-to-ansible)
- [Why Use Ansible?](#why-use-ansible)
- [Prerequisites](#prerequisites)
- [Installing Ansible](#installing-ansible)
- [Setting Up Your Environment](#setting-up-your-environment)
- [Secret Management](#secret-management)
- [Running Ansible Inventory](#running-ansible-inventory)
- [Health Check examples](#health-check-examples)
- [Additional Resources](#additional-resources)
- [Contribution Guidelines](#contribution-guidelines)
- [License](#license)
- [Changelog](#changelog)

## Introduction to Ansible

Ansible is an open-source automation tool that simplifies the process of configuration management, application deployment, and task automation. It uses a simple, human-readable language (YAML) to describe automation jobs, which allows you to manage complex deployments easily.

### Why Use Ansible?

- **Agentless**: No need to install any software on the nodes you manage.
- **Simple**: Uses YAML for configuration, which is easy to read and write.
- **Powerful**: Can manage complex deployments and configurations.

## Prerequisites

Before you start, ensure you have the following:

- A machine with Ansible installed (control node).
- SSH access to the target machines (managed nodes).

## Installing Ansible

To install Ansible on your control node, follow these steps:

### Ubuntu

```sh
sudo apt update
sudo apt install ansible
```

### macOS

```sh
brew install ansible
```

For other operating systems, refer to the [Ansible installation guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

## Setting Up Your Environment

1. **Clone the Repository**:

    ```sh
    git clone https://github.com/your-repo/ansible-elixir.git
    cd ansible-elixir
    ```

2. **Create a Virtual Environment** (optional but recommended):

    ```sh
    python3 -m venv venv
    source venv/bin/activate
    ```

3. **Install Required Ansible Collections**:

    ```sh
    ansible-galaxy install -r requirements.yml
    ```

4. **Modify the Ansible Configuration File**:

    Edit the `ansible.cfg` file to set the inventory path and any other necessary configurations. Here is an example of what you might need to add or modify:

    ```ini
    [defaults]
    inventory = ./inventory
    remote_user = root
    private_key_file = ~/.ssh/id_ed25519
    host_key_checking = False
    ```

    This configuration sets the inventory file path, remote user, private key file, and disables host key checking.

## Secret Management

To manage your secrets, create a `secrets.yml` file and populate it with the necessary credentials. **This file should be kept secure and not committed to version control (i.e. add to your .gitignore).**

We recommend backing up your secrets using 1Password for simple secret storage.

```sh
touch secrets.yml
```

Copy below into secrets.yml.
Not all values are required.

```sh
telegram_bot_token: "********"
telegram_chat_id: "********"
private_key_path: "/Path/.ssh/id_rsa"
IP: 10.101.10.101
WALLET_PRIVATE_KEY: "********"
WALLET_ADDRESS: "Address"
GRAFANA_ADMIN_PASSWORD: "********"
server_name: "example-01.axol.io"
```

## Running Ansible Inventory

Inventory example

```sh
#IP of Host
[servers]
IP ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_rsa
```

To run the Ansible inventory, use the following command:

```sh
ansible-playbook -i inventory main.yml
```

This command will execute the playbook defined in `main.yml` using the hosts specified in the `inventory` file.

## Health Check examples

```sh
curl http://localhost:17690/health | jq

{
  "status": "OK",
  "started_at": "2024-10-13 09:44:39",
  "data_frame_version": "1.0.1",
  "order_proposal_version": "1.0.1",
  "app_version": "3.5.1",
  "display_name": "Axol",
  "beneficiary": "0xc74f57c8AB7Fd9f7dA37536dE3f75CB6763D32BE",
  "validator_address": "0xc74f57c8AB7Fd9f7dA37536dE3f75CB6763D32BE",
  "broker_url": "validator.testnet-3.elixirdev.xyz:9092",
  "api url": "https://api.testnet-3.elixir.xyz"
}

root@Elixir:~# curl https://api.testnet-3.elixir.xyz/health/ | jq

{
  "status": "up"
}
```

## Additional Resources

- [Ansible Documentation](https://docs.ansible.com/)
- [Ansible Galaxy](https://galaxy.ansible.com/)
- [Ansible Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)

## Contribution Guidelines

Please read our [contribution guidelines](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Changelog

All notable changes to this project will be documented in this file. See [CHANGELOG](CHANGELOG.md) for details.
