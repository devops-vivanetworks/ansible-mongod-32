# shared/mongodb #

Ensures MongoDB 3.2 is installed and configured.

## Requirements ##

This role requires Ansible 2.0

## Role Variables ##

These variables are not defined by default, but can be passed to the role to modify installation behaviour.

    - name: mongodb_bind_ip
      desc: IP address MongoDB should to bind to

    - name: mongodb_keyfile
      desc: local path to keyfile used for replica set synchronization

    - name: mongodb_keyfile_content
      desc: content of the kefile for replica sync, overrides contents of `mongodb_keyfile` if both defined

    - name: mongodb_repl_set_name
      desc: MongoDB replica set name

    - name: mongodb_repl_oplog_size
      desc: replication OpLog size

### Defaults ##

    - name: mongodb_version
      value: 3.2.21
      desc: MongoDB version to install

    - name: mongodb_port
      value: 27017
      desc: TCP port MongoDB will listen on

    - name: mongodb_dbpath
      value: /var/lib/mongodb
      desc: path to db files

    - name: mongodb_logpath
      value: /var/log/mongodb/mongod.log
      desc: path to log

    - name: mongodb_keyfile_path
      value: /etc/mongodb_keyfile
      desc: default keyfile location

    - name: mongodb_storage_engine
      value: "wiredTiger"
      desc: the storage engine for mongod database

    - name: mongodb_journal_enabled
      value: true
      desc: whether to enable journaling

    - name: mongodb_profiling_threshold
      value: 100
      desc: minimum query duration before it is logged by profiler

    - name: mongodb_profiling_mode
      value: "off"
      desc: profiling mode (off|slowOp|all)

    - name: mongodb_enable_localhost_auth_bypass
      value: true
      desc: whether to enable local authentication bypass

    - name: mongodb_is_arbiter
      value: false
      desc: whether this node is an arbiter (if true disables journaling, enables small files)

    - name: mongodb_authorization_enabled
      value: true
      desc: whether to enable authorization

### Vars ###

This variables are not meant to be modified, but can be used by other roles.

    - name: mongodb_service_name
      desc: Name of the MongoDB service
      value: mongod

    - name: mongodb_user
      desc: name of the user MongoDB is running as
      value: mongodb

    - name: mongodb_group
      desc: name of the group MongoDB is running as
      value: mongodb

    - name: mongodb_packages
      desc: List of MongoDB packages to install
      value:
        - mongodb-org={{ mongodb_version }}
        - mongodb-org-server={{ mongodb_version }}
        - mongodb-org-shell={{ mongodb_version }}
        - mongodb-org-mongos={{ mongodb_version }}
        - mongodb-org-tools={{ mongodb_version }}

## Dependencies ##

None

## Example Playbook ##

Install standalone MongoDB

```
- hosts: all
  roles:
    - shared/mongodb-3.2
```

Install standalone MongoDB specifying port, IP and with journal disabled

```
- hosts: all
  roles:
    - role: shared/mongodb-3.2
      mongodb_port: 38875
      mongodb_bind_ip: 127.0.0.1
      mongodb_journal_enabled: false
```

Install replica MongoDB with custom oplog size

```
- hosts: all
  roles:
    - role: shared/mongodb-3.2
      mongodb_repl_set_name: replicaA
      mongodb_repl_oplog_size: 51200
      mongodb_keyfile: /local/path/to/keyfile
```

Install MongoDB arbiter

```
- hosts: all
  roles:
    - role: shared/mongodb-3.2
      mongodb_is_arbiter: true
      mongodb_repl_set_name: replicaA
      mongodb_keyfile: /local/path/to/keyfile
```

Install MongoDB with specific version (3.2.21)

```
- hosts: all
  roles:
    - role: shared/mongodb-3.2
      mongodb_repl_set_name: replicaA
      mongodb_repl_oplog_size: 51200
      mongodb_keyfile: /local/path/to/keyfile
      mongodb_version: 3.2.21
