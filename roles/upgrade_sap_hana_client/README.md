# Role upgrade_sap_hana_client

The SAP HANA client is needed to access a remote SAP HANA database from an SAP Application Server ABAP running on IBM i. It can be downloaded from the Software Distribution Center in the SAP Support Portal ([SWDC](https://support.sap.com/swdc)) as an SAP Archive (SAR) file. The installation of the SAP HANA client on IBM i is documented in [SAP Note 1705999](https://launchpad.support.sap.com/#/notes/1705999). For preventive maintenance, it is advised to upgrade the SAP HANA client from time to time with the latest version. The role upgrade_sap_hana_client can be used to install, upgrade, or uninstall an SAP HANA client from a downloaded SAR file. Both SAP HANA client versions are supported, version 1 and version 2.

For detail guides and reference, please visit the <a href=https://ibm.github.io/ansible-for-i-sap/">Documentation</a> site.

## Requirements

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see [README.md](../../README.md) in the general section about Ansible scripts for SAP on IBM i systems.

The role upgrade_sap_hana_client must be executed as user profile QSECOFR or a user profile with similar authorities, user class *\*SECOFR* or at least special authorities *\*ALLOBJ*, *\*SECADM* and *\*JOBCTL*.

When the SAP HANA client is upgraded or uninstalled, all SAP instances that are using the currently installed SAP HANA client must be stopped.

When installing a new SAP HANA client, the directory specified in variable ``upgsaphdbclt_dir_hdb_client_managednode`` (default: ``/usr/sap/<sid>/hdbclient``) must not exist and will be created. When upgrading or uninstalling an SAP HANA client, the directory specified in variable ``upgsaphdbclt_dir_hdb_client_managednode`` must exist, and its owner must be the user profile specified in variable ``upgsaphdbclt_usr_hdb_client_dir_owner`` (default: ``<sid>adm``).

Ensure that the IBM i host has enough free diskspace for creating and holding the backup files as well as the temporary files. By default, these files and directories will be created in directory ``/tmp/Ansible/HANA``.

## Dependencies

None.

## License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Author Information

SAP on IBM Power Development Team
