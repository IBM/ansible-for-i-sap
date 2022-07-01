# Role install_saphostagent

The SAP Host Agent is an agent that can accomplish several life-cycle management tasks, such as operating system monitoring, database monitoring, system instance control and provisioning. For the database IBM Db2 for i, it also contains the server component of the remote database driver DBSLDirectDrive. It must be installed on any logical partition that is hosting one or multiple SAP NetWeaver instances, and it can also be installed on logical partitions that do not contain any other SAP coding.

SAP is providing bug fixes and enhancements for the SAP Host Agent several times per year. It is advised, to update the SAP Host Agent with the current version from time to time as preventive action. SAP is providing the SAP Host Agent in the SAP Software Distribution Center ([SWDC](https://support.sap.com/swdc)) as an SAP Archive (SAR) file. The role install_saphostagent can be used to install or upgrade the SAP Host Agent from a downloaded SAR file according to the SAP documentation in [SAP Note 1031096](https://launchpad.support.sap.com/#/notes/1031096).

When the SAP Host Agent is installed for the first time in a logical partition that is running with IBM i, it will create a group profile named R3GROUP. To avoid authority problems in a distributed landscape, it is recommended that the group profile R3GROUP has the same group ID on all logical partitions in that landscape. Because of that, the landscape-wide numeric group ID of group profile R3GROUP must be specified when running the playbook on a freshly installed logical partition.

For detail guides and reference, please visit the <a href="https://ibm.github.io/ansible-for-i-sap/">Documentation</a> site.

## Requirements

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see [README.md](../../README.md) in the general section about Ansible scripts for SAP on IBM i systems.

The role install_saphostagent must be executed as user profile QSECOFR or a user profile with similar authorities, user class *\*SECOFR* or at least special authorities *\*ALLOBJ*, *\*SECADM* and *\*JOBCTL*.

## Dependencies

None.

## License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Author Information

SAP on IBM Power Development Team
