.. _IBM.ansible-for-i-sap.docsite.install_saphostagent:

Role install_saphostagent
=========================

The SAP Host Agent is an agent that can accomplish several life-cycle management tasks, such as operating system monitoring, database monitoring, system instance control and provisioning. For the database IBM Db2 for i, it also contains the server component of the remote database driver DBSLDirectDrive. It must be installed on any logical partition that is hosting one or multiple SAP NetWeaver instances, and it can also be installed on logical partitions that do not contain any other SAP coding.

SAP is providing bug fixes and enhancements for the SAP Host Agent several times per year. It is advised to update the SAP Host Agent with the current version from time to time as preventive action. SAP is providing the SAP Host Agent in the SAP Software Distribution Center (`SWDC <https://support.sap.com/swdc>`_) as an SAP Archive (SAR) file. The role install_saphostagent can be used to install or upgrade the SAP Host Agent from a downloaded SAR file according to the SAP documentation in `SAP Note 1031096 <https://launchpad.support.sap.com/#/notes/1031096>`_.

When the SAP Host Agent is installed for the first time in a logical partition that is running with IBM i, it will create a group profile named R3GROUP. To avoid authority problems in a distributed landscape, it is recommended that the group profile R3GROUP has the same group ID on all logical partitions in that landscape. Because of that, the landscape-wide numeric group ID of group profile R3GROUP must be specified in variable ``insshagnt_r3group_gid`` before running the playbook on a freshly installed logical partition.

.. contents:: Table of contents
   :depth: 2

Requirements
------------

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see the prerequisites section in :ref:`Ansible Content for IBM Power Systems - IBM i with SAP Software <IBM.ansible-for-i-sap.docsite.install_and_config.prerequisites>`.

The role install_saphostagent must be executed as user profile QSECOFR or a user profile with similar authorities, user class *\*SECOFR* or at least special authorities *\*ALLOBJ*, *\*SECADM* and *\*JOBCTL*.

Tags
----

The following tag can be specified optionally. Omitting the tag or specifying tag ``-t all`` has the same effect.

+--------------------------+------------------------------------------------------------------+
| Tag                      | Usage                                                            |
+==========================+==================================================================+
| ``install_saphostagent`` | Install the SAP Host Agent.                                      |
+--------------------------+------------------------------------------------------------------+

Variables
---------

+-----------------------------------------------+-------------------------------------------------------------------------------------------------------+----------+
| Variable                                      | Usage                                                                                                 | Required |
+===============================================+=======================================================================================================+==========+
| ``insshagnt_dir_download_managednode``        | Directory path on the target host where SAR file is located                                           | Yes [1]_ |
+-----------------------------------------------+-------------------------------------------------------------------------------------------------------+----------+
| ``insshagnt_input_file_saphostagent_sar``     | Name of the SAR file in the download directory on the target host that contains the SAP Host Agent    | No [2]_  |
+-----------------------------------------------+-------------------------------------------------------------------------------------------------------+----------+
| ``insshagnt_dir_sapcar_default_managednode``  | Fallback directory on the target host where SAPCAR is searched                                        | No [1]_  |
+-----------------------------------------------+-------------------------------------------------------------------------------------------------------+----------+
| ``insshagnt_dir_sapcar_managednode``          | Directory path on the target host where SAPCAR is located                                             | No [3]_  |
+-----------------------------------------------+-------------------------------------------------------------------------------------------------------+----------+
| ``insshagnt_file_sapcar``                     | Local SAPCAR file name                                                                                | Yes [1]_ |
+-----------------------------------------------+-------------------------------------------------------------------------------------------------------+----------+
| ``insshagnt_dir_temp_managednode``            | Temporary directory path that will be created on the target host                                      | Yes [1]_ |
+-----------------------------------------------+-------------------------------------------------------------------------------------------------------+----------+
| ``insshagnt_bool_clean_dir_temp_managednode`` | Boolean variable to indicate if the temporary directory will be removed or not after the installation | Yes [1]_ |
+-----------------------------------------------+-------------------------------------------------------------------------------------------------------+----------+
| ``insshagnt_r3group_gid``                     | Group ID to specify when creating user profile R3GROUP                                                | Yes [4]_ |
+-----------------------------------------------+-------------------------------------------------------------------------------------------------------+----------+

Remarks:
^^^^^^^^

.. [1] Default provided.
.. [2] Defaults to the alphanumerically last filename in ``insshagnt_dir_download_managednode`` that is matching the pattern "\*.SAR" or "\*.sar".
.. [3] When ``insshagnt_dir_sapcar_managednode`` is undefined, SAPCAR will be searched in ``insshagnt_dir_download_managednode`` and ``insshagnt_dir_sapcar_default_managednode``.
.. [4] Only when user profile R3GROUP does not yet exist.

Defaults
--------

Suggested default values are provided in defaults/main.yml:

+-----------------------------------------------+-----------------------------+
| Variable                                      | Default                     |
+===============================================+=============================+
| ``insshagnt_dir_download_managednode``        | ``"/tmp/downloads"``        |
+-----------------------------------------------+-----------------------------+
| ``insshagnt_dir_sapcar_default_managednode``  | ``"/usr/sap/hostctrl/exe"`` |
+-----------------------------------------------+-----------------------------+
| ``insshagnt_file_sapcar``                     | ``"SAPCAR"``                |
+-----------------------------------------------+-----------------------------+
| ``insshagnt_dir_temp_managednode``            | ``"/tmp/hostctrl"``         |
+-----------------------------------------------+-----------------------------+
| ``insshagnt_bool_clean_dir_temp_managednode`` | ``True``                    |
+-----------------------------------------------+-----------------------------+
| ``insshagnt_install_cmd_args``                | For internal use only       |
+-----------------------------------------------+-----------------------------+

Dependencies
------------

None.

Example Playbook
----------------

The example playbook is used to upgrade the SAP Host Agent to a newer patch level on several hosts. It is based on the assumption that a configuration file and an inventory file with contents similar to the :ref:`configuration documentation <IBM.ansible-for-i-sap.docsite.install_and_config.configuration>` exist in the current directory. The necessary SAP Host Agent archive must have been downloaded from the SAP Software Distribution Center and stored in directory /tmp/downloads/saphostagent on each of the hosts. The SAPCAR executable is available from a previous version of the SAP Host Agent in directory /usr/sap/hostctrl/exe or has been downloaded from the SAP Software Distribution Center into the download directory. The example playbook in the current directory is named install_saphostagent.yml and has the following contents:

.. code:: yaml

    - hosts: ibmi_servers
      vars:
      - insshagnt_dir_download_managednode: "/tmp/downloads/saphostagent"
      roles:
      - role: <ansible_dir>/roles/install_saphostagent

To execute this playbook, enter the command:

.. code:: yaml

  ansible-playbook --verbose install_saphostagent.yml

License
-------

This collection is licensed under the `Apache 2.0 license <https://www.apache.org/licenses/LICENSE-2.0>`_.

Author Information
------------------

SAP on IBM Power Development Team

Copyright
---------

Copyright IBM Corporation 2021,2022
