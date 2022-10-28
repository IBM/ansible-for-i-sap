.. _IBM.ansible-for-i-sap.docsite.sap_install_app_server:

Role sap_install_app_server
===========================

After the initial install of an SAP system, you can add or remove additional application servers for that SAP system to distribute the workload better when the usage of the SAP system increases or decreases. The SAP Software Provisioning Manager (SWPM) is offering options to install or uninstall SAP application servers for an existing SAP system. For more information about the SWPM, see the SAP documentation at the link `SAP Software Logistics - Software Provisioning <https://support.sap.com/en/tools/software-logistics-tools.html#section_622087154>`_.

Typically, the SWPM is used as an interactive tool which takes the input from the SAP administrator to perform the selected installation activity. Besides that, the SWPM can be executed in unattended mode and process a parameter input file that has been prepared prior to the actual execution. For more information about the unattended mode for the SWPM, see `SAP Note 2230669 <https://launchpad.support.sap.com/#/notes/2230669>`_ and the `SAP Installation Documentation <https://help.sap.com/docs/SOFTWARE_PROVISIONING_MANAGER/30839dda13b2485889466316ce5b39e9/c8ed609927fa4e45988200b153ac63d1.html>`_, section "System Provisioning Using a Parameter Input File".

The role sap_install_app_server is installing or uninstalling additional application servers on IBM i using the SWPM in unattended mode. To execute the SWPM in unattended mode, the role is using a parameter input file called "inifile.params". The role sap_install_app_server can create its own input file based on Ansible input variables during its execution or use an already existing input file that has been created by the SWPM previously.

Currently, installations for additional SAP ABAP application servers based on SAP NetWeaver 7.4 and higher are supported with a database of type IBM Db2 for i or SAP HANA. The role sap_install_app_server only supports SWPM 1.0, SWPM 2.0 is not supported with this role.

If the SWPM is executed in unattended mode by Ansible and an error occurs, you will have to check the SWPM installation directory for the cause, like in a manual installation. After correcting the error, you can restart the Ansible playbook again, and the SWPM will continue the current installation in unattended mode at the point of the failure.

.. contents:: Table of contents
   :depth: 2

Requirements
------------

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see the prerequisites section in :ref:`Ansible Content for IBM Power Systems - IBM i with SAP Software <IBM.ansible-for-i-sap.docsite.install_and_config.prerequisites>`.

The role sap_install_app_server must be executed as user profile QSECOFR or a user profile with similar authorities, user class *\*SECOFR* or at least special authorities *\*ALLOBJ*, *\*SECADM* and *\*JOBCTL*. Additionally, the user must have the following parameters defined: *OWNER(\*USRPRF) LANGID(ENU) CNTRYID(US) CCSID(500) LOCALE(\*NONE)*.

To create a new user profile use the following command::

    CRTUSRPRF USRPRF(<user profile name>) PASSWORD(<password>) USRCLS(*SECOFR) TEXT('SAP installation user used by Ansible') SPCAUT(*USRCLS) OWNER(*USRPRF) LANGID(ENU) CNTRYID(US) CCSID(500) LOCALE(*NONE)

To adjust an already existing user profile with sufficient authorities, use the following command::

    CHGUSRPRF USRPRF(<user profile name>) OWNER(*USRPRF) LANGID(ENU) CNTRYID(US) CCSID(500) LOCALE(*NONE)

The host must have enough free space on disk to store and execute an SAP application server. The amount of storage needed depends on many factors, such as number of active SAP users, the number of SAP work processes and SAP buffer sizes.

A current version of the SAP Host Agent must be installed.

The database, ASCS instance and primary application server instance (or the central instance) of the SAP system must be installed. At least the ASCS instance of the SAP system must be active.

The global directories of the SAP system (``/sapmnt/<sid>/exe``, ``/sapmnt/<sid>/global``, ``/sapmnt/<sid>/j2ee`` and ``/sapmnt/<sid>/profile``) must be accessible  by the installation user from the managed node.

Tags
----

Specify one of the following tags to specify if you want to install or uninstall an additional application server. If you do not specify a tag, an error message will be sent.

