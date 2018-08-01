Ansible
=======

* Ansible version: 2.6.1
* Python version: 2.7
* Location: `/opt/10forge.org/ansible`

Getting started
---------------

### Bootstrap

1. Install ansible: `pip install ansible==<ansible_version>`
1. Clone the repository to `/opt/10forge.org/ansible`
    ```bash
    git clone git@github.com:10forge/ansible.git /opt/10forge.org/ansible
    ```
1. Change into the repository path: `cd /opt/10forge.org/ansible`
1. Install roles: `ansible-galaxy install -r requirements.yml`
1. Create the vault password file: `echo '<vault_password> > .vault`
1. Run ansible.

### Run

Run a playbook from the root of the ansible repository. Look into a playbook at least once before using it to get a general feeling of what will happen.

```bash
ansible-playbook -i inventories/<inventory>.ini --limit <limit> --tags <tags> playbooks/<playbook>.yml
```

#### Mandatory options

* `-i / --inventories` Select between different customers. If no inventory is specified, only localhost will be available.

#### Optional options

* `-l / --limit` Limits the run to all hosts that match the limit string. This can be a group, a pattern or a comma separated list of hosts/patterns.

Maintenance
-----------

### Upgrading ansible version

When upgrading the ansible version for the repository there are multiple locations which need to be updated. Find all files by running `grep -iR "ansible_version"` from the repository root directory.

