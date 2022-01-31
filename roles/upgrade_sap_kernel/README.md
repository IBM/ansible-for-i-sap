# Role upgrade_sap_kernel

The role upgrade_sap_kernel is used to upgrade the kernel of an already installed SAP system with new code that has been downloaded from the SAP Sowftware Distribution Center ([SWDC](https://support.sap.com/swdc)) in SAP Archive (SAR) files. The SAR files must be available in the download directory that is specified in variable `upgsapkrn_dir_download_managednode`. In a distributed landscape, it is recommended to share the download directory across all servers or logical partitions of that landscape. The role uses the tool APYSIDKRN, which is documented in [SAP Note 1632755](https://launchpad.support.sap.com/#/notes/1632755). It will apply the contents of _all_ SAR files that are provided in the download directory.

You can either replace one or a few components of an SAP kernel, for example applying a new dw patch, or you can replace the complete kernel, for example with a new stack kernel or even a new release. By specifying a tag, you define if you are replacing the currently installed kernel with code of the same release and build level or with code at a different release or build level. If you are replacing the kernel code in the same release and build level, you can specify in a variable, if you want to replace only a selected executable or the complete kernel. It is your responsibility to ensure that the download directory contains all necessary patches for the requested operation.

If instance-specific directories are used for executables (see [SAP Note 1632754](https://launchpad.support.sap.com/#/notes/1632754)), the new kernel will only be activated when stopping and starting the instances.

Log files from the tool APYSIDKRN will be stored in directory /sapmnt/`sid`/patches/log. If you execute the role upgrade_sap_kernel frequently, you may want to cleanup this directory from time to time to save space on disk.

For detail guides and reference, please visit the <a href="https://ibm.github.io/ansible-for-i-sap/">Documentation</a> site.

## Requirements

This role is intended for the operating system IBM i. The target system must be enabled to execute Ansible playbooks. For details, see [README.md](../../README.md) in the general section about Ansible scripts for SAP on IBM i systems.

The SAP system must be configured to use the "New User Concept" as described in [SAP Note 1123501](https://launchpad.support.sap.com/#/notes/1123501).

## Dependencies

None.

## License

This collection is licensed under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Author Information

SAP on IBM Power Development Team
