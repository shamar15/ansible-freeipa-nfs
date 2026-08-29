# Ansible-freeipa-nfs

Ansible automation to deploy a FreeIPA identity management environment with NFS-backed roaming home directories across RHEL/CentOS Linux clients.

---

## Overview

This project builds a Linux identity management lab using Ansible, FreeIPA, NFS, and autofs. The automation deploys a FreeIPA server, enrolls three client systems into the FreeIPA domain, provisions FreeIPA users, configures per-user NFS home host assignments, and validates that users can access the same home directory from any enrolled client machine.

The goal is to demonstrate repeatable Linux infrastructure automation with a focus on identity management, centralized authentication, NFS-backed home directory access, autofs /net path resolution, and role-based Ansible design.

---

## Architecture

                ┌─────────────────────────────┐
                │      ansible-control-node   │
                │   (runs all playbook cmds)  │
                └──────────────┬──────────────┘
                               │ SSH
                ┌──────────────┴──────────────┐
                │                             │
      ┌─────────▼──────────┐     ┌────────────▼────────────┐
      │   ipa-server       │     │  ipa-client1            │
      │   FreeIPA Server   │◄────│  ipa-client2            │
      │   NFS Server       │ NFS │  ipa-client3            │
      │                    │     │  (FreeIPA Clients)      │
      └────────────────────┘     └─────────────────────────┘
      Kerberos · LDAP · DNS        autofs → /net/-hosts
      exports /home                mounts on demand



---

## Technologies Used

- **Ansible** — automation engine and orchestration
- **FreeIPA** — identity management (LDAP, Kerberos, DNS)
- **NFS** — centralized home directory storage
- **autofs** — on-demand automatic NFS mounting
- **RHEL 9 / CentOS Stream** — operating system
- **Ansible Vault** — secrets encryption
- **GitHub** — version control

---
## Repository Structure

```
.
├── ansible.cfg
├── docs/
│   └── test_results.txt
├── group_vars/
│   ├── all.yml
│   ├── ipaclients.yml
│   └── users_vault.yml
├── inventory/
│   └── hosts.ini
├── playbooks/
│   ├── site.yml
│   └── create_users.yml
├── roles/
│   └── nfs_server/
│       ├── defaults/
│       ├── handlers/
│       ├── tasks/
│       ├── templates/
│       └── vars/
├── ansible.cfg
├── requirements.yml
├── LICENSE
└── README.md
```
---

## Inventory Design

The lab uses one FreeIPA server and three FreeIPA client systems. The client systems also act as designated NFS home hosts for assigned users.

```ini
[ipaserver]
ipa-server.lab.ipa

[ipaclients]
ipa-client1.lab.ipa
ipa-client2.lab.ipa
ipa-client3.lab.ipa

[nfsserver]
ipa-server.lab.ipa

[lab:children]
ipaserver
ipaclients
```

---

## User Home Directory Design

Each FreeIPA user is assigned a designated NFS home host. The user's FreeIPA homedir attribute points to a /net/HOSTNAME/home/USERNAME path, allowing the user to log into any enrolled client and access the same NFS-backed home directory through autofs.

| User  | Assigned NFS Home Host  | FreeIPA Home Directory                      |
|-------|-------------------------|---------------------------------------------|
| alice | ipa-client1.lab.ipa     | /net/ipa-client1.lab.ipa/home/alice         |
| bob   | ipa-client3.lab.ipa     | /net/ipa-client3.lab.ipa/home/bob           |

When either user logs in from any enrolled client, autofs resolves the /net path back to the assigned home host automatically.

---

## What This Automation Builds

- Deploys a FreeIPA server (Kerberos, LDAP, DNS)
- Enrolls three RHEL client systems into the FreeIPA domain
- Provisions FreeIPA users with Vault-backed credentials
- Configures FreeIPA user homedir attributes to /net paths
- Creates physical home directories on designated home hosts
- Configures NFS /home exports on client machines
- Configures autofs with the /net -hosts map for roaming access
- Sets the SELinux boolean required for NFS-backed home directories
- Opens required firewall services for NFS traffic
- Configures NFSv4 ID mapping with the lab.ipa domain
- Validates cross-client roaming home directory access for alice and bob

---

## Prerequisites

- RHEL 9 systems with network connectivity between all nodes
- SSH key-based authentication from control node to all managed hosts
- Python 3 installed on all managed nodes
- ansible-core installed on the control node
- Required Ansible collections installed from requirements.yml
- Inventory and Vault variables configured for your environment

---

## How to Run

```bash
# 1. Clone the repository
git clone git@github.com:shamar15/ansible-freeipa-nfs.git
cd ansible-freeipa-nfs

# 2. Install required Ansible collections
ansible-galaxy collection install -r requirements.yml -p ./collections

# 3. Update inventory with your actual hostnames or IPs
vi inventory/hosts.ini

# 4. Update group_vars/all.yml with your domain and network settings
vi group_vars/all.yml

# 5. Create your vault file with sensitive credentials
ansible-vault create group_vars/vault.yml

# 6. Run the full deployment
ansible-playbook playbooks/site.yml --ask-vault-pass

# 7. Create FreeIPA users with home directory assignments
ansible-playbook playbooks/create_users.yml --ask-vault-pass

# 8. Run only NFS/autofs configuration
ansible-playbook playbooks/site.yml --tags nfs --limit ipaclients --ask-vault-pass
```

---

## Security Design

Sensitive values are stored with Ansible Vault and never hardcoded in playbooks.

Vault-backed values include:
- FreeIPA admin password
- Directory Manager password
- FreeIPA user passwords

The user creation task uses `no_log: true` to prevent passwords from appearing in Ansible output. Non-sensitive identity data such as usernames and display names is stored separately from encrypted values.

---

## Validation

Roaming home directory validation confirmed:

- FreeIPA users alice and bob authenticate successfully from all three client machines
- FreeIPA homedir attributes point to the correct /net paths
- Physical home directories exist on the assigned home hosts
- autofs resolves /net paths correctly from all enrolled FreeIPA clients
- Test files written on one client persist and are accessible from other clients
- NFS exports, autofs, SELinux, and file ownership all work together correctly

Detailed results are documented in `docs/test_results.txt`.

---

## Skills Demonstrated

- Linux identity management with FreeIPA (LDAP, Kerberos, DNS)
- RHEL 9 system administration
- Ansible playbook and role development
- Ansible Vault for secrets management
- FreeIPA user and client automation via the freeipa.ansible_freeipa collection
- NFS server configuration and export management
- autofs /net -hosts map configuration for roaming home directories
- SELinux boolean management for NFS home directory access
- firewalld service management
- Jinja2 templating for dynamic configuration files
- Handler-based service reloads triggered only on configuration changes
- Cross-client validation and infrastructure documentation
- Git-based version control with meaningful commit history

---

