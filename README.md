<!-- DOCSIBLE START -->

# 📃 Role overview

## tmueller.pvenetconf



Description: Configure Network for Proxmox VE-Cluster


| Field                | Value           |
|--------------------- |-----------------|
| Readme update        | 16/07/2024 |

### Defaults

**These are static variables with lower priority**

#### File: main.yml



| Var          | Type         | Value       |Required    | Title       |
|--------------|--------------|-------------|-------------|-------------|
| [pve_force_reboot](defaults/main.yml#L2)   | bool   | `False`  |  n/a  |  n/a |





### Tasks


#### File: main.yml

| Name | Module | Has Conditions |
| ---- | ------ | --------- |
| Check system OS | ansible.builtin.fail | True |
| Install bridge-utils | ansible.builtin.apt | False |
| Configure networking | ansible.builtin.include_tasks | True |

#### File: config-network.yml

| Name | Module | Has Conditions |
| ---- | ------ | --------- |
| Configure network interfaces | ansible.builtin.template | False |
| Configure network interface names | ansible.builtin.template | True |
| Force host reboot | ansible.builtin.debug | True |


## Task Flow Graphs



### Graph for main.yml

```mermaid
flowchart TD
Start
classDef block stroke:#3498db,stroke-width:2px;
classDef task stroke:#4b76bb,stroke-width:2px;
classDef include stroke:#2ecc71,stroke-width:2px;
classDef import stroke:#f39c12,stroke-width:2px;
classDef rescue stroke:#665352,stroke-width:2px;
classDef importPlaybook stroke:#9b59b6,stroke-width:2px;
classDef importTasks stroke:#34495e,stroke-width:2px;
classDef includeTasks stroke:#16a085,stroke-width:2px;
classDef importRole stroke:#699ba7,stroke-width:2px;
classDef includeRole stroke:#2980b9,stroke-width:2px;
classDef includeVars stroke:#8e44ad,stroke-width:2px;

  Start-->|Task| Check_system_OS0_when_ansible_distribution_____Debian_[check system os]:::task
  Check_system_OS0_when_ansible_distribution_____Debian_---|When: ansible distribution     debian | Check_system_OS0_when_ansible_distribution_____Debian_
  Check_system_OS0_when_ansible_distribution_____Debian_-->|Task| Install_bridge_utils1[install bridge utils]:::task
  Install_bridge_utils1-->|Include task| config_network_yml2[\configure networking<br>include_task: config_network_yml\]:::includeTasks
  config_network_yml2---|When: pve netif alias is defined and pve netif bridges<br>is defined| config_network_yml2
  config_network_yml2-->End
```


### Graph for config-network.yml

```mermaid
flowchart TD
Start
classDef block stroke:#3498db,stroke-width:2px;
classDef task stroke:#4b76bb,stroke-width:2px;
classDef include stroke:#2ecc71,stroke-width:2px;
classDef import stroke:#f39c12,stroke-width:2px;
classDef rescue stroke:#665352,stroke-width:2px;
classDef importPlaybook stroke:#9b59b6,stroke-width:2px;
classDef importTasks stroke:#34495e,stroke-width:2px;
classDef includeTasks stroke:#16a085,stroke-width:2px;
classDef importRole stroke:#699ba7,stroke-width:2px;
classDef includeRole stroke:#2980b9,stroke-width:2px;
classDef includeVars stroke:#8e44ad,stroke-width:2px;

  Start-->|Task| Configure_network_interfaces0[configure network interfaces]:::task
  Configure_network_interfaces0-->|Task| Configure_network_interface_names1_when_pve_netif_alias_is_defined[configure network interface names]:::task
  Configure_network_interface_names1_when_pve_netif_alias_is_defined---|When: pve netif alias is defined| Configure_network_interface_names1_when_pve_netif_alias_is_defined
  Configure_network_interface_names1_when_pve_netif_alias_is_defined-->|Task| Force_host_reboot2_when_pve_force_reboot[force host reboot]:::task
  Force_host_reboot2_when_pve_force_reboot---|When: pve force reboot| Force_host_reboot2_when_pve_force_reboot
  Force_host_reboot2_when_pve_force_reboot-->End
```


## Playbook

```yml
# code language=ansible

- name: Update Known hosts
  ansible.builtin.import_playbook: playbooks/update_known_hosts.yaml
- name: Add sudo and sudoers
  ansible.builtin.import_playbook: playbooks/sudoers.yaml

- name: Configure NTP and Timezone
  hosts: all
  become: true
  become_method: ansible.builtin.sudo
  roles:
    - geerlingguy.ntp

- name: Setup Base OS with always wanted packages
  ansible.builtin.import_playbook: playbooks/setup_base.yaml

- name: Deploy/Configure Cluster (or single Node depend)
  ansible.builtin.import_playbook: playbooks/cluster.yaml

```
## Playbook graph
```mermaid
flowchart TD
  all-->|Role| geerlingguy_ntp[geerlingguy ntp]
```

## Author Information
Tobias Müller

#### License

MIT

#### Minimum Ansible Version

2.14

#### Platforms

- **Debian**: ['bookworm', 'bullseye']
#### Thanks to
- https://github.com/exaluc/docsible

<!-- DOCSIBLE END -->
