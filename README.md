# ansible-kubuntu

```bash
sudo apt update -y
sudo apt install -y git ansible

git clone https://rboonzaijer@github.com/rboonzaijer/ansible-kubuntu.git
cd ansible-kubuntu

ansible-playbook \
    --inventory inventory.ini \
    --ask-become-pass \
    playbook.yml
```
