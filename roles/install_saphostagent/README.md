# Role install_saphostagent

The SAP Host Agent is an agent that can accomplish several life-cycle management tasks, such as operating system monitoring, database monitoring, system instance control and provisioning. For the database IBM Db2 for i, it also contains the server component of the remote database driver DBSLDirectDrive. It must be installed on any logical partition that is hosting one or multiple SAP NetWeaver instances, and it can also be installed on logical partitions that do not contain any other SAP coding.

SAP is providing bug fixes and enhancements for the SAP Host Agent several times per year. It is advised, to update the SAP Host Agent with the current version from time to time as preventive action. SAP is providing the SAP Host Agent in the SAP Software Distribution Center ([SWDC](https://support.sap.com/swdc)) as an SAP Archive (SAR) file. The role install_saphostagent can be used to install or upgrade the SAP Host Agent from a downloaded SAR file according to the SAP documentation in [SAP Note 1031096](https://launchpad.support.sap.com/#/notes/1031096).

When the SAP Host Agent is installed for the first time in a logical partition that is running with IBM i, it will create a group profile named R3GROUP. To avoid authority problems in a distributed landscape, it is recommended that the group profile R3GROUP has the same group ID on all logical partitions in that landscape. Because of that, the landscape-wide numeric group ID of group profile R3GROUP must be specified in variable `sap_gid` before running the playbook on a freshly installed logical partition.

## Requirements

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see [README.md](../../README.md) in the general section about Ansible scripts for SAP on IBM i systems.

The role install_saphostagent must be executed as user profile QSECOFR or a user profile with similar authorities, user class *\*SECOFR* or at least special authorities *\*ALLOBJ* and *\*SECADM*.

## Variables

| **Variable** | **Usage** | **Required** |
|:-------------|:----------|:-------------|
|`sap_hostagent_download_local_path`|Directory path on the target host where SAR file is located|Yes [(1)](#Remarks)|
|`sap_hostagent_sar_file_name`|Name of the SAR file in the download directory on the target host that contains the SAP Host Agent|No [(2)](#Remarks)|
|`sap_sapcar_local_path_default`|Fallback directory on the target host where SAPCAR is searched|No [(1)](#Remarks)|
|`sap_sapcar_local_path`|Directory path on the target host where SAPCAR is located|No [(3)](#Remarks)|
|`sap_hostagent_sapcar_file_name`|Local SAPCAR file name|Yes [(1)](#Remarks)|
|`sap_hostagent_agent_tmp_directory`|Temporary directory path that will be created on the target host|Yes [(1)](#Remarks)|
|`sap_hostagent_clean_tmp_directory`|Boolean variable to indicate if the temporary directory will be removed or not after the installation|Yes [(1)](#Remarks)|
|`sap_gid`|Group ID to specify when creating user profile R3GROUP|Yes [(4)](#Remarks)|

#### Remarks:
1. Default provided
2. Defaults to the alphanumerically last filename in `sap_hostagent_download_local_path` that is matching the pattern "\*.SAR" or "\*.sar"
3. When `sap_sapcar_local_path` is undefined, SAPCAR will be searched in `sap_hostagent_download_local_path` and `sap_sapcar_local_path_default`.
4. Only when user profile R3GROUP does not yet exist

## Defaults

Suggested default values are provided in defaults/main.yml:

| **Variable** | **Default** |
|:-------------|:------------|
|`sap_hostagent_download_local_path` | "/tmp/downloads" |
|`sap_sapcar_local_path_default` | "/usr/sap/hostctrl/exe" |
|`sap_hostagent_sapcar_file_name` | "SAPCAR" |
|`sap_hostagent_agent_tmp_directory` | "/tmp/hostctrl" |
|`sap_hostagent_clean_tmp_directory` | True |
|`sap_hostagent_agent_command_args` | FOR INTERNAL USE ONLY |

## Dependencies

None.

## Example Playbook

The example playbook is based on the assumption that a configuration file and an inventory file with contents similar to the [Configuration documentation](../../../ansible#configuration) exist in the current directory. The necessary archive must have been downloaded from the SAP Software Distribution Center and stored in directory /tmp/downloads/saphostagent. The example playbook in the current directory is named install_saphostagent.yml and has the following contents:

```YAML
    - hosts: ibmi_servers
      vars:
      - sap_hostagent_download_local_path: "/tmp/downloads/saphostagent"
      roles:
      - role: <ansible_dir>/roles/install_saphostagent
```

To execute this playbook, enter the command:

```YAML
ansible-playbook --verbose install_saphostagent.yml
```


## License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Author Information

SAP on IBM Power Development Team
