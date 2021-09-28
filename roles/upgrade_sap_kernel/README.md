# Role upgrade_sap_kernel

The role upgrade_sap_kernel is used to upgrade the kernel of an already installed SAP system with new code that has been downloaded from the SAP Sowftware Distribution Center ([SWDC](https://support.sap.com/swdc)) in SAP Archive (SAR) files. The SAR files must be available in the download directory that is specified in variable `kernel_download_dir`. In a distributed landscape, it is recommended to share the download directory across all servers or logical partitions of that landscape. The role uses the tool APYSIDKRN, which is documented in [SAP Note 1632755](https://launchpad.support.sap.com/#/notes/1632755). It will apply the contents of _all_ SAR files that are provided in the download directory.

You can either replace one or a few components of an SAP kernel, for example applying a new dw patch, or you can replace the complete kernel, for example with a new stack kernel or even a new release. Variable `kernel_upgrade_mode` defines how to proceed: A value of "ADD" specifies that only the provided components are being replaces, a value of "FULLY" specifies, that the existing kernel is deleted and completely replaced with the contents of the downloaded SAR files. Option "FULLY" is required when upgrading the kernel release or flavor (e.g. from 7.22 to 7.22 EXT) but bears the risk that some kernel components may be missing if not all required SAR files were provided.

If instance-specific directories are used for executables (see [SAP Note 1632754](https://launchpad.support.sap.com/#/notes/1632754)), the new kernel will only be activated when stopping and starting the instances.

The role upgrade_sap_kernel needs the SAP System ID as input in variable `sap_sid`.

Log files from the tool APYSIDKRN will be stored in directory /sapmnt/`sid`/patches/log. If you execute the role upgrade_sap_kernel frequently, you may want to cleanup this directory from time to time to save space on disk.

## Requirements

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see [README.md](../../README.md) in the general section about Ansible scripts for SAP on IBM i systems.

The SAP system must be configured to use the "New User Concept" as described in [SAP Note 1123501](https://launchpad.support.sap.com/#/notes/1123501).

The role upgrade_sap_kernel will install the contents of all SAR files in the download directory specified in variable `kernel_download_dir`. It is the responsibility of the user to ensure that the download directory contains all necessary patches for the required upgrade mode ("ADD" or "FULLY").

## Variables

| **Variable** | **Usage** | **Required** |
|:-------------|:----------|:-------------|
|`sap_sid`|SAP system ID|Yes|
|`kernel_upgrade_dir`|Directory for temporary storing a script that is needed to execute the APYSIDKRN command|Yes [(1)](#Remarks)|
|`kernel_download_dir`|Download directory that contains the SAR files with the new kernel components to be installed. To avoid runtime errors, the length of the path name should not exceed 220 characters.|Yes [(1)](#Remarks)|
|`release_switch`|Kernel upgrade to a newer release: `True` or `False`|Yes [(1)](#Remarks)|
|`kernel_upgrade_mode`|Upgrade mode: "ADD" or "FULLY"|No [(2)](#Remarks)|
|`sap_hostagent_sapcar_file_name`|File name of the SAPCAR tool|Yes [(1)](#Remarks)|
|`sap_sapcar_local_path`|Path name to the SAPCAR tool|Yes [(1)](#Remarks)|

#### Remarks:
1. Default provided
2. When `release_switch`=`False`, the default will be "ADD". When `release_switch`=`True`, the value will be ignored.

## Defaults

Suggested default values are provided in defaults/main.yml:

| **Variable** | **Default** |
|:-------------|:------------|
|`kernel_upgrade_dir`|"/tmp/kernel"|
|`kernel_download_dir`|"/tmp/downloads/kernel"|
|`release_switch`|`False`|
|`sap_hostagent_sapcar_file_name`|"SAPCAR"|
|`sap_sapcar_local_path_default`|"/usr/sap/hostctrl/exe"|

## Dependencies

None.

## Example Playbook

The example playbook is based on the assumption that a configuration file and an inventory file with contents similar to the [Configuration documentation](../../../ansible#configuration) exist in the current directory. It replaces the existing kernel of SAP system PRD with a new stack kernel in the same release. The necessary archives must have been downloaded from the SAP Software Distribution Center and stored in directory /tmp/downloads/kernel. The playbook is located in the current directory, named replace_kernel_in_same_release.yml and has the following contents:

```YAML
    - hosts: ibmi_servers
      vars:
      - sid: "PRD"
      - kernel_upgrade_mode: "FULLY"
      roles:
      - role: <ansible_dir>/roles/upgrade_sap_kernel
```

To execute this playbook, enter the command:

```YAML
ansible-playbook --verbose replace_kernel_in_same_release.yml
```

## License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Author Information

SAP on IBM Power Development Team
