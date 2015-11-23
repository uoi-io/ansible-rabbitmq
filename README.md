# Ansible RabbitMQ
Install RabbitMQ on CentOS 7 and Debian like distributions.

Two setups:
- As a RabbitMQ single node
- As a RabbitMQ HA cluster

## Requirements
None.

## Role Variables
### CONFIG
```
rabbitmq_firewalld: true
rabbitmq_erlang_cookie: 'AbCd3rl4ngC00k13WxYz'
rabbitmq_clustering: true
rabbitmq_cluster_name: 'rabbit'
rabbitmq_reset_cluster: false
rabbitmq_management_plugin: true
rabbitmq_ssl: false
rabbitmq_bind_address: 0.0.0.0
rabbitmq_port: 5672
rabbitmq_fd_limits: 4096
rabbitmq_vm_memory: 0.4
rabbitmq_vm_memory_ratio: 0.5
rabbitmq_mem_relative: 1.0
rabbitmq_heartbeat: 10
rabbitmq_admin_password: 'Myv3RY5tr0ngPa33w0RD'
```

### SSL
```
rabbitmq_cacert_file: etc/rabbitmq/ssl/cacert.pem
rabbitmq_cert_file: etc/rabbitmq/ssl/cert.pem
rabbitmq_key_file: etc/rabbitmq/ssl/key.pem
rabbitmq_ssl_bind_address: 0.0.0.0
rabbitmq_ssl_port: 5671
```

### VHOSTS
```
rabbitmq_vhost_list:
  - name: glance
  - name: nova
```

### USERS
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

## Dependencies
None.

## Example Playbook
Please have a look on the "Role Variables" section to have some examples.

## License
Apache

## Author Information
This role was created in 2015 by Gaëtan Trellu (goldyfruit).
