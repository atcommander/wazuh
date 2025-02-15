# Wazuh Collection


## Indexer Deployment Playbook


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

<example.com> ansible_host=<11.11.11.16> ansible_user=root

```

### <span style="color:#5dbaebff">Vars File</span>

**Path:** /path/to/playbook/repo/inventory/group_vars/wazuh_indexer/vars.yml

```yaml

instances: # A certificate will be generated for every node using the name as CN.

  node1:

    name: node-1

    ip: <node-1 IP>

    role: indexer

  node2:

    name: node-2

    ip: <node-2 IP>

    role: indexer

  node3:

    name: node-3

    ip: <node-3 IP>

    role: indexer

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
