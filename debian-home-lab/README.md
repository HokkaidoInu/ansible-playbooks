# My Debian Home Lab Ansible Playbook

This is my personal, opinionated Ansible playbook for setting up a Debian home lab with Dockge, Jellyfin, and Firefly III. Feel free to use it as inspiration for your own setup, but please note that this playbook is tailored to my specific needs and preferences.

## Prerequisites

### Host System (Arch Linux)

1. Update your Arch Linux system:
   ```
   sudo pacman -Syu
   ```

2. Install Ansible and other required packages:
   ```
   sudo pacman -S ansible openssh
   ```

3. Install `yay` if not already installed:
   ```
   git clone https://aur.archlinux.org/yay.git
   cd yay
   makepkg -si
   ```

4. Install additional AUR packages if needed:
   ```
   yay -S ansible-lint
   ```

### Target System (Debian)

1. Update your Debian system:
   ```
   sudo apt-get update
   sudo apt-get upgrade
   ```

2. Install required packages:
   ```
   sudo apt-get install openssh-server python3 python3-pip
   ```

### SSH Setup

1. Generate an SSH key pair on your Arch Linux host if you haven't already:
   ```
   ssh-keygen -t ed25519 -C "debian-homelab"
   ```

2. Copy your public key to the Debian target system:
   ```
   ssh-copy-id your_ssh_user@your_server_ip_or_hostname
   ```

3. Test the SSH connection:
   ```
   ssh your_ssh_user@your_server_ip_or_hostname
   ```

4. (Optional) Configure SSH key-based authentication only:
   Edit `/etc/ssh/sshd_config` on the Debian target system and set:
   ```
   PasswordAuthentication no
   ```
   Then restart the SSH service:
   ```
   sudo systemctl restart sshd
   ```

### Additional Prerequisites

- A Debian server with SSH access configured as above
- Docker installed on the Debian server (the playbook will install it if not present)

## Setup

1. Clone the repository containing this playbook:
   ```
   git clone https://github.com/HokkaidoInu/ansible-playbooks.git
   cd ansible-playbooks/debian-home-lab
   ```

2. Copy the example vault file and edit it with your Firefly III settings:
   ```
   cp example_vault.yml vault.yml
   ansible-vault edit vault.yml
   ```

3. Copy the example inventory file and edit it with your server details:
   ```
   cp example_inventory.ini inventory.ini
   ```
   Edit `inventory.ini` with your preferred text editor and replace `your_server_ip_or_hostname` and `your_ssh_user` with your actual server IP or hostname and SSH username.

4. Review and modify the `homelab_playbook.yml` file if needed, especially the volume paths for each service.

## Usage

To run the playbook:

```
<!-- ansible-playbook -i inventory.ini homelab_playbook.yml --ask-vault-pass -->
ansible-playbook -i inventory.ini homelab_playbook.yml -kK
```

This will prompt you for the vault password and then run the playbook.

## What's Included

- **Dockge**: A Docker compose management tool (accessible on port 5001)
- **Jellyfin**: A media server (accessible on port 8096)
- **Firefly III**: A personal finance manager (accessible on port 8080)

## Customization

- Modify the `vars` section in `homelab_playbook.yml` to change versions or add more variables.
- Adjust the Docker container configurations (ports, volumes, etc.) in the playbook as needed.

## Security Notes

- The `vault.yml` file is ignored by git to prevent accidentally committing sensitive information. Always keep your `vault.yml` file secure.
- The actual `inventory.ini` file is also ignored by git to prevent accidentally committing server details. Use the `example_inventory.ini` as a template.
- Consider implementing additional security measures such as firewalls and regular system updates.

## Backup

Remember to regularly backup the data directories specified in the volume mappings to prevent data loss.

## Disclaimer

This playbook is provided as-is, without any warranties or guarantees. It's designed for my personal use, and while you're welcome to use it, please understand that it may not fit your specific needs or environment without modifications.