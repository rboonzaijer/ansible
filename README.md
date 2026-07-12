# ansible

Ansible playbooks to quickly set up a fresh minimal installation with all required development tools and configuration.

- Minimal installation
- Kubuntu 26.04

## Initial setup

```bash
# Make sure ansible is not installed with apt
sudo apt update -y
sudo apt purge ansible -y
sudo apt autoremove -y

# Install ansible with python
# https://docs.ansible.com/projects/ansible/latest/installation_guide/intro_installation.html
sudo apt install pipx -y
pipx install --include-deps ansible
pipx ensurepath
source ~/.bashrc
ansible --version

# Upgrade ansible
pipx upgrade --include-injected ansible
```

## Download playbooks

```bash
curl -L -o ansible.zip https://github.com/rboonzaijer/ansible/archive/refs/heads/main.zip

unzip ansible.zip -d ~/ansible-zip

cd ~/ansible-zip/*/
```

## Run

```bash
# Debug variables
ansible-playbook -i inventory.ini --ask-become-pass playbook-debug.yml

# Install desktop tools
ansible-playbook -i inventory.ini --ask-become-pass playbook-desktop.yml --limit local_desktop

# Update/Upgrade
ansible-playbook -i inventory.ini --ask-become-pass playbook-upgrade.yml --limit local_desktop

# After installation, reload profile/path
source ~/.bashrc

# Cleanup
cd ~
rm ~/ansible.zip
rm -r ansible-zip/
```

## Add SSH-Key to git accounts

```bash
cat ~/.ssh/id_ed25519.pub
```
- github: https://github.com/settings/ssh/new
- gitlab: https://gitlab.com/-/user_settings/ssh_keys

```bash
docker login
npm login
```

Also:

- 1password: login
- chrome: login
- firefox: login
- vscode: signin with github (sync)
