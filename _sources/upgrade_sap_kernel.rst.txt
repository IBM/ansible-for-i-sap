.. _IBM.ansible-for-i-sap.docsite.upgrade_sap_kernel:

Role upgrade_sap_kernel
=======================

The role upgrade_sap_kernel is used to upgrade the kernel of an already installed SAP system with new code that has been downloaded from the SAP Software Distribution Center (`SWDC <https://support.sap.com/swdc>`_) in SAP Archive (SAR) files. The SAR files must be available in the download directory that is specified in variable ``upgsapkrn_dir_download_managednode``. In a distributed landscape, it is recommended to share the download directory across all servers or logical partitions of that landscape. The role uses the tool APYSIDKRN, which is documented in `SAP Note 1632755 <https://launchpad.support.sap.com/#/notes/1632755>`_. It will apply the contents of *all* SAR files that are provided in the download directory. It is your responsibility to ensure that the downloaded SAR files are compatible with your SAP system and database.

You can either replace one or a few components of an SAP kernel, for example applying a new dw patch, or you can replace the complete kernel, for example with a new stack kernel or even a new release. If you are replacing the currently installed kernel with code of the same release and build level, variable ``upgsapkrn_input_upgrade_mode`` defines how to proceed: A value of ``"ADD"`` specifies that only the provided components are being replaces, a value of ``"FULLY"`` specifies, that the existing kernel is deleted and completely replaced with the contents of the downloaded SAR files.

If instance-specific directories are used for executables (see `SAP Note 1632754 <https://launchpad.support.sap.com/#/notes/1632754>`_), the new kernel will only be activated when stopping and starting the instances.

The role upgrade_sap_kernel needs the SAP System ID as input in variable ``upgsapkrn_input_sap_sid``.

By specifying a tag, you define if you are replacing the currently installed kernel with code of the same release and build level (``-t sap_kernel_upgrade_same_release``) or with code at a different release or build level (``-t sap_kernel_upgrade_new_release``). SAP is marking different build levels in the same release with additions like "EXT" or "EX2". The default is ``-t sap_kernel_upgrade_same_release``.

Log files from the tool APYSIDKRN will be stored in directory /sapmnt/<sid>/patches/log. If you execute the role upgrade_sap_kernel frequently, you may want to cleanup this directory from time to time to save space on disk.

.. contents:: Table of contents
   :depth: 2

Requirements
------------

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see the prerequisites section in :ref:`Ansible Content for IBM Power Systems - IBM i with SAP Software <IBM.ansible-for-i-sap.docsite.install_and_config.prerequisites>`.

The SAP system must be configured to use the "New User Concept" as described in `SAP Note 1123501 <https://launchpad.support.sap.com/#/notes/1123501>`_.

The role upgrade_sap_kernel will install the contents of all SAR files in the download directory specified in variable ``upgsapkrn_dir_download_managednode``. It is the responsibility of the user to ensure that the download directory contains all necessary patches for the required upgrade mode (``"ADD"`` or ``"FULLY"``).

Tags
----

Specify one of the following tags to specify if the code in the downloaded SAR file is at the same release and build level as the currently installed kernel. If you do not specify a tag, ``-t sap_kernel_upgrade_same_release`` will be assumed as default. If you misspell the tag, an error message will be sent.

+-------------------------------------+-------------------------------------------------------------------------+
| Tag                                 | Usage                                                                   |
+=====================================+=========================================================================+
| ``sap_kernel_upgrade_same_release`` | The SAR file contains executables at the same release and build level   |
|                                     | as the currently installed level. This is the default.                  |
+-------------------------------------+-------------------------------------------------------------------------+
| ``sap_kernel_upgrade_new_release``  | The SAR file contains executables at a different release or build level |
|                                     | than the currently installed level.                                     |
+-------------------------------------+-------------------------------------------------------------------------+

Variables
---------

