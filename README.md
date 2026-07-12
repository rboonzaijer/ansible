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

# Manage playbooks/roles

### How to know repository

```bash

#The old way:
#sudo add-apt-repository -r ppa:git-core/ppa

1. Open the Launchpad page.
2. Copy the fingerprint.
3. Construct: signed_by: https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x<FINGERPRINT>
4. Use the standard Launchpad URL pattern: uris: https://ppa.launchpadcontent.net/<owner>/<ppa>/ubuntu

# https://launchpad.net/~git-core/+archive/ubuntu/ppa
# Click: Technical details about this PPA
# Fingerprint = F911AB184317630C59970973E363C90F8F1B6217
# so: signed_by: https://keyserver.ubuntu.com/pks/lookup?op=get&search=0xF911AB184317630C59970973E363C90F8F1B6217
# url is already there: https://ppa.launchpadcontent.net/git-core/ppa/ubuntu
# name = the part of the url without slashes (git-core)
```
Create:
```yml
- name: Add apt repository for GIT
  ansible.builtin.deb822_repository:
    name: git-core
    types: [deb]
    uris: https://ppa.launchpadcontent.net/git-core/ppa/ubuntu
    suites: "{{ ansible_facts.distribution_release }}"
    components: [main]
    signed_by: https://keyserver.ubuntu.com/pks/lookup?op=get&search=0xF911AB184317630C59970973E363C90F8F1B6217
``` 