+------------------------------+-------------------------------------------------------------------+
| Tag                          | Usage                                                             |
+==============================+===================================================================+
| ``sap_install_app_server``   | Install an additional SAP application server.                     |
+------------------------------+-------------------------------------------------------------------+
| ``sap_uninstall_app_server`` | Remove an existing SAP application server and its components.     |
+------------------------------+-------------------------------------------------------------------+

Variables
---------

+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| Variable                                              | Usage                                                                                            | Required           |
+=======================================================+==================================================================================================+====================+
| ``insappsvr_input_install_mode``                      | Reuse or create input parameter file for the unattended mode:                                    | Yes [1]_           |
|                                                       | ``reuseInifile`` = An already existing file inifile.params will be used.                         |                    |
|                                                       | ``createInifile`` = A new file inifile.params will be created base on the other input variables. |                    |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_dir_swpm_managednode``                    | Directory where the SWPM software is located.                                                    | Yes [2]_           |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_logfile_path``                            | Temporary directory path where installation and uninstall logs and data are saved.               | No [1]_            |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_sap_sid``                           | The SAP SID of the SAP system used for the installation of the additional application server.    | Yes [7]_           |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_file_inifile``                      | Directory where the prepared files inifile.params and instkey.pkey are located.                  | Yes [7]_ [3]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_initfile_NW_AS_instanceNumber``     | The SAP instance number of the SAP instance to be installed.                                     | Yes [7]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_nwUsers_sapsysGID``                 | Group ID (GID) of the SAP user profile SAPSYS.                                                   | Yes [7]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_nwUsers_sapsysGID``                 | User ID (UID) of the SAP user profile <SAPSID>ADM.                                               | Yes [7]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_NW_GetMasterPassword``              | Encrypted master password used by the SWPM.                                                      | Yes [7]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_SAPInstDes25Hash``                  | Encryption string of the file inifile.params used by the SWPM.                                   | Yes [7]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_instkey``                           | Encryption string of the instkey.pkey used by the SWPM.                                          | Yes [7]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_SAPDBType``                         | The SAP database type (``DB4`` for IBM Db2 for i or ``HDB`` for SAP HANA).                       | Yes [7]_ [5]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_NW_release``                        | The SWPM Product ID needed for the SAP release of the SAP system.                                | Yes [7]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_dir_hdbclient_managednode``         | Directory where the HANA database client media is located (only used with SAP HANA).             | Yes [7]_ [6]_ [4]_ |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_dir_downloadbasket_managednode``    | Directory where the IGSHELPER.SAR archive is located.                                            | Yes [7]_ [6]_ [4]_ |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_HDB_Schema_schemaName``             | The SAP HANA database schema (only used with SAP HANA).                                          | Yes [7]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_HDB_Schema_schemaPassword``         | Encrypted password of SAP HANA database schema (only used with SAP HANA).                        | Yes [7]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``insappsvr_input_HDB_getDBInfo_instanceNumber``      | The HDB instance number (only used with SAP HANA).                                               | Yes [7]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``uninsappsvr_input_sap_sid``                         | The SAP SID of the SAP system used for the uninstall of an application server.                   | Yes [8]_           |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``uninsappsvr_input_file_inifile``                    | Directory where the files inifile.params and instkey.pkey for the uninstall are located.         | Yes [8]_ [3]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+
| ``uninsappsvr_input_initfile_NW_AS_instanceNumber``   | The SAP instance number of the SAP instance to be uninstalled.                                   | Yes [8]_ [4]_      |
+-------------------------------------------------------+--------------------------------------------------------------------------------------------------+--------------------+

Remarks:
^^^^^^^^

.. [1] Default provided.
.. [2] Required software and the location of the software has to be provided.
.. [3] Only needed in mode reuseInifile.
.. [4] Only needed in mode createInifile. For encrypted data like passwords, the data has to be copied directly from the SWPM generated files inifile.params and instkey.pkey!
.. [5] Only needed in mode createInifile. Default provided.
.. [6] Use only SAP media and SAP archives which are compatible with the target SAP system (the most current patch level of the version which was initially used to setup the SAP system).
.. [7] Only needed for the installation of an additional application server.
.. [8] Only needed for the uninstall of an existing application server.

