# Wazuh Collection


## Cluster Deployment Playbook


</br>
</br>


## <span style="color:#5dbaebff">Table of content</span>

- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## <span style="color:#5dbaebff">Examples</span>

### <span style="color:#5dbaebff">Host File</span>

```ini

[wazuh_indexer]

<example.com> ansible_host=<11.11.11.11> ansible_user=root

<example.com> ansible_host=<11.11.11.12> ansible_user=root

<example.com> ansible_host=<11.11.11.13> ansible_user=root

[wazuh_manager]

<example.com> ansible_host=<11.11.11.14> ansible_user=root

[wazuh_worker]

<example.com> ansible_host=<11.11.11.15> ansible_user=root

[wazuh_dshboard]

<example.com> ansible_host=<11.11.11.16> ansible_user=root

```

### <span style="color:#5dbaebff">Vars File</span>

**Path:** /path/to/playbook/repo/inventory/group_vars/wazuh_dashboard/vars.yml

```yaml

#### Dashboard

indexer_network_host: "{{ hostvars.dashboard.private_ip }}"

indexer_node_name: node-6

indexer_node_master: false

indexer_node_ingest: false

indexer_node_data: false

dashboard_node_name: node-6

wazuh_api_credentials:

  - id: default

    url: https://{{ hostvars.manager.private_ip }}
    
    port: 55000
    
    username: custom-user
    
    password: SecretPassword1!

ansible_shell_allow_world_readable_temp: true

```

**Path:** /path/to/playbook/repo/inventory/group_vars/wazuh_indexer/vars.yml

```yaml

#### Certificate Generation

indexer_node_master: true

indexer_network_host: "{{ private_ip }}"

instances:

  node1:
  
    name: node-1 # Important: must be equal to indexer_node_name.
  
    ip: "{{ hostvars.wi1.private_ip }}" # When unzipping, the node will search for its node name folder to get the cert.
  
    role: indexer
  
  node2:
  
    name: node-2
  
    ip: "{{ hostvars.wi2.private_ip }}"
  
    role: indexer
  
  node3:
  
    name: node-3
  
    ip: "{{ hostvars.wi3.private_ip }}"
  
    role: indexer
  
  node4:
  
    name: node-4
  
    ip: "{{ hostvars.manager.private_ip }}"
  
    role: wazuh
  
    node_type: master
  
  node5:
  
    name: node-5
  
    ip: "{{ hostvars.worker.private_ip }}"
  
    role: wazuh
  
    node_type: worker
  
  node6:
  
    name: node-6
  
    ip: "{{ hostvars.dashboard.private_ip }}"
  
    role: dashboard

#### Indexer

indexer_cluster_nodes:

  - "{{ hostvars.wi1.private_ip }}"

  - "{{ hostvars.wi2.private_ip }}"

  - "{{ hostvars.wi3.private_ip }}"

indexer_discovery_nodes:

  - "{{ hostvars.wi1.private_ip }}"

  - "{{ hostvars.wi2.private_ip }}"

  - "{{ hostvars.wi3.private_ip }}"

```

**Path:** /path/to/playbook/repo/inventory/group_vars/wazuh_manager/vars.yml

```yaml

#### Manager

filebeat_node_name: node-4

wazuh_manager_config:

  connection:

    - type: "secure"

      port: "1514"
    
      protocol: "tcp"
    
      queue_size: 131072

  api:

    https: "yes"

  cluster:

    disable: "no"

    node_name: "master"

    node_type: "master"

    key: "c98b62a9b6169ac5f67dae55ae4a9088"

    nodes:

      - "{{ hostvars.manager.private_ip }}"

    hidden: "no"

wazuh_api_users:

  - username: custom-user

    password: SecretPassword1!

filebeat_output_indexer_hosts:

  - "{{ hostvars.wi1.private_ip }}"

  - "{{ hostvars.wi2.private_ip }}"

  - "{{ hostvars.wi3.private_ip }}"

```

**Path:** /path/to/playbook/repo/inventory/group_vars/wazuh_worker/vars.yml

```yaml

#### Worker

filebeat_node_name: node-5

wazuh_manager_config:

  connection:

  - type: "secure"

    port: "1514"

    protocol: "tcp"

    queue_size: 131072

  api:

    https: "yes"

  cluster:

    disable: "no"

    node_name: "worker_01"

    node_type: "worker"

    key: "c98b62a9b6169ac5f67dae55ae4a9088"

  nodes:

    - "{{ hostvars.manager.private_ip }}"

  hidden: "no"

```

### <span style="color:#5dbaebff">Setup</span>

1. Install collection

```bash

ansible-galaxy collection install https://github.com/atcommander/wazuh.git

```

2. Setup Inventory folder and Vars and Hosts files

3. Run Ansible Playbook to Prepare Server

```bash

ansible-playbook -i inventory/hosts wazuh.wazuh.aio.install --tags "setup-all"

```




## <span style="color:#5dbaebff">Dependencies</span>

None.
