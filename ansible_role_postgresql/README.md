Role Name
=========

Postgresql role for Ubuntu

Requirements
------------


Role Variables
--------------

defaults for postgresql-role: 

      postgresql_role: master

      postgresql_version: 14

      postgresql_additional_packages:
        - gnupg
        - python3-psycopg2

      postgresql_packages: 
        - "postgresql-{{ postgresql_version }}"
        - "postgresql-contrib-{{ postgresql_version }}"

      postgresql_repo_key: https://www.postgresql.org/media/keys/ACCC4CF8.asc

      postgresql_repo: "deb http://apt.postgresql.org/pub/repos/apt/ {{ ansible_distribution_release }}-pgdg main"
      postgresql_repo_filename: pgdg

      postgresql_service_name: postgresql.service
      postgresql_service_enabled: yes

      postgresql_postgres_password: postgres

      postgresql_users_hba_method: md5
      postgresql_replication_hba_method: md5

      postgresql_data_dir: "/var/lib/postgresql/{{ postgresql_version }}"
      postgresql_config_dir: "/etc/postgresql/{{ postgresql_version }}/main"
      postgresql_new_data_dir: /tmp/postgresql

      postgresql_users: #[]
        - name: AnnaSheludchenko
          password: 1234qwerty
          encrypted: true # defaults to true
          role_attr_flags: SUPERUSER # defaults to not set
          port: 5432 # defaults to 5432
        
        - name: IvanIvanov
          password: qwerty1234
          encrypted: true # defaults to true
          role_attr_flags: NOSUPERUSER # defaults to not set
          port: 5432 # defaults to 5432

      postgresql_databases: #[]
        - name: AnnaSheludchenkoDb
          encoding: # defaults to not set
          lc_collate: # defaults to not set
          lc_ctype: # defaults to not set
          conn_limit: # defaults to not set
          port: 5432 # defaults to not set
          # users configuration
          roles: AnnaSheludchenko
          privs: ALL # defaults to ALL

        - name: IvanIvanovDb
          encoding: # defaults to not set
          lc_collate: # defaults to not set
          lc_ctype: # defaults to not set
          conn_limit: # defaults to not set
          port: 5432 # defaults to not set
          # users configuration
          roles: IvanIvanov
          privs: ALL # defaults to ALL

Dependencies
------------


Example Playbook
----------------
**Pay Attention!**
You need to provide accessible IP addresses to `ansible_host` variable in inventory file:  

    [master]
    srv1 ansible_host=192.168.56.201


Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - name: Setup Master
      hosts: master
      become: true

      tasks:
        - name: "Include acme.ansible_role_postgresql for master role"
          include_role:
            name: ansible-role-postgresql
          vars:
            postgresql_role: master

    - name: Setup Replica
      hosts: replica
      become: true
      
      tasks:
        - name: "Include acme.ansible_role_postgresql for replica role"
          include_role:
            name: ansible-role-postgresql
          vars:
            postgresql_role: replica


License
-------

BSD

Author Information
------------------

Anna Sheludchenko, ITMO University
