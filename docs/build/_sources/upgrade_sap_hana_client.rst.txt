.. _IBM.ansible-for-i-sap.docsite.upgrade_sap_hana_client:

Role upgrade_sap_hana_client
============================

The SAP HANA client is needed to access a remote SAP HANA database from an SAP Application Server ABAP running on IBM i. It can be downloaded from the Software Distribution Center in the SAP Support Portal (`SWDC <https://support.sap.com/swdc>`_) as an SAP Archive (SAR) file. The installation of the SAP HANA client on IBM i is explained in `SAP Note 1705999 <https://launchpad.support.sap.com/#/notes/1705999>`_. For preventive maintenance, it is advised to upgrade the SAP HANA client from time to time with the latest version. The role upgrade_sap_hana_client is used to install, upgrade or uninstall an SAP HANA client from a downloaded SAR file. Both SAP HANA client versions are supported, version 1 and version 2.

To extract the downloaded SAR file, you must provide the SAP utility SAPCAR and provide it's location in variable ``upgsaphdbclt_dir_sapcar_managednode``. By default, SAPCAR from the SAP Host Agent executable directory ``/usr/sap/hostctrl/exe`` is being used.

If you upgrade or uninstall an already installed SAP HANA client, the currently installed version is first backed up into a compressed file that follows the naming pattern ``__save_HDB_Client.<Timestamp-Year-Month-Day-Hour-Minute-Second>.tar.Z``. By default, the backup file is saved in directory ``/tmp/Ansible/HANA/hdbclient_backup``. Older backups in this directory are not cleaned up automatically, so you may want to check and clean up the directory manually from time to time.

If you install or upgrade an SAP HANA client, the downloaded SAR file is extracted into a temporary directory, by default ``/tmp/Ansible/HANA/hana_client_extract``. After a successful installation of the SAP HANA client, this directory will be deleted. After some consistency checks, the installation of the SAP HANA client will be performed by executing the program ``hdbinst`` with the parameter ``-path`` and the path name specified in variable ``upgsaphdbclt_dir_hdb_client_managednode``. The default target path name is ``/usr/sap/<sid>/hdbclient``, but you may want to specify another path name if your system is configured to use ``sapcpe``, as described in `SAP Note 1705999 <https://launchpad.support.sap.com/#/notes/1705999>`_. 

If you uninstall an SAP HANA client, the role will try to use the program ``hdbuninst`` from the SAP HANA client directory (specified in variable ``upgsaphdbclt_dir_hdb_client_managednode``). If the program ``hdbuninst`` is not found, it will be extracted from the downloaded SAP archive specified in variable ``upgsaphdbclt_file_hana_client_sar_name`` and located in the directory defined by variable ``upgsaphdbclt_dir_hana_client_download_managednode``.

An upgrade or uninstall operation will fail if an SAP instance is active that is using the currently installed SAP HANA client. In this case, you need to stop the active SAP application server instances and repeat the upgrade or the uninstall.

.. contents:: Table of contents
   :depth: 2

Requirements
------------

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see the prerequisites section in :ref:`Ansible Content for IBM Power Systems - IBM i with SAP Software <IBM.ansible-for-i-sap.docsite.install_and_config.prerequisites>`.

The role upgrade_sap_hana_client must be executed as user profile QSECOFR or a user profile with similar authorities, user class *\*SECOFR* or at least special authorities *\*ALLOBJ*, *\*SECADM* and *\*JOBCTL*.

When the SAP HANA client is upgraded or uninstalled, all SAP instances that are using the currently installed SAP HANA client must be stopped.

When installing a new SAP HANA client, the directory specified in variable ``upgsaphdbclt_dir_hdb_client_managednode`` (default: ``/usr/sap/<sid>/hdbclient``) must not exist and will be created. When upgrading or uninstalling an SAP HANA client, the directory specified in variable ``upgsaphdbclt_dir_hdb_client_managednode`` must exist, and its owner must be the user profile specified in variable ``upgsaphdbclt_usr_hdb_client_dir_owner`` (default: ``<sid>adm``).

Ensure that the IBM i host has enough free diskspace for creating and holding the backup files as well as the temporary files. By default, these files and directories will be created in directory ``/tmp/Ansible/HANA``.

Tags
----

The following tags can be specified. Omitting the tag or specifying tag ``-t all`` has the same effect as the ``-t upgrade_sap_hana_client``.

