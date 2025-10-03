# Automated Network Configuration with Ansible


This repo contains a minimal, working example of using Ansible to automate network-related tasks across Ubuntu servers. The project demonstrates three roles:


* **bind** - deploys BIND9 DNS server and a simple zone file
* **firewall** - configures UFW firewall rules
* **users** - creates a common deployment user and installs an SSH key


## Topology (example)


* Controller (Ansible): 192.168.24.10
* DNS server (dns_servers): 192.168.24.20
* Firewall server (firewalls): 192.168.24.21


## Setup steps (quick)


1. Clone the repo to your controller machine.
   
2. Ensure managed nodes have Python 3 and SSH enabled.
   
3. Generate an SSH key on the controller and copy it to managed nodes:


```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_ansible -N ""
ssh-copy-id -i ~/.ssh/id_ed25519_ansible.pub ubuntu@192.168.24.20
ssh-copy-id -i ~/.ssh/id_ed25519_ansible.pub ubuntu@192.168.24.21

4. Edit inventory/hosts.ini to match your IPs and usernames.

5. (Optional) Create vaulted vars with ansible-vault create group_vars/all_vault.yml.

6. Run a ping test:

ansible all -m ping

7. Dry-run:

ansible-playbook playbooks/site.yml --check --diff

8. Apply the configuration:

ansible-playbook playbooks/site.yml -v

9. Re-run to show idempotency:

ansible-playbook playbooks/site.yml -v