+-----------------------------------------------------+------------------------------------------------------------------------------------------------+----------+
| Variable                                            | Usage                                                                                          | Required |
+=====================================================+================================================================================================+==========+
| ``upgsapkrn_input_sap_sid``                         | SAP system ID                                                                                  | Yes      |
+-----------------------------------------------------+------------------------------------------------------------------------------------------------+----------+
| ``upgsapkrn_dir_temp_script_managednode``           | Directory for temporarily storing a script that is needed to execute the APYSIDKRN command     | Yes [1]_ |
+-----------------------------------------------------+------------------------------------------------------------------------------------------------+----------+
| ``upgsapkrn_dir_download_managednode``              | Download directory that contains the SAR files with the new kernel components to be installed. | Yes [1]_ |
|                                                     | To avoid runtime errors, the length of the path name should not exceed 220 characters.         |          |
+-----------------------------------------------------+------------------------------------------------------------------------------------------------+----------+
| ``upgsapkrn_dir_temp_iletools_extract_managednode`` | Directory for temporarily extracting the ILETOOLS archive.                                     | Yes [1]_ |
+-----------------------------------------------------+------------------------------------------------------------------------------------------------+----------+
| ``upgsapkrn_input_upgrade_mode``                    | Upgrade mode: "ADD" or "FULLY". This value is ignored when the tag                             | Yes [2]_ |
|                                                     | ``sap_kernel_upgrade_new_release`` is specified.                                               |          |
+-----------------------------------------------------+------------------------------------------------------------------------------------------------+----------+
| ``upgsapkrn_dir_sapcar_managednode``                | Path name to the SAPCAR tool                                                                   | Yes [1]_ |
+-----------------------------------------------------+------------------------------------------------------------------------------------------------+----------+
| ``upgsapkrn_file_sapcar``                           | File name of the SAPCAR tool                                                                   | Yes [1]_ |
+-----------------------------------------------------+------------------------------------------------------------------------------------------------+----------+

Remarks:
^^^^^^^^

.. [1] Default provided.
.. [2] Only required when tag ``sap_kernel_upgrade_same_release`` is selected. Default provided.

Defaults
--------

Suggested default values are provided in defaults/main.yml:

+-----------------------------------------------------+-----------------------------+
| Variable                                            | Default                     |
+=====================================================+=============================+
| ``upgsapkrn_dir_temp_script_managednode``           | ``"/tmp/kernel"``           |
+-----------------------------------------------------+-----------------------------+
| ``upgsapkrn_dir_download_managednode``              | ``"/tmp/downloads/kernel"`` |
+-----------------------------------------------------+-----------------------------+
| ``upgsapkrn_dir_temp_iletools_extract_managednode`` | ``"/tmp/apysidkrn"``        |
+-----------------------------------------------------+-----------------------------+
| ``upgsapkrn_input_upgrade_mode``                    | ``"ADD"``                   |
+-----------------------------------------------------+-----------------------------+
| ``upgsapkrn_dir_sapcar_default_managednode``        | ``"/usr/sap/hostctrl/exe"`` |
+-----------------------------------------------------+-----------------------------+
| ``upgsapkrn_file_sapcar``                           | ``"SAPCAR"``                |
+-----------------------------------------------------+-----------------------------+

Dependencies
------------

None.

Example Playbook
----------------

The example playbook is based on the assumption that a configuration file and an inventory file with contents similar to the :ref:`configuration documentation <IBM.ansible-for-i-sap.docsite.install_and_config.configuration>` exist in the current directory. It replaces the existing kernel of SAP system PRD with a new stack kernel in the same release. The necessary archives must have been downloaded from the SAP Software Distribution Center and stored in directory /tmp/downloads/kernel. The playbook is located in the current directory, named replace_kernel_in_same_release.yml and has the following contents:

.. code:: YAML

    - hosts: ibmi_servers
      vars:
      - upgsapkrn_input_sap_sid: "PRD"
      - upgsapkrn_input_upgrade_mode: "FULLY"
      roles:
      - role: <ansible_dir>/roles/upgrade_sap_kernel

To execute this playbook, enter the command:

.. code:: YAML

    ansible-playbook --verbose replace_kernel_in_same_release.yml -t sap_kernel_upgrade_same_release

License
-------

This collection is licensed under the `Apache 2.0 license <https://www.apache.org/licenses/LICENSE-2.0>`_.

Author Information
------------------

SAP on IBM Power Development Team

Copyright
---------

Copyright IBM Corporation 2021,2022
