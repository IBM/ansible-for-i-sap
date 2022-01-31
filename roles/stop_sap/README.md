# Role stop_sap

The role stop_sap is used to stop one or multiple SAP instances, either for a given SAP System ID (SID) or for all SAP systems that are configured in file /usr/sap/sapservices. The role stop_sap is using the sapcontrol command of the SAP Host Agent as suggested in [SAP Note 1763593](https://launchpad.support.sap.com/#/notes/1763593). The role stop_sap is using tags to control whether a single instance for a given SID, all local instances for all configured SIDs or the SAP start services for a given SID are being stopped.

For detail guides and reference, please visit the <a href="https://ibm.github.io/ansible-for-i-sap/">Documentation</a> site.

## Requirements

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see [README.md](../../README.md) in the general section about Ansible scripts for SAP on IBM i systems.

The SAP Start Services for the requested SAP system must be configured in file /usr/sap/sapservices and started. In a standard installation, they are started automatically after an IPL when subsystem QUSRWRK is stoped (autostop job entry SAPINIT).

## Dependencies

None.

## License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Author Information

SAP on IBM Power Development Team
