# Ansible Cosmos Network Creator
✨ An Ansible toolkit for Cosmos networks 💫

Use this toolkit to:

- Join a testnet
- Start a local testnet
- Start a multi-node testnet

![Waterdrops feeding seedlings](images/seedling.gif)

## 🌰 Requirements 

- Python 3
- Ansible 
  - Install Ansible with `pip` and not `apt`:
    ```
    pip install ansible
    ```

## 🌱 Quick Start

To join the Cosmos Hub [public testnet](https://github.com/cosmos/testnets/tree/master/public):

1. Clone this repository
2. Run `ansible-galaxy install -r requirements.yml` to install dependencies 
3. Set up SSH access to the target machine
4. Run the playbook
   ```
   ansible-playbook node.yml -i examples/inventory-public-testnet.yml -e 'target=SERVER_IP_OR_DOMAIN'
   ```
5. Log into the target machine to follow the syncing process
   ```
   journalctl -fu cosmovisor
   ```

Watch the video below to see the playbook in action:

[![Join the Cosmos Hub Public Testnet](https://img.youtube.com/vi/4KkMblQ6wcY/0.jpg)](https://youtu.be/4KkMblQ6wcY)

## 🌳 Explore Further

- See the [examples](examples/README.md) for more command, playbook, and configuration options.
- See the [Playbook Variables Overview](docs/Playbook-Variables.md) for a list of default variables you can override with the `--extra-vars` or `-e` option.
- Visit the [Cosmos testnets repo](https://github.com/cosmos/testnets) for more information.

### Playbook Tags

Use `cascadia_control.py` to run only part of the `cascadia` playbook:

```
./cascadia-control.py [-i inventory] [-t target] operation
```

The inventory argument is optional and defaults to `inventory.yml` (e.g. `./cascadia-control.py restart`).

The target option is the server IP or domain.

The operation will apply to all the nodes in the inventory:
- `restart` restarts the cascadiad/cosmovisor service
- `stop` stops the cascadiad/cosmovisor service
- `start` starts thecascadiad/cosmovisor service
- `reboot` reboots the machine
- `reset` runs `cascadiad unsafe-reset-all`

### Role Folder Structure

- The `cascadia` role provides the core functionality of this toolkit
- Node setup: `roles/cascadia/tasks/main.yml`
- Default variables: `roles/cascadia/defaults/main.yml`
- Systemd services: `roles/cascadia/templates/`
- To add a variable to the cascadia config files, add it to:
  - `roles/cascadia/templates/ansible_vars.json.j2`  

## 🌴 Automatic Tests

This repository runs different tests automatically as defined below.

### Fresh State (weekly)

The fresh state test is run using GitHub Actions and results are displayed with a badge at the top of this readme.

### Mainnet exported genesis (bi-weekly)

We export a genesis file from `cosmoshub-4` and modify it using our [tinkerer script](https://github.com/hyphacoop/cosmos-genesis-tinkerer). The exported and modified genesis files can be accessed [here](https://files.polypore.xyz/genesis/).

We run the stateful tests with the modified genesis file when there is a major version of Cascadia that is higher than the major version running on `cosmoshub-4`.

### Joining the Public Testnet (weekly)

We test joining the Cosmos Hub public testnet weekly using GitHub Actions and a badge is displayed at the top of this readme.

## 🔎 Code Standards

- All Python code is formatted to PEP 8 and linted with `pylint`.
- All YAML code is linted with `yamllint`.
- See `lint.sh` and `.config/` for details.
