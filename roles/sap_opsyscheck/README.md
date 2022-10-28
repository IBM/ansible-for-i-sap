# Role sap_opsyscheck

The role sap_opsyscheck is checking some basic operating system settings that are required to run SAP applications on the operating system IBM i smoothly. It can be executed prior to the first SAP installation on a new system or partition with the operating system IBM i or as a "health check" on systems or partitions where SAP is already installed. The following checks will be performed:

- Check if the user profile used to execute the Ansible role has at least the special authorities *\*ALLOBJ*, *\*SECADM* and *\*JOBCTL*.
- Check if the SAP Host Agent is installed. If it is installed, show release level and patch information.
- Show the release level of the installed operating system.
- Check the usage (in percent) of the system auxiliary storage pool (ASP) and compare it with the threshold that can be configured in the IBM i system service tools (STRSST).
- Check the status of the database cross reference (XREF) files on the host.
- Show the group ID (GID) of user profile R3GROUP and check if it is the same on all specified hosts.
- Check the fill level of the job tables and issue a warning if they are more filled than specified (default: 10%).

For detail guides and reference, please visit the <a href="https://ibm.github.io/ansible-for-i-sap/">Documentation</a> site.

## Requirements

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see [README.md](../../README.md) in the general section about Ansible scripts for SAP on IBM i systems.

The role sap_opsyscheck must be executed as user profile QSECOFR or a user profile with similar authorities, user class *\*SECOFR* or at least special authorities *\*ALLOBJ*, *\*SECADM* and *\*JOBCTL*. If the user profile does not have these special authorities, the results of the role are unpredictable.

The check for the job tables filling requires IBM i 7.3 with SF99703 Level 22, IBM i 7.4 with SF99704 Level 10, or a higher IBM i release. On releases prior to IBM i 7.3, the check is skipped.

## Dependencies

None.

## License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Author Information

SAP on IBM Power Development Team
