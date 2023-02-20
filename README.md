# opensql-lab

This directory consists of playbook, inventory, and vars directory. Hosts expressed in inventory are a set of settings that must be set in inventory when using a role. Settings for each host(e.g. 'primary', 'standby') are examples, and should be divided according to use and used appropriately.

The yml files in the vars directory are used as a list of 'vars_files' in the playbook, and can be used like a config file by declaring specific facts for each role. Even if you do not import the corresponding vars, it is possible to operate normally with the default settings.

Looking at the structure of playbook-example:

```text
├── playbook.yml
├── inventory.yml
└── vars
    ├── autotuning.yml
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
