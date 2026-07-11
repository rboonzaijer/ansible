# ansible-kubuntu

## Kubuntu 26.04

### Initial setup

```bash
sudo apt update -y
sudo apt purge ansible -y
sudo apt autoremove -y
sudo apt install -y git firefox
```

### Download 1password first with firefox

```bash
./usr/bin/firefox
```

### Configurue git

```bash
git config --global user.name "Roel"
git config --global user.email "10514742+rboonzaijer@users.noreply.github.com"

ssh-keygen -t ed25519 -C "rboonzaijer"
cat ~/.ssh/id_ed25519.pub
```

### Add to github

https://github.com/settings/ssh/new

### Install ansible

```bash
# Install ansible with python (not with apt!): https://docs.ansible.com/projects/ansible/latest/installation_guide/intro_installation.html
pipx install --include-deps ansible
pipx ensurepath
source ~/.bashrc
ansible --version

# Upgrade ansible to latest
pipx upgrade --include-injected ansible
```

### Get ansible playbook files

```bash
git clone git@github.com:rboonzaijer/ansible-kubuntu.git
cd ansible-kubuntu
```

### Run ansible playbook

```bash
ansible-playbook -i inventory.ini playbook-install-desktop.yml --ask-become-pass --limit local

ansible-playbook -i inventory.ini playbook-upgrade.yml --ask-become-pass --limit local
```

- --ask-become-pass # ask the sudo password before running
- --limit local # limit to local hosts

### Manual

- Add ssh-key:
  - github: https://github.com/settings/ssh/new
  - gitlab: https://gitlab.com/-/user_settings/ssh_keys
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