+-------------------------------+------------------------------------------------------------------------------------+
| Tag                           | Usage                                                                              |
+===============================+====================================================================================+
| ``upgrade_sap_hana_client``   | An already installed SAP HANA client will be updated with the version provided in  |
|                               | the SAP archive specified by variable ``upgsaphdbclt_file_hana_client_sar_name``.  |
+-------------------------------+------------------------------------------------------------------------------------+
| ``install_sap_hana_client``   | A new SAP HANA client will be installed from the SAP archive specified by          |
|                               | variable ``upgsaphdbclt_file_hana_client_sar_name``.                               |
+-------------------------------+------------------------------------------------------------------------------------+
| ``uninstall_sap_hana_client`` | An already installed SAP HANA client will be uninstalled.                          |
+-------------------------------+------------------------------------------------------------------------------------+

Variables
---------

+------------------------------------------------------------+---------------------------------------------------------------------+----------+
| Variable                                                   | Usage                                                               | Required |
+============================================================+=====================================================================+==========+
| ``upgsaphdbclt_input_sap_sid``                             | SAP SID of the SAP HANA System                                      | Yes      |
+------------------------------------------------------------+---------------------------------------------------------------------+----------+
| ``upgsaphdbclt_usr_hdb_client_dir_owner``                  | Owner of the SAP HANA Client directory                              | Yes [1]_ |
+------------------------------------------------------------+---------------------------------------------------------------------+----------+
| ``upgsaphdbclt_dir_hdb_client_managednode``                | Directory on the target host where the SAP HANA client resides.     | Yes [1]_ |
|                                                            | The directory must not exist when installing the SAP HANA client    |          |
|                                                            | for the first time. It must exist and be owned by the user profile  |          |
|                                                            | specified in ``upgsaphdbclt_usr_hdb_client_dir_owner`` when an      |          |
|                                                            | already installed SAP HANA client is upgraded or uninstalled.       |          |
+------------------------------------------------------------+---------------------------------------------------------------------+----------+
| ``upgsaphdbclt_dir_hdb_client_backup_managednode``         | Backup directory to backup the HANA client directory                | Yes [1]_ |
+------------------------------------------------------------+---------------------------------------------------------------------+----------+
| ``upgsaphdbclt_dir_hana_client_download_managednode``      | Directory on the target node where the SAP archive (SAR) file with  | Yes [2]_ |
|                                                            | the SAP HANA client executables is provided.                        |          |
+------------------------------------------------------------+---------------------------------------------------------------------+----------+
| ``upgsaphdbclt_file_hana_client_sar_name``                 | Name of the SAP HANA client SAP archive (SAR) file.                 | Yes [2]_ |
+------------------------------------------------------------+---------------------------------------------------------------------+----------+
| ``upgsaphdbclt_dir_temp_hana_client_extract_managednode``  | Temporary directory, that will be created on the target host and    | Yes [1]_ |
|                                                            | used for extracting the HANA client SAP archive (SAR) file.         |          |
|                                                            |                                                                     |          |
+------------------------------------------------------------+---------------------------------------------------------------------+----------+
| ``upgsaphdbclt_dir_sapcar_managednode``                    | Directory on the target host where SAPCAR utility is provided       | Yes [1]_ |
+------------------------------------------------------------+---------------------------------------------------------------------+----------+
| ``upgsaphdbclt_file_sapcar_name``                          | SAPCAR utility file name                                            | Yes [1]_ |
+------------------------------------------------------------+---------------------------------------------------------------------+----------+

Remarks:
^^^^^^^^

.. [1] Default provided.
.. [2] Default provided. Not necessarily required with tag ``uninstall_sap_hana_client`` if the SAP HANA client directory is complete and operational.

Defaults
--------

Suggested default values are provided in defaults/main.yml:

+------------------------------------------------------------+------------------------------------------------+
| Variable                                                   |                    Default                     |
+===================================================+========+================================================+
| ``upgsaphdbclt_usr_hdb_client_dir_owner``                  | ``"<sid>adm"``                                 |
+------------------------------------------------------------+------------------------------------------------+
| ``upgsaphdbclt_dir_hdb_client_managednode``                | ``"/usr/sap/<SID>/hdbclient"``                 |
+------------------------------------------------------------+------------------------------------------------+
| ``upgsaphdbclt_dir_hdb_client_backup_managednode``         | ``"/tmp/Ansible/HANA/hdbclient_backup"``       |
+------------------------------------------------------------+------------------------------------------------+
| ``upgsaphdbclt_dir_hana_client_download_managednode``      | ``"/tmp/Ansible/HANA/hana_client_download"``   |
+------------------------------------------------------------+------------------------------------------------+
| ``upgsaphdbclt_file_hana_client_sar_name``                 | ``"HANA_CLIENT.SAR"``                          |
+------------------------------------------------------------+------------------------------------------------+
| ``upgsaphdbclt_dir_temp_hana_client_extract_managednode``  | ``"/tmp/Ansible/HANA/hana_client_extract"``    |
+------------------------------------------------------------+------------------------------------------------+
| ``upgsaphdbclt_dir_sapcar_managednode``                    | ``"/usr/sap/hostctrl/exe"``                    |
+------------------------------------------------------------+------------------------------------------------+
| ``upgsaphdbclt_file_sapcar_name``                          | ``"SAPCAR"``                                   |
+------------------------------------------------------------+------------------------------------------------+

