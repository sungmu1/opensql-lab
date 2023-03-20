# opensql-lab
11f
This Repository consists of playbook, inventory, and vars directory. Hosts expressed in inventory are a set of settings that must be set in inventory when using a role. Settings for each host(e.g. 'primary', 'standby') are examples, and should be divided according to use and used appropriately.

The yml files in the vars directory are used as a list of 'include_vars' in the playbook, and can be used like a config file by declaring specific facts for each role. Even if you do not import the corresponding vars, it is possible to operate normally with the default settings.

Looking at the structure of opensql-lab:

```text
├── .gitmodules
├── inventory.yml
├── playbook.yml
├── README.md
└── vars
    ├── autotuning.yml11
    ├── init_dbserver.yml
    ├── install_dbserver.yml
    ├── manage_dbserver.yml
    ├── manage_extension.yml
    ├── manage_pgbouncer.yml
    ├── setup_barman.yml
    ├── setup_barmanserver.yml
    ├── setup_pgbouncer.yml
    ├── setup_pgpool2.yml
    ├── setup_replication.yml
    ├── setup_repmgr.yml
    └── setup_repo.yml
```

# Requirement

* Ansible >= 2.11
* opensql galaxy collection
* python >= 2.6 (python >= 3.8)
* SSH Client (Managed Node : SSH Server)

# variable yml files

"#" commented variables is not default variable. This is only used when defined to the user.
Anything not commented out with "#" is the value the variable is set to by default.
You can define it by uncommenting it or changing the value.

This is an example of modifying setup_repo.yml by forcibly reinstalling the repository
and adding epel to the yum repository.
Of course there is no difference in behavior since it installs epel repo by default.

```yml
# If force_repo is true, delete repo related postgresql repo(epel, pgdg) .
force_repo: true
#
# This variable run only RedHat.
yum_additional_repos:
 - name: "epel"
   description: "EPEL YUM repo"
   baseurl: "https://download.fedoraproject.org/pub/epel/$releasever/$basearch/"
   gpgcheck: "no"
```


# Tutorial (How to working)

Read and execute the yml files in the vars directory using the 'include_vars' module in the playbook.
See the [include_vars](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/include_vars_module.html) module page for details.

The example below is the operation of setup_repo, install_dbserver, and init_dbserver on the primary server.

- inventory.yml
    ```yml
    all:
      children:
        primary:
          hosts:
            primary1:
            ansible_host: 192.168.0.2
            private_ip: 192.168.0.2
    ```

- playbook.yml
    ```yml
    - hosts: all
      name: Postgres deployment playbook
      become: true
      gather_facts: true

      collections:
        - tmax_opensql.postgres

      pre_tasks:
        - name: Initialize the user defined variables
          set_fact:
            pg_version: 14.6
            pg_type: "PG"
            disable_logging: false
        - name: Set variable
          ansible.builtin.include_vars:
            dir: vars
            ignore_unknown_extensions: True
            extensions:
              - 'yml'

      roles:
        - role: setup_repo
        - role: install_dbserver
        - role: init_dbserver
    ```