Defaults
--------

Suggested default values are provided in defaults/main.yml:

+-----------------------------------------------+-----------------------------+
| Variable                                      | Default                     |
+===============================================+=============================+
| ``insappsvr_input_install_mode``              | ``"reuseInifile"``          |
+-----------------------------------------------+-----------------------------+
| ``insappsvr_logfile_path``                    | ``"/tmp/Ansible/SWPM"``     |
+-----------------------------------------------+-----------------------------+
| ``insappsvr_input_SAPDBType``                 | ``"DB4"``                   |
+-----------------------------------------------+-----------------------------+

The file defaults/main.yml contains more entries, but the values for the other variables are set to empty strings. These entries are required to ensure complete contents in inifile.params when variable ``insappsvr_input_install_mode`` is set to ``createInifile``. It is in the responsibility of the playbook to set meaningful values as required for the selected operation.

Dependencies
------------

None.

Example Playbooks
-----------------

SAP Installation
^^^^^^^^^^^^^^^^

**Example Playbook for the installation of the SAP additional application server reusing an existing inifile.params and the related instkey.pkey**

The example playbook is used to install an additional application server for an already installed SAP system with the SAP system ID (SID) PRD on a host named ibmiserver02.mycorp.com. The underlying database is IBM Db2 for i. It is based on the assumption that a configuration file and an inventory file with contents similar to the :ref:`configuration documentation <IBM.ansible-for-i-sap.docsite.install_and_config.configuration>` exist in the current directory. During the installation, a previously prepared file name inifile.params will be used. For more information how to create an inifile.params file, see `SAP Note 2230669 <https://launchpad.support.sap.com/#/notes/2230669>`_. Make sure that the software locations defined in the file inifile.params are available. The example playbook in the current directory is named sap_install_app_server_reuseInifile.yml and has the following contents:

.. code:: yaml

    - name: Install SAP Additional Application Server
      hosts: ibmiserver02.mycorp.com
      vars:
       - insappsvr_input_install_mode: "reuseInifile"
       - insappsvr_input_sap_sid: "PRD"
       - insappsvr_dir_swpm_managednode: "/tmp/ANSIBLE/SWPM10"
       - insappsvr_input_file_inifile: "/tmp/ANSIBLE/inifiles/PRD/inst/07/inifile.params"
      roles:
       - role: <ansible_dir>/roles/sap_install_app_server

Note: Due to a glitch in the SWPM, the location of the SAP HANA DB client media will not be automatically saved in inifile.params after is has been specified during the installation dialog. Add the following line manually to inifile.params before using it with this Ansible role::

    SAPINST.CD.PACKAGE.RDBMS-HDB-CLIENT=<SAP_HANA_DB_Client_Media_Directory>

This is also explained in the SAP installation documentation.

To execute this playbook, enter the command:

.. code:: yaml

   ansible-playbook --verbose sap_install_app_server_reuseInifile.yml -t sap_install_app_server

**Example Playbook for the installation of the SAP additional application server creating its own inifile.params and the related instkey.pkey**

The example playbook is used to install an additional application server for an already installed SAP system with the SAP system ID (SID) PRD on a host named ibmiserver02.mycorp.com. The underlying database is SAP HANA. It is based on the assumption that a configuration file and an inventory file with contents similar to the :ref:`configuration documentation <IBM.ansible-for-i-sap.docsite.install_and_config.configuration>` exist in the current directory. During the installation, the installation parameter file inifile.params will be created based on the specified parameters. For more information how to create an inifile.params file, see `SAP Note 2230669 <https://launchpad.support.sap.com/#/notes/2230669>`_. Make sure that the software locations defined in the file inifile.params are available, for example the SAP HANA database client. The example playbook in the current directory is named sap_install_app_server_createInifile.yml and has the following contents:

