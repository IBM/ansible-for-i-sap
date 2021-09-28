# Ansible Content for IBM Power Systems - IBM i with SAP Software

The <b>Ansible Content for IBM Power Systems - IBM i with SAP Software</b> provides roles to automate administrator tasks for SAP installations on IBM i, such installing the SAP Host Agent, starting and stopping SAP instances, upgrading the SAP kernel, etc.

IBM Power Systems is a family of enterprise servers that helps transform your organization by delivering industry leading resilience, scalability and accelerated performance for the most sensitive, mission critical workloads and next-generation AI and edge solutions. The Power platform also leverages open source technologies that enable you to run these workloads in a hybrid cloud environment with consistent tools, processes and skills.

Ansible Content for IBM Power Systems - IBM i with SAP Software, as part of the broader offering of <b>Ansible Content for IBM Power Systems</b>, is available from Ansible Galaxy and has community support.

## Prerequisites

To execute Ansible playbooks for SAP on IBM i, the following is required:

1. Your endpoints must run IBM i release 7.2 or higher.

2. Open Source Package Management through IBM i Access Client Solutions must be enabled as described at https://www.ibm.com/support/pages/node/706903.

3. You must install Python 3 on your endpoints from http://ibm.biz/ibmi-rpms.

4. You must run Ansible version 2.9 or later on your controlling node (Ansible automation engine).

It is recommended that you use a dedicated user profile on IBM i for your Ansible playbooks. There may be different authority requirements for the individual roles. To avoid authority problems, it is recommended that you create the user profile with user class *\*SECOFR* or with sufficient special authorities:

   ```yaml
   CRTUSRPRF USRPRF(ANSIBLE) PASSWORD(<...>) USRCLS(*SECOFR) TEXT('Ansible functional user')
   SPCAUT(*USRCLS) OWNER(*USRPRF) LANGID(ENU) CNTRYID(US) CCSID(500) LOCALE(*NONE)
   ```
or:

   ```yaml
   CRTUSRPRF USRPRF(ANSIBLE) PASSWORD(<...>) USRCLS(*PGMR) TEXT('Ansible functional user')
   SPCAUT(*ALLOBJ *SECADM *JOBCTL *SPLCTL *IOSYSCFG *SAVSYS) OWNER(*USRPRF) LANGID(ENU)
   CNTRYID(US) CCSID(500) LOCALE(*NONE)
   ```

## Installation

On every IBM i host you want to be an end point for ansible (target host, remote host), you have to perform the following 2 steps:

1. Install the new Open Source Solutions package.
   Because 5733-OPS no longer is available for IBM i V7R2 and higher (see: https://www.ibm.com/support/pages/node/668177), install the new Open Source Solutions package according to https://ibmi-oss-docs.readthedocs.io/en/latest/yum/README.html

   Download bootstrap.sh (https://public.dhe.ibm.com/software/ibmi/products/pase/rpms/bootstrap.sh) and bootstrap.tar.Z (https://public.dhe.ibm.com/software/ibmi/products/pase/rpms/bootstrap.tar.Z), put it on the IBM i machines, then execute:
   ```bash
   cd /tmp
   touch /tmp/bootstrap.log; /QOpenSys/usr/bin/ksh /tmp/bootstrap.sh > /tmp/bootstrap.log 2>&1
   ```
   After successful installation, adjust your search path $PATH like
   ```bash
   PATH="/QOpenSys/pkgs/bin:${PATH}"
   export PATH
   ```

2. Install Python 3 on the IBM i hosts you want to use as remotes.
   ```bash
   PATH=/QOpenSys/pkgs/bin:/QOpenSys/usr/bin:/usr/ccs/bin:/usr/sbin:.:/usr/bin:/usr/local/bin:$PATH
   export PATH
   
   LIBPATH=.:$LIBPATH
   export LIBPATH
   
   yum install python3-pip python3-ibm_db python3-itoolkit
   ```


For every IBM i host you want to use as controlling node(s), additionally perform the following 2 steps:

3. Install Ansible (required only on the controlling node(s))
   ```bash
   yum install ansible
   ```

4. Install the Ansible roles that you want to use (required only on the controlling node(s))
   ```bash
   ansible-galaxy install <path to ansible role>
   ```

## Configuration

1. On the controlling node create an Ansible configuration file:

   ```bash
   cat ~/.ansible.cfg
   
   #remote_tmp     = ~/.ansible/tmp
   #local_tmp      = ~/.ansible/tmp
   inventory       = ~/.ansible/hosts
   library         = ~/.ansible/modules
   roles_path      = ~/.ansible/roles
   ssh_args        = -i /home/johndoe/ssh/id_rsa
   transfer_method = scp
   ```
 
2. On the controlling node create an inventory file:
   ```bash
   cat ~/.ansible/hosts
   
   [ibmi_servers]
   ibmiserver01.mycorp.com
   ibmiserver02.mycorp.com
   ibmiserver03.mycorp.com
   
   [ibmi_servers:vars]
   ansible_python_interpreter=/QOpenSys/pkgs/bin/python3
   ```

## Usage

The following roles for administrator tasks with SAP on IBM i are provided:

### [Installing or upgrading the SAP Host Agent](roles/install_saphostagent)

### [Start SAP instances](roles/start_sap)

### [Stop SAP instances](roles/stop_sap)

### [Upgrade the kernel of an already existing SAP system](roles/upgrade_sap_kernel)

Follow the links for a detailed description of the roles.

# License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

# Author Information

SAP on IBM Power Development Team

# Copyright

© Copyright IBM Corporation 2021
