Ansible Filebeat role
=========

cd to level up than ansible-filebeat and run below command:
ansible-playbook --private-key ~/.ssh/use-prod-dcos-agent.pem ansible-filebeat/test/integration/default/default.yml

[![Ansible Galaxy](https://img.shields.io/badge/galaxy-DavidWittman.filebeat-blue.svg?style=flat)](https://galaxy.ansible.com/detail#/role/6293)

Installs Elastic's Filebeat for forwarding logs.
```
ansible-playbook --private-key ~/.ssh/use-prod-dcos-agent.pem ansible-filebeat/test/integration/default/default.yml
``` 
Role Variables
--------------

 - `filebeat_config` - YAML representation of your filebeat config. This is templated directly into the configuration file as YAML. See the [example configuration](https://github.com/elastic/filebeat/blob/master/etc/filebeat.yml) for an exhaustive list of configuration options. Defaults to:

  ``` yaml
  filebeat_config:
    filebeat:
      prospectors:
        - paths:
            - /var/log/messages
            - /var/log/*.log
          input_type: log
    output:
      file:
        path: /tmp/filebeat
        filename: filebeat
    logging:
      to_syslog: true
      level: error
  ```

You can also store the config in separate `filebeat.yml` file and include it using [lookup](http://docs.ansible.com/ansible/playbooks_lookups.html#intro-to-lookups-getting-file-contents):

```
filebeat_config: "{{ lookup('file', './filebeat.yml')|from_yaml }}"
```

Common Configurations
---------------------

Connecting to Elasticsearch:

  ``` yaml
  filebeat_config:
    filebeat:
      prospectors:
        - paths:
            - /var/log/messages
            - /var/log/*.log
          input_type: log
    output:
      elasticsearch:
        hosts:
          - "http://localhost:9200"
        username: "bob"
        password: "12345"
    logging:
      to_syslog: true
      level: error
  ```

License
-------

BSD

Author Information
------------------

David Wittman
