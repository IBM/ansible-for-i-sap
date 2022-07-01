# Ansible Content for IBM Power Systems - IBM i with SAP Software

The <b>Ansible Content for IBM Power Systems - IBM i with SAP Software</b> provides roles to automate administrator tasks for SAP installations on IBM i, such installing the SAP Host Agent, starting and stopping SAP instances, upgrading the SAP kernel, etc.

IBM Power Systems is a family of enterprise servers that helps transform your organization by delivering industry leading resilience, scalability and accelerated performance for the most sensitive, mission critical workloads and next-generation AI and edge solutions. The Power platform also leverages open source technologies that enable you to run these workloads in a hybrid cloud environment with consistent tools, processes and skills.

Ansible Content for IBM Power Systems - IBM i with SAP Software, as part of the broader offering of <b>Ansible Content for IBM Power Systems</b>, is available from Ansible Galaxy and has community support.

For detail guides and reference, please visit the <a href="https://ibm.github.io/ansible-for-i-sap/">Documentation</a> site.

## Prerequisites

To execute Ansible playbooks for SAP on IBM i, the following is required:

1. Your endpoints must run IBM i release 7.2 or higher.

2. Open Source Package Management through IBM i Access Client Solutions must be enabled as described at https://www.ibm.com/support/pages/node/706903.

3. You must install Python 3 on your endpoints from http://ibm.biz/ibmi-rpms.

4. You must run Ansible version 2.9 or later on your controlling node (Ansible automation engine).

It is recommended that you use a dedicated user profile on IBM i for your Ansible playbooks. There may be different authority requirements for the individual roles. To avoid authority problems, it is recommended that you create the user profile with user class *\*SECOFR* or with sufficient special authorities, at least *\*SECADM*, *\*ALLOBJ* and *\*JOBCTL*.

## Usage

The following roles for administrator tasks with SAP on IBM i are provided:

- Installing or upgrading the SAP Host Agent
- Installing additional application servers on IBM i for IBM Db2 for i or SAP HANA databases
- Check basic operating system settings
- Start SAP instances or SAP start services
- Stop SAP instances or SAP start services
- Upgrade the SAP kernel

# License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

# Author Information

SAP on IBM Power Development Team

# Copyright

Copyright IBM Corporation 2021,2022
