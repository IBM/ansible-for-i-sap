.. _IBM.ansible-for-i-sap.docsite.sap_opsyscheck:

Role sap_opsyscheck
===================

The role sap_opsyscheck is checking some basic operating system settings that are required to run SAP applications on the operating system IBM i smoothly. It can be executed prior to the first SAP installation on a new system or partition with the operating system IBM i or as a "health check" on systems or partitions where SAP is already installed. The following checks will be performed:

* Check if the user profile used to execute the Ansible role has at least the special authorities *\*ALLOBJ* and *\*SECADM*.
* Check if the SAP Host Agent is installed. If it is installed, show release level and patch information.
* Show the release level of the installed operating system.
* Check the usage (in percent) of the system auxiliary storage pool (ASP) and compare it with the threshold that can be configured in the IBM i system service tools (STRSST).

During the execution of the role sap_opsyscheck, the directory specified in ``sapopsyschk_dir_temp_managednode`` will be created if it does not exist. At the end of the execution, the directory and its contents will be deleted.

.. contents:: Table of contents
   :depth: 2

Requirements
------------

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see the prerequisites section in :ref:`Ansible Content for IBM Power Systems - IBM i with SAP Software <IBM.ansible-for-i-sap.docsite.install_and_config.prerequisites>`.

The role sap_opsyscheck must be executed as user profile QSECOFR or a user profile with similar authorities, user class *\*SECOFR* or at least special authorities *\*ALLOBJ* and *\*SECADM*. If the user profile does not have these special authorities, the results of the role are unpredictable.

Variables
---------

+----------------------------------------+------------------------------------------------------------------------------+----------+
| Variable                               | Usage                                                                        | Required |
+========================================+==============================================================================+==========+
| ``sapopsyschk_dir_temp_managednode``   | Directory path on the target host for creation of a temporary soft link      | Yes [1]_ |
+----------------------------------------+------------------------------------------------------------------------------+----------+
| ``sapopsyschk_asp_threshold``          | Threshold for ASP usage, when to issue a warning                             | Yes [2]_ |
+----------------------------------------+------------------------------------------------------------------------------+----------+

Remarks:
^^^^^^^^

.. [1] Default provided.
.. [2] The default is being retrieved from the ASP threshold that has been configured in the system service tools (STRSST). The default setting is 90%.

Defaults
--------

Suggested default values are provided in defaults/main.yml:

+-----------------------------------------------+-----------------------------+
| Variable                                      | Default                     |
+===============================================+=============================+
| ``sapopsyschk_dir_temp_managednode``          | ``"/tmp/ansible"``          |
+-----------------------------------------------+-----------------------------+

Dependencies
------------

None.

Example Playbook
----------------

The example playbook is based on the assumption that a configuration file and an inventory file with contents similar to the :ref:`configuration documentation <IBM.ansible-for-i-sap.docsite.install_and_config.configuration>` exist in the current directory. The necessary archive must have been downloaded from the SAP Software Distribution Center and stored in directory /tmp/downloads/saphostagent. The example playbook in the current directory is named sap_opsyscheck.yml and has the following contents:

.. code:: YAML

    - hosts: ibmi_servers
      roles:
      - role: <ansible_dir>/roles/sap_opsyscheck

To execute this playbook, enter the command:

.. code:: YAML
  
   ansible-playbook sap_opsyscheck.yml

License
-------

This collection is licensed under the `Apache 2.0 license <https://www.apache.org/licenses/LICENSE-2.0>`_.

Author Information
------------------

SAP on IBM Power Development Team

Copyright
---------

Copyright IBM Corporation 2022
