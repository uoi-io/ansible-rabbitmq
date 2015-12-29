# Ansible RabbitMQ
This module provides a support for the installation of RabbitMQ.

Supported distributions:
- Centos 7
- Debian 7/8

Two methods are supported:
- Deploy a RabbitMQ single node
- Deploy a RabbitMQ HA cluster

## Requirements
Ansible 1.9

## Role Variables
rabbitmq_fd_limits will increase the file descriptors limit for the RabbitMQ service.
### CONFIG
```
rabbitmq_admin_password: 'Myv3RY5tr0ngPa33w0RD'
rabbitmq_firewalld: true
rabbitmq_management_plugin: true
rabbitmq_bind_address: 0.0.0.0
rabbitmq_port: 5672
rabbitmq_fd_limits: 4096
rabbitmq_vm_memory: 0.4
rabbitmq_vm_memory_ratio: 0.5
rabbitmq_mem_relative: 1.0
rabbitmq_heartbeat: 10
```

### CLUSTER
Two more variables should be declared when clustering is used:
```
master: node-1
rabbitmq_master_name: node-1
```
These two variables allow you to define who is the master and which name should have the master node.

When rabbitmq_reset_cluster variable is set to true, the task will destroy the current cluster and wipe all data related to this cluster. 
```
rabbitmq_clustering: true
rabbitmq_cluster_name: 'rabbit'
rabbitmq_reset_cluster: false
rabbitmq_erlang_cookie: 'AbCd3rl4ngC00k13WxYz'
```

### SSL
If you want to use SSL, you will have to generate your own certificate.
```
rabbitmq_ssl: false
rabbitmq_cacert_file: etc/rabbitmq/ssl/cacert.pem
rabbitmq_cert_file: etc/rabbitmq/ssl/cert.pem
rabbitmq_key_file: etc/rabbitmq/ssl/key.pem
rabbitmq_ssl_bind_address: 0.0.0.0
rabbitmq_ssl_port: 5671
```

### VHOSTS
Please have a look on the rabbitmq_vhost module: http://docs.ansible.com/ansible/rabbitmq_vhost_module.html
```
rabbitmq_vhost_list:
  - name: glance
  - name: nova
```

### USERS
Please have a look on the rabbitmq_user module: http://docs.ansible.com/ansible/rabbitmq_user_module.html
```
rabbitmq_users_list:
  - vhost: glance
    user: glance
    password: "{{ rabbitmq_glance_password | default('gl4nc3p4ssw0rd') }}"
  - vhost: /
    user: admin
    password: "{{ rabbitmq_admin_password }}"
    tags:
    - administrator
```

### POLICIES
Please have a look on the rabbitmq-policy module: http://docs.ansible.com/ansible/rabbitmq_policy_module.html
```
rabbitmq_policies_list:
  - name: HA-Glance
    pattern: .*
    tags:
      ha-mode: all
    vhost: glance
  - name: HA
    pattern: .*
    tags:
      ha-mode: all
    vhost: /
```

## Dependencies
None.

## Example Playbook
Please have a look on the "Role Variables" section to have some examples.

### Single node
```
rabbitmq_admin_password: 'Myv3RY5tr0ngPa33w0RD'
rabbitmq_firewalld: true
rabbitmq_management_plugin: true
rabbitmq_bind_address: 0.0.0.0
rabbitmq_port: 5672

rabbitmq_users_list:
  - vhost: /
    user: admin
    password: "{{ rabbitmq_admin_password }}"
    tags:
    - administrator
```

### Cluster
```
master: node-1
rabbitmq_master_name: node-1
rabbitmq_admin_password: 'Myv3RY5tr0ngPa33w0RD'
rabbitmq_firewalld: true
rabbitmq_management_plugin: true
rabbitmq_bind_address: 0.0.0.0
rabbitmq_port: 5672

rabbitmq_users_list:
  - vhost: /
    user: admin
    password: "{{ rabbitmq_admin_password }}"
    tags:
    - administrator

rabbitmq_vhost_list:
  - name: glance

rabbitmq_policies_list:
  - name: HA-Glance
    pattern: .*
    tags:
      ha-mode: all
    vhost: glance
  - name: HA
    pattern: .*
    tags:
      ha-mode: all
    vhost: /
```

## License
Apache

## Author Information
This role was created in 2015 by Gaëtan Trellu (goldyfruit).
