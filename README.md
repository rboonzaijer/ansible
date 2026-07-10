# ansible-kubuntu

```bash
sudo apt update -y
sudo apt install -y git ansible firefox

./usr/bin/firefox

# Download 1password first...

git config --global user.name "Roel"
git config --global user.email "10514742+rboonzaijer@users.noreply.github.com"

ssh-keygen -t ed25519 -C "rboonzaijer"
cat ~/.ssh/id_ed25519.pub

# Add to github
https://github.com/settings/ssh/new

git clone git@github.com:rboonzaijer/ansible-kubuntu.git
cd ansible-kubuntu

ansible-playbook \
    --inventory inventory.ini \
    --ask-become-pass \
    playbook.yml
```