Dependencies
------------

None.

Example Playbooks
-----------------

Install or Upgrade an SAP HANA client
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The example playbook is used to install or upgrade the SAP HANA client software for an SAP system named HDA on several hosts. It is based on the assumption that a configuration file and an inventory file with contents similar to the :ref:`configuration documentation <IBM.ansible-for-i-sap.docsite.install_and_config.configuration>` exist in the current directory. The necessary SAP archive file IMDB_CLIENT20_004_202-80002091.SAR must have been downloaded from the SAP Software Distribution Center and stored in directory /tmp/Ansible/HANA/hana_client_download. The playbook is located in the current directory, named upgrade_hdb_client.yml and has the following contents:

.. code:: yaml

    - name: Install or Upgrade SAP HANA Client
      hosts: ibm_i_servers
       vars:
       - upgsaphdbclt_input_sap_sid: "HDA"
       - upgsaphdbclt_usr_hdb_client_dir_owner: "hdaadm"
       - upgsaphdbclt_dir_hdb_client_managednode: "/usr/sap/HDA/hdbclient"
       - upgsaphdbclt_dir_hdb_client_backup_managednode: "/tmp/ANSIBLE/HANA/hdb_client_backup"
       - upgsaphdbclt_dir_hana_client_download_managednode: "/tmp/Ansible/HANA/hana_client_download"
       - upgsaphdbclt_file_hana_client_sar_name: "IMDB_CLIENT20_004_202-80002091.SAR"
       - upgsaphdbclt_dir_temp_hana_client_extract_managednode: "/tmp/Ansible/HANA/hana_client_extract"
       - upgsaphdbclt_dir_sapcar_managednode: "/usr/sap/hostctrl/exe"
       - upgsaphdbclt_file_sapcar_name: "SAPCAR"
      roles:
       - role: <ansible_dir>/roles/upgrade_sap_hana_client

or shorter, by relying on default values as much as possible:

.. code:: yaml

    - name: Install or Upgrade SAP HANA Client
      hosts: ibm_i_servers
       vars:
       - upgsaphdbclt_input_sap_sid: "HDA"
       - upgsaphdbclt_file_hana_client_sar_name: "IMDB_CLIENT20_004_202-80002091.SAR"
      roles:
       - role: <ansible_dir>/roles/upgrade_sap_hana_client

To install new SAP HANA clients on systems where no SAP HANA clients existed yet, execute the playbook with this command:

.. code:: yaml

   ansible-playbook --verbose -t install_sap_hana_client upgrade_hdb_client.yml

To upgrade already existing SAP HANA clients, execute the playbook with this command:

.. code:: yaml

   ansible-playbook --verbose -t upgrade_sap_hana_client upgrade_hdb_client.yml

Uninstall an SAP HANA client
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The example playbook is used to uninstall the SAP HANA client software from directory /usr/sap/HDA/hdbclient on several hosts. It is based on the assumption that a configuration file and an inventory file with contents similar to the :ref:`configuration documentation <IBM.ansible-for-i-sap.docsite.install_and_config.configuration>` exist in the current directory. The playbook is located in the current directory, named upgrade_hdb_client.yml and has the following contents:

.. code:: yaml

    - name: Uninstall SAP HANA Client
      hosts: ibm_i_servers
       vars:
       - upgsaphdbclt_input_sap_sid: "HDA"
      roles:
       - role: <ansible_dir>/roles/upgrade_sap_hana_client

To execute this playbook, enter the command:

.. code:: yaml

   ansible-playbook --verbose -t uninstall_sap_hana_client upgrade_hdb_client.yml

License
-------

This collection is licensed under the `Apache 2.0 license <https://www.apache.org/licenses/LICENSE-2.0>`_.

Author Information
------------------

SAP on IBM Power Development Team

Copyright
---------

Copyright IBM Corporation 2021,2022