.. code:: yaml

    - name: Install SAP Additional Application Server
      hosts: ibmiserver02.mycorp.com
      vars:
       - insappsvr_input_install_mode: "createInifile"
       - insappsvr_input_sap_sid: "PRD"
       - insappsvr_dir_swpm_managednode: "/tmp/ANSIBLE/SWPM10"
       - insappsvr_input_initfile_NW_AS_instanceNumber: "07"
       - insappsvr_input_nwUsers_sapsysGID: "204"
       - insappsvr_input_nwUsers_sidAdmUID: "205"
       - insappsvr_input_NW_GetMasterPassword: "des25(71cIuqdFOxGZRkPNI3r5iAxx)"
       - insappsvr_input_SAPInstDes25Hash: "SAPInstDes25Hash=$eY3ELBT5gQ2Z$C+eS02APqADAELB7RK2SuI2rZCajRanfIv/JgPeqeAesO7SPAT9Bj1Ycxf6tV/QHkrMqW1i2QHLqPLTwy8f6xicu2fsLNQjX"
       - insappsvr_input_instkey: "5HhD4qsHDP6S+eJXsVu3xeU1dh4nu78x"
       - insappsvr_input_SAPDBType: "HDB"
       - insappsvr_input_NW_release: "NW_DI:S4HANA1709.CORE.HDB.PD"
       - insappsvr_input_dir_hdbclient_managednode: "/tmp/ANSIBLE/SAPHANAclient/SAP_HANA_CLIENT"
       - insappsvr_input_dir_downloadbasket: "/tmp/ANSIBLE/downloadBasket"
       - insappsvr_input_HDB_Schema_schemaName: "SAPHDBABAP"
       - insappsvr_input_HDB_Schema_schemaPassword: "des25(iD9vfeDFE1otL9JQbPeF6Qxx)"
       - insappsvr_input_HDB_getDBInfo_instanceNumber: "00"
      roles:
       - role: <ansible_dir>/roles/sap_install_app_server

To execute this playbook, enter the command:

.. code:: yaml

   ansible-playbook --verbose sap_install_app_server_createInifile.yml -t sap_install_app_server


For some selected entries for the playbook Yaml file you must use the following mapping table to copy the needed values from the file inifile.params and the file instkey.pkey:

+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| Variable in the playbook                          | Parameter in the file inifile.params        | Remarks                                 |
+===================================================+=============================================+=========================================+
| ``insappsvr_input_sap_sid``                       | ``NW_readProfileDir.profileDir``            | Get the <SID> from the profile dir name |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| ``insappsvr_input_initfile_NW_AS_instanceNumber`` | ``NW_AS.instanceNumber``                    |                                         |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| ``insappsvr_input_nwUsers_sapsysGID``             | ``nwUsers.sapsysGID``                       |                                         |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| ``insappsvr_input_nwUsers_sidAdmUID``             | ``nwUsers.sidAdmUID``                       |                                         |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| ``insappsvr_input_NW_GetMasterPassword``          | ``NW_GetMasterPassword.masterPwd``          |                                         |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| ``insappsvr_input_SAPInstDes25Hash``              |                                             | Grep the Des25 hash key after the       |
|                                                   |                                             | string ``# IMPORTANT DO NOT DELETE!!!`` |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| ``insappsvr_input_NW_release``                    |                                             | Get the SAP product id in the third     |
|                                                   |                                             | line after the string ``product id``    |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| ``insappsvr_input_dir_downloadbasket_managednode``| ``archives.downloadBasket``                 | Directory where the IGSHELPER.SAR is    |
|                                                   |                                             | located                                 |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| ``insappsvr_input_HDB_Schema_schemaName``         | ``HDB_Schema_Check_Dialogs.schemaName``     | Only SAP HANA                           |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| ``insappsvr_input_HDB_Schema_schemaPassword``     | ``HDB_Schema_Check_Dialogs.schemaPassword`` | Only SAP HANA                           |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+
| ``insappsvr_input_HDB_getDBInfo_instanceNumber``  | ``NW_HDB_getDBInfo.instanceNumber``         | Only SAP HANA                           |
+---------------------------------------------------+---------------------------------------------+-----------------------------------------+

