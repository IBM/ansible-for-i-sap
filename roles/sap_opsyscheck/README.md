# Role sap_opsyscheck

The role sap_opsyscheck is checking some basic operating system settings that are required to run SAP applications on the operating system IBM i smoothly. It can be executed prior to the first SAP installation on a new system or partition with the operating system IBM i or as a "health check" on systems or partitions where SAP is already installed. The following checks will be performed:

- Check if the SAP Host Agent is installed. If it is installed, show release level and patch information.
- Show the release level of the installed operating system.
- Check if the user profile used to execute the Ansible role has at least the special authorities *\*ALLOBJ* and *\*SECADM*.

For detail guides and reference, please visit the <a href="https://ibm.github.io/ansible-for-i-sap/">Documentation</a> site.

## Requirements

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see [README.md](../../README.md) in the general section about Ansible scripts for SAP on IBM i systems.

The role sap_opsyscheck must be executed as user profile QSECOFR or a user profile with similar authorities, user class *\*SECOFR* or at least special authorities *\*ALLOBJ* and *\*SECADM*. If the user profile does not have these special authorities, the results of the role are unpredictable.

## Dependencies

None.

## License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Author Information

SAP on IBM Power Development Team