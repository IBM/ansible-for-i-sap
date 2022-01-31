# Upgrading to Ansible Content for IBM Power Systems - IBM i with SAP Software Version 1.1.0

# Breaking changes

With version 1.1.0, new conventions for variable names were introduced to increase clarity about their usage. These are the guidelines:

- Each variable name has a prefix that indicates the role for which it is used, such as insshagnt_ for the role install_saphostagent.
- The second component of a variable name indicates whether the variable contains user input, a directory name, a file name or a boolean (true/false) value.
- If the variable name is referring to a directory, the suffix in the variable name indicates whether the directory is located on the Ansible controlling node or managed node.

## Role install_saphostagent

The following variables have been renamed:

| **Old name**                        | **New name**                                |
|:------------------------------------|:--------------------------------------------|
| `sap_hostagent_download_local_path` | `insshagnt_dir_download_managednode`        |
| `sap_hostagent_sar_file_name`       | `insshagnt_input_file_saphostagent_sar`     |
| `sap_sapcar_local_path_default`     | `insshagnt_dir_sapcar_default_managednode`  |
| `sap_sapcar_local_path`             | `insshagnt_dir_sapcar_managednode`          |
| `sap_hostagent_sapcar_file_name`    | `insshagnt_file_sapcar`                     |
| `sap_hostagent_agent_tmp_directory` | `insshagnt_dir_temp_managednode`            |
| `sap_hostagent_clean_tmp_directory` | `insshagnt_bool_clean_dir_temp_managednode` |
| `sap_gid`                           | `insshagnt_r3group_gid`                     |

## Role start_sap

The following variables have been renamed:

| **Old name**     | **New name**                       |
|:-----------------|:-----------------------------------|
| `sap_sid`        | `startsap_input_sap_sid`           |
| `sap_nr`         | `startsap_input_sap_instance_nr`   |
| `sapctr_exe_dir` | `startsap_dir_sapctrl_managednode` |

## Role stop_sap

The following variables have been renamed:

| **Old name**      | **New name**                      |
|:------------------|:----------------------------------|
| `sap_sid`         | `stopsap_input_sap_sid`           |
| `sap_nr`          | `stopsap_input_sap_instance_nr`   |
| `sapctr_exe_dir`  | `stopsap_dir_sapctrl_managednode` |
| `sap_softtimeout` | `stopsap_softtimeout`             |

## Role upgrade_sap_kernel

The task `sap_kernel_upgrade_same_rel.yml` has been split up into the two tasks `sap_kernel_upgrade_same_rel_add.yml` and `sap_kernel_upgrade_same_rel_fully.yml`. When executing the role ``upgrade_sap_kernel``, a tag rather than a variable is used to indicate whether the kernel is upgraded to a higher release or build level.

The following variables have been renamed:

| **Old name**                             | **New name**                                      |
|:-----------------------------------------|:--------------------------------------------------|
| `sap_sid`                                | `upgsapkrn_input_sap_sid`                         |
| `kernel_upgrade_dir`                     | `upgsapkrn_dir_temp_script_managednode`           |
| `kernel_download_dir`                    | `upgsapkrn_dir_download_managednode`              |
| `kernel_upgrade_work_dir`                | `upgsapkrn_dir_temp_iletools_extract_managednode` |
| `release_switch`                         | (obsolete)                                        |
| `kernel_upgrade_mode`                    | `upgsapkrn_input_upgrade_mode`                    |
| `sap_hostagent_sapcar_file_name`         | `upgsapkrn_file_sapcar`                           |
| `sap_sapcar_local_path`                  | `upgsapkrn_dir_sapcar_managednode`                |
| `sap_sapcar_local_path_default`          | `upgsapkrn_dir_sapcar_default_managednode`        |
