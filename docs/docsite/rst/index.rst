.. _IBM.ansible-for-i-sap.docsite.index:

Ansible Content for IBM Power Systems - IBM i with SAP Software
===============================================================

Ansible is a radically simple IT automation system. It handles configuration management, application deployment, cloud provisioning, ad-hoc task execution, network automation, and multi-node orchestration. The **Ansible Content for IBM Power Systems - IBM i with SAP Software** provides roles to automate administrator tasks for SAP installations on IBM i, such installing the SAP Host Agent, starting and stopping SAP instances, upgrading the SAP kernel, etc.

IBM Power Systems is a family of enterprise servers that helps transform your organization by delivering industry leading resilience, scalability and accelerated performance for the most sensitive, mission critical workloads and next-generation AI and edge solutions. The Power platform also leverages open source technologies that enable you to run these workloads in a hybrid cloud environment with consistent tools, processes and skills.

Ansible Content for IBM Power Systems - IBM i with SAP Software, as part of the broader offering of **Ansible Content for IBM Power Systems**, is available from Ansible Galaxy and has community support.

Features
--------

The following roles for administrator tasks with SAP on IBM i are provided:

  - :ref:`Installing or upgrading the SAP Host Agent <IBM.ansible-for-i-sap.docsite.install_saphostagent>`

  - :ref:`Installing additional SAP application servers <IBM.ansible-for-i-sap.docsite.sap_install_app_server>`

  - :ref:`Check basic operating system settings <IBM.ansible-for-i-sap.docsite.sap_opsyscheck>`

  - :ref:`Start SAP instances or SAP start services <IBM.ansible-for-i-sap.docsite.start_sap>`

  - :ref:`Stop SAP instances or SAP start services <IBM.ansible-for-i-sap.docsite.stop_sap>`

  - :ref:`Upgrade the SAP HANA client software for an already existing SAP system <IBM.ansible-for-i-sap.docsite.upgrade_sap_hana_client>`

  - :ref:`Upgrade the kernel of an already existing SAP system <IBM.ansible-for-i-sap.docsite.upgrade_sap_kernel>`

Follow the links for a detailed description of the roles.

System requirements, installation steps and configuration information can be found in the section :ref:`Installation and configuration <IBM.ansible-for-i-sap.docsite.install_and_config>`.

License
-------

This collection is licensed under the `Apache 2.0 license <https://www.apache.org/licenses/LICENSE-2.0>`_.

Author Information
------------------

SAP on IBM Power Development Team

Copyright
---------

Copyright IBM Corporation 2021,2022

.. toctree::
   :maxdepth: 3
   :caption: Installation and configuration
   :hidden:

   install_and_config

.. toctree::
   :maxdepth: 3
   :caption: Available roles
   :hidden:

   install_saphostagent
   sap_install_app_server
   sap_opsyscheck
   start_sap
   stop_sap
   upgrade_sap_hana_client
   upgrade_sap_kernel
