Bootstrap
=========

This role must used to bootstrap Odoo Database. So when the server is up, we can manage Database easily and make some few actions:

* **create_database**: to create an empty database with some basic parameters like database_name, super_admin_password, odoo_language

* **delete_database**: to delete a list of Odoo databases (look at the `odoo_database_name_to_detele` variable)

* **change_super_user_password**: to change the default super user password to prevent security issue, it's the first action to make before making any other. By default the super_user_password is set to "admin" (look at the `odoo_super_admin_password` and `odoo_new_super_admin_password` variable) - WARNING /!\

* **backup_database**: to backup our Odoo database (look at the `odoo_database_backup_file` and `odoo_database_name` variables)

* **restore_database**: to restore database using a provided ZIP dump file of Odoo database (look at the `odoo_database_restore_file` variable)

`NB`: To easily use this role, every single action are tasks with the corresponding tags (tags: create_database, restore_database ...)

If you need to boostrap Odoo service (deploy an Odoo instance and configure it) you can refer to this [ansible-odoo](https://github.com/OCA/ansible-odoo) role written by the [OCA](https://github.com/OCA).
This role only take care of the databases management.

Odoo version tested
-------------------
-> 10.0

Requirements
------------

This role is based on Odoo api called [ODOORPC](https://github.com/OCA/odoorpc). Big thanks to [OCA](https://github.com/OCA)
You can install it with a simple `pip install odoorpc` 

Role Variables
--------------

There are a lot of variables, so to see all of them plase check the `default/main.yml` where i present variable with description and cautions

Dependencies
------------

.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      user: root
      become: yes
      gather_facts: false

      pre_tasks:
        - block:
            - name: "Check connectivity"
             ping: ~
          rescue:
            - debug:
                msg: "Module ping: error"
            - raw: bash -c "test -f /usr/bin/python || (apt-get update && apt-get install python python3 -yqq) || (yum -y install python)"
              register: output
          always:
            - setup: ~

      roles:
        - { role: bootstrap, tags: ['change_super_admin_password', 'restore_database'] }

License
-------

BSD

Author Information
------------------

- Dirane TAFEN (diranetafen@yahoo.com)