+-----------------------------+--------------------------+-----------------------------------------------------------------+
| Variable in the playbook    | Data in the instkey.pkey | Remarks                                                         |
+=============================+==========================+=================================================================+
| ``insappsvr_input_instkey`` |                          | Grep the complete encrypted first line in the file instkey.pkey |
+-----------------------------+--------------------------+-----------------------------------------------------------------+


SAP Uninstall
^^^^^^^^^^^^^

**Example Playbook for the uninstall of the SAP additional application server reusing an existing inifile.params**

The example playbook is used to uninstall an application server with instance number 07 from an SAP system with the SAP system ID (SID) PRD on a host named ibmiserver02.mycorp.com. It is based on the assumption that a configuration file and an inventory file with contents similar to the :ref:`configuration documentation <IBM.ansible-for-i-sap.docsite.install_and_config.configuration>` exist in the current directory. During the installation, a previously prepared file name inifile.params will be used. For more information how to create an inifile.params file, see `SAP Note 2230669 <https://launchpad.support.sap.com/#/notes/2230669>`_. Make sure that the software locations defined in the file inifile.params are available. The example playbook in the current directory is named sap_uninstall_app_server_reuseInifile.yml and has the following contents:

.. code:: yaml

    - name: Uninstall SAP Additional Application Server
      hosts: ibmiserver02.mycorp.com
      vars:
       - insappsvr_input_install_mode: "reuseInifile"
       - insappsvr_dir_swpm_managednode: "/tmp/ANSIBLE/SWPM10"
       - uninsappsvr_input_file_inifile: "/tmp/ANSIBLE/inifiles/PRD/uninst/07/inifile.params"
      roles:
       - role: <ansible_dir>/roles/sap_install_app_server

Note: No encrypted data and no instkey.pkey are needed for an SAP uninstall reusing an existing inifile.params file. This playbook works with both IBM Db2 for i and SAP HANA.

To execute this playbook, enter the command:

.. code:: yaml

   ansible-playbook --verbose sap_uninstall_app_server_reuseInifile.yml -t sap_uninstall_app_server

**Example Playbook for the uninstall of the SAP additional application server creating its own inifile.params**

The example playbook is used to uninstall an application server with instance number 07 from an SAP system with the SAP system ID (SID) PRD on a host named ibmiserver02.mycorp.com. It is based on the assumption that a configuration file and an inventory file with contents similar to the :ref:`configuration documentation <IBM.ansible-for-i-sap.docsite.install_and_config.configuration>` exist in the current directory. During the installation, the installation parameter file inifile.params will be created based on the specified parameters. For more information how to create an inifile.params file, see `SAP Note 2230669 <https://launchpad.support.sap.com/#/notes/2230669>`_. Make sure that the software locations defined in the file inifile.params are available. The example playbook in the current directory is named sap_uninstall_app_server_createInifile.yml and has the following contents:

.. code:: yaml

    - name: Uninstall SAP Additional Application Server
      hosts: ibmiserver02.mycorp.com
      vars:
       - insappsvr_input_install_mode: "createInifile"
       - insappsvr_dir_swpm_managednode: "/tmp/ANSIBLE/SWPM10"
       - uninsappsvr_input_sap_sid: "PRD"
       - uninsappsvr_input_initfile_NW_AS_instanceNumber: "07"
      roles:
       - role: <ansible_dir>/roles/sap_install_app_server

Note: Mainly the SAP SID and the SAP instance number are needed for the uninstall, no encrypted passwords or other details. This playbook works with both IBM Db2 for i and SAP HANA.

To execute this playbook, enter the command:

.. code:: yaml

   ansible-playbook --verbose sap_uninstall_app_server_createInifile.yml -t sap_uninstall_app_server

Additional Information
----------------------

License
-------

This collection is licensed under the `Apache 2.0 license <https://www.apache.org/licenses/LICENSE-2.0>`_.

Author Information
------------------

SAP on IBM Power Development Team

Copyright
---------

Copyright IBM Corporation 2021,2022

