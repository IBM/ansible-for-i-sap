# Role stop_sap

The role stop_sap is used to stop one or multiple SAP instances for a given SAP System ID (SID). If a requested SAP instance is already inactive, the request will be ignored and no error message will be sent. The role stop_sap is using the sapcontrol command of the SAP Host Agent as suggested in [SAP Note 1763593](https://launchpad.support.sap.com/#/notes/1763593).

The role stop_sap needs the SAP System ID as input in variable `sap_sid`. An SAP instance number can be provided optionally in variable `sap_nr`. If an instance number is provided, only this instance is stoped. If no instance number is provided, stop_sap will stop all configured instances for the given SID, also on remote servers in a distributed landscape. It is recommended that the role stop_sap is executed on the logical partition or server that hosts the central services instance (ASCS, SCS), the primary application server instance, or the central instance.

## Requirements

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see [README.md](../../README.md) in the general section about Ansible scripts for SAP on IBM i systems.

The SAP Start Services for the requested SAP system must be configured in file /usr/sap/sapservices and started. In a standard installation, they are started automatically after an IPL when subsystem QUSRWRK is stoped (autostop job entry SAPINIT). They can be started manually by calling program R3SAP400/SAPINIT.

## Variables

| **Variable** | **Usage** | **Required** |
|:-------------|:----------|:-------------|
|`sapctr_exe_dir`|Directory path on the target host where the sapcontrol executable is located|Yes [(1)](#Remarks)|
|`sap_sid`|SAP system ID|Yes|
|`sap_nr`|SAP instance number|No [(2)](#Remarks)|
|`sap_softtimeout`|softtimeout in seconds: -1 = infinite wait, 0 = hard shutdown|No [(3)](#Remarks)|

#### Remarks:
1. Default provided
2. When the parameter is omitted, all instances are stopped.
3. When the parameter is omitted, a hard shutdown is executed.

## Defaults

Suggested default values are provided in defaults/main.yml:

| **Variable** | **Default** |
|:-------------|:------------|
|`sapctr_exe_dir` | "/usr/sap/hostctrl/exe" |
|`instance_order` | FOR INTERNAL USE ONLY |

## Dependencies

None.

## Example Playbook

The example playbook is based on the assumption that a configuration file and an inventory file with contents similar to the [Configuration documentation](../../../ansible#configuration) exist in the current directory. The example playbook in the current directory is named stop_sap.yml and has the following contents:

```YAML
    - hosts: ibmi_servers
      vars:
      - sap_sid: "PRD"
      roles:
      - role: <ansible_dir>/roles/stop_sap
```

To execute this playbook, enter the command:

```YAML
ansible-playbook --verbose stop_sap.yml
```

## License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Author Information

SAP on IBM Power Development Team
