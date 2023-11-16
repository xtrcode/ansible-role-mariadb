Role Name
=========

A customizable ansible-role to deploy MariaDB.

Requirements
------------

`gather_facts` must be enabled to calculate the default memory configuration for MariaDB (see Role Variables).

Role Variables
--------------


Most-used variables are listed here. See all available in `defaults/main.yml`.
```yaml
mysql_user: "mysql"
mysql_group: "mysql"
```
Default user and group, usually created automatically during installation.

```yaml
mysql_python_package_debian: python3-mysqldb
```

Ansible dependency for Debian 12.

```yaml
mysql_port: "0.0.0.0"
mysql_bind_address: "3306"
```
Default port and listening address.


```yaml
mysql_data_dir: "{{ mysql_data_dir_default }}"
mysql_data_dir_move: false
```
Controls weather the data directory should be moved upon first-run. Leave as is, to keep the default location.

_When the data dir is moved, a lock-file is placed in the default location (src) to prevent additional moves on consecutive runs_


```yaml
# Databases.
mysql_databases: []
#   - name: example
#     collation: utf8_general_ci
#     encoding: utf8
#     replicate: 1

# Users.
mysql_users: []
#   - name: example
#     host: 127.0.0.1
#     password: secret
#     priv: *.*:USAGE
```


**DANGER ZONE**
```yaml
# Slow query log settings.
mysql_slow_query_log_enabled: true
mysql_slow_query_time: "2"

# Memory settings (default values optimized ~512MB RAM).
mysql_key_buffer_size: "{{ (ansible_memtotal_mb * 0.5) | int | abs }}MB"
mysql_max_allowed_packet: "{{ (ansible_memtotal_mb / 8) | int | abs }}M"
mysql_myisam_sort_buffer_size: "{{ (ansible_memtotal_mb / 8) | int | abs }}M"
mysql_query_cache_size: "{{ (ansible_memtotal_mb / 16) | int | abs }}M"
mysql_tmp_table_size: "{{ (ansible_memtotal_mb / 16) | int | abs }}M"
mysql_max_heap_table_size: "{{ (ansible_memtotal_mb / 16) | int | abs }}M"

# InnoDB settings.
mysql_innodb_buffer_pool_size: "{{ (ansible_memtotal_mb * 0.5) | int | abs }}MB"
mysql_innodb_log_file_size: "{{ (ansible_memtotal_mb * 8) | int | abs }}MB"

mysql_mysqldump_max_allowed_packet: "{{ (ansible_memtotal_mb * 8) | int | abs }}MB"
```

 These are _my_ suitable defaults. Feel free to change to your requirements.

Dependencies
------------

```yaml
- name: geerlingguy.mysql
```

Example Playbook
----------------


### Install locally
```yaml
# requirements.yml
- src: git+ssh://git@github.com/xtrcode/ansible-role-mariadb.git
  scm: git
```

```bash
$ ansible-galaxy install -r requirements.yml -p ./roles
```

### Playbook
```yaml
- hosts: deploy
  roles:
    - { role: ansible-role-mariadb }
```

License
-------

MIT 

Author Information
------------------

Website: [blog.xtracode.ws](https://blog.xtracode.ws)
