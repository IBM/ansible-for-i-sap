# Role sap_install_app_server

After the initial install of an SAP system, you can add or remove additional application servers for that SAP system to distribute the workload better when the usage of the SAP system increases or decreases. The SAP Software Provisioning Manager (SWPM) is offering options to install or uninstall SAP application servers for an existing SAP system. For more information about the SWPM, see the SAP documentation at the link <a href="https://support.sap.com/en/tools/software-logistics-tools.html#section_622087154>">SAP Software Logistics - Software Provisioning</a>.

Typically, the SWPM is used as an interactive tool which takes the input from the SAP administrator to perform the selected installation activity. Besides that, the SWPM can be executed in unattended mode and process a parameter input file that has been prepared prior to the actual execution. For more information about the unattended mode for the SWPM, see <a href="https://launchpad.support.sap.com/#/notes/2230669">SAP Note 2230669</a> and the <a href=https://help.sap.com/docs/SOFTWARE_PROVISIONING_MANAGER/30839dda13b2485889466316ce5b39e9/c8ed609927fa4e45988200b153ac63d1.html>SAP Installation Documentation</a>, section "System Provisioning Using a Parameter Input File".

The role sap_install_app_server is installing or uninstalling additional application servers on IBM i using the SWPM in unattended mode. To execute the SWPM in unattended mode, the role is using a parameter input file called "inifile.params". The role sap_install_app_server can create its on input file based on Ansible input variables during execution or use an already existing input file that has been created by the SWPM previously.

Currently, application server installations based on SAP NetWeaver 7.4 and higher are supported for both IBM Db2 for i and SAP HANA as the database. The role sap_install_app_server only supports SWPM 1.0, SWPM 2.0 is currently not supported.

For detail guides and reference, please visit the <a href="https://ibm.github.io/ansible-for-i-sap/">Documentation</a> site.

## Requirements

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see [README.md](../../README.md) in the general section about Ansible scripts for SAP on IBM i systems.

The role sap_install_app_server must be executed as user profile QSECOFR or a user profile with similar authorities, user class *\*SECOFR* or at least special authorities *\*ALLOBJ*, *\*SECADM* and *\*JOBCTL*. If the user profile does not have these special authorities, the results of the role are unpredictable.

To create a new user profile use the following command: CRTUSRPRF USRPRF(``user profile name``) PASSWORD(``password``) USRCLS(*SECOFR) TEXT('SAP installation user used by Ansible') SPCAUT(*USRCLS) OWNER(*USRPRF) LANGID(ENU) CNTRYID(US) CCSID(500) LOCALE(*NONE)

To adjust an already existing user profile with sufficient authorities, use the following command: CHGUSRPRF USRPRF(``user profile name``) OWNER(*USRPRF) LANGID(ENU) CNTRYID(US) CCSID(500) LOCALE(*NONE)

The host must have enough free space on disk to store and execute an SAP application server. The amount of storage needed depends on many factors, such as number of active SAP users, the number of SAP work processes and SAP buffer sizes.

A current version of the SAP Host Agent must be installed.

The database, ASCS instance and primary application server instance (or the central instance) of the SAP system must be installed. At least the ASCS instance of the SAP system must be active.

The global directories of the SAP system (``/sapmnt/<sid>/exe``, ``/sapmnt/<sid>/global``, ``/sapmnt/<sid>/j2ee`` and ``/sapmnt/<sid>/profile``) must be accessible from the managed node.

## Dependencies

None.

## License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Author Information

SAP on IBM Power Development Team
