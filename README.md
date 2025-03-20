# ansible-debian-starter 🚀
An Ansible playbook for setting up a secure Debian server on a server with essential configurations, including user management, Docker installation, and enhanced security measures like ufw, fail2ban, and IP blocklists.

## Configuration

Check out the repository.

Edit the file `hosts` by adding the IP addresses of your servers:

```shell
[servers]
server1 ansible_host=1.2.3.4
```

Edit the file `playbook.yml` and replace all occurrences of `username` with your username.

Copy your SSH public key to the current folder and name it `id_rsa_username.pub`, where `username` is your username.

## Basic setup

The basic setup contains:

- System Updates and Package Management: Updates the apt package cache, upgrades installed packages, and ensures a base set of software packages are installed.
- Docker Installation: Installs required system packages for Docker, adds Docker's official GPG key, sets up the Docker repository, and installs Docker and related packages.
- User and SSH Configuration: Creates users with sudo privileges, ensures authorized keys for remote users, disables root login via SSH, and sets the SSH port number.
- Security Enhancements: Configures UFW (Uncomplicated Firewall) to manage incoming and outgoing connections, enables UFW logging, starts and enables the fail2ban service, and imports tasks for IP set blacklist.
- System Hardening: Hardens the TCP/IP stack against SYN floods and makes users passwordless for sudo.
- Service Management: Checks if any services need to be restarted, reboots the server if needed, and restarts necessary services like cron, fail2ban, sshd, networking, and UFW.
- Cleanup: Removes old packages from the cache and dependencies that are no longer needed. 
 
Run the playbook the first time:

```shell
ansible-playbook -i hosts -u root --ask-pass playbook.yml
```

Add `-l server1` to only run the playbook on the server with the name `server1`.

After the first run, the ssh port is changed and the root user disabled.

If you want to run the playbook again, use the following command:

```shell
ansible-playbook -i hosts playbook.yml -e "ansible_port=22222"
```

## Docker Login

Run the following playbook to log into DockerHub:

```shell
ansible-playbook -i hosts docker-login.yml -e "ansible_port=22222"
```

## Rclone Login

Install rclone on your local machine:

```shell
brew install rclone
```

Create a new configuration on your local machine:

```shell
rclone config
```

(n, name, drive, client_id, client_secret, 1, enter, n, y, n, y, q)

Run the following playbook to copy the configuration to the server:

```shell
ansible-playbook -i hosts rclone-login.yml -e "ansible_port=22222"
```

## Mail setup

Run the following playbook so that the server can send mails:

```shell
ansible-playbook -i hosts mail-setup.yml -e "ansible_port=22222"
```
