Ansible
=======

* Ansible version: 2.6.1
* Python version: 2.7


Getting started
---------------

### Bootstrap

1. Install ansible: `pip install ansible==<ansible_version>`
1. Clone the repository: `git clone git@github.com:10forge/ansible.git`
1. Enter the repository: `cd ansible`
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


Playbooks
---------

A playbook is a set of tasks that will be executed on a host. The name of a playbook consists of a prefix and a summary of it's included tasks.

### Prefixes

* **bootstrap** Self containing playbooks that are only used once for bootstrapping a specific environment on the host. Playbooks with the bootstrap prefix have no dependencies to external variables or roles and could easily be run outside of the repository without failure.
* **manage** Playbooks that cope with often changing configurations (e.g. adding/removing entities).
* **provision** Playbooks that contain multiple other playbooks to create a fully functional and configured host with one ansible run. They are mostly used once at the first provisioning phase of a new host since the amount of tasks they contain can be hard to track and the impact on the host may be unpredictable.
* **purge** Playbooks that remove specific configurations. Often there is a **setup** playbook with the same summary to fill the gap.
* **setup** Playbooks that add specific configurations. When creating new playbooks this is usually the one you want to create.


Variables
---------

Hosts are configured by adding variables to files contained inside `inventories/group_vars` and `inventories/host_vars`whereby every group and every host has it's own directory containing a `vars.yml` and a `secrets.yml`. The `secrets.yml` file contains all classified variables like passwords or keys and **must** be encrypted with ansible-vault.

### secrets.yml

Every group can contain a `secrets.yml` file containing all classified variables of the group which then can be easily used in the `vars.yml` file per variable reference (`"{{ variable-name }}"`). To reduce name conflicts all variables within the `secrets.yml` must have the group name as prefix.

### ansible-vault

To use ansible vault inside this repository there are 2 conditions to be met:

* the `.vault` file has to containt the vault password and must reside in the root directory of the repository
* all actions must be executed from the root directory of the repository (since the `ansible.cfg` needs to be read)

More information can be found here: https://docs.ansible.com/ansible/latest/user_guide/vault.html

#### Encrypt a file

```bash
ansible-vault encrypt foo.yml bar.yml baz.yml
```

#### Edit a file

```bash
ansible-vault edit foo.yml bar.yml baz.yml
```

#### Rekey a file

When the vault password changes (e.g. in case of leackage) all vault encrypted files need to be rekeyed with the new vault password.

```bash
ansible-vault rekey foo.yml bar.yml baz.yml
```

#### Decrypt a file

```bash
ansible-vault decrypt foo.yml bar.yml baz.yml
```

