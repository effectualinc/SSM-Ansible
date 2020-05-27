# Effectual SSM-Ansible 

SSM-Ansible is a security configuration solution that will run ansible-playbook against the content of this repo. All Effectual linux servers are setup with SSM-Ansible deploying a minimum amount of configurations that are required on the Operating System to run under the Effectual AWS environment. To report a bug open an issue at the project site.

This Ansible playbook is structured into a role-based playbook. This enables granular control over the configuration performed, through simple selection of the available roles within the playbook via updating the local.yml file found in the root of the repository directory. Currently, all roles are active by default. Edit local.yml prior to execution to select roles.

## System Configurations
The Effectual SSM-Ansible configuration management will conduct the following system configurations based on the Effectual Baselines:

### General Banner Configurations
Playbook modifies the banner, motd, issue, issue.net files to apply necessary federal configurations. These configurations are not specific to distro or version and can be found in the [tasks/banner.yaml](tasks/banner.yaml) file.

Task Name  | NIST 800-53 Fulfillment
---------  | -----------------------
Modify the System Login Banner - /etc/issue | AC,AC-8,AC-8(a),AC-8(b),AC-8(c)
Modify the System Login Banner - /etc/issue.net | AC,AC-8,AC-8(a),AC-8(b),AC-8(c)
Modify the System Login Banner - /etc/motd  | AC,AC-8,AC-8(a),AC-8(b),AC-8(c)

### General logrotate Configurations
These configurations are not specific to distro or version. Details can be found in the [tasks/logrotate.yaml](tasks/logrotate.yaml) file.

Task Name  | NIST 800-53 Fulfillment | Notes
---------  | ----------------------- | -----
Ensure Logrotate Runs Periodically | AU-9 | Ensures that logrotate service is running with whatever configurations system has
Create logrotate entry for SSM-Ansible.log | AU-9 | Sets the logrotate specifics for SSM-Ansible logs

### General Service Security Configurations
The service security configurations applies basic security through verification of adding and enabling secure services while removing insecure services. Details can be found in the [tasks/services.yaml](tasks/services.yaml) file.

The current implementation of the Ansible tasks sets the errors to be ignored. Thus the results log will show errors if the service is not installed or available to be set to disabled. We are currently working on a better method to improve log error false-positives.

Task Name  | NIST 800-53 Fulfillment
---------  | -----------------------
Enable cron Service  | CM,CM-7
Disable xinetd Service  | CM,CM-7
Uninstall xinted Package | CM,CM-7
Disable telnet Service | CM,CM-7
Uninstall telnet Package | CM,CM-7
Disable rexec Service | CM,CM-7
Disable rsh Service | CM,CM-7
Disable rlogin Service | CM,CM-7
Uninstall rsh-server Package | CM,CM-7
Remove Rsh Trust Files - /etc/hosts.equiv | M,CM-7
Find User Rsh Trust Files | CM,CM-7
Remove Rsh Trust Files - ~/.rhosts  | CM,CM-7
Disable ypbind Service | CM,CM-7
Uninstall ypserv Package | CM,CM-7
Disable tftp Service | CM,CM-7
Uninstall tftp-server Package | CM,CM-7
Disable avahi Service | CM,CM-7
Disable ntpdate Service | CM,CM-7
Disable Network Router Discovery Daemon (rdisc)  | CM,CM-7
Disable Network Console (netconsole) | CM,CM-7

### SSH Security Configurations
The following configurations are applied to the SSH installation:

Task Name | NIST 800-53 Fulfillment | Details
--------- | ----------------------- | -------
Find SSH Keys | AC,AC-6 | Finds all ssh keys on server
Verify Permissions on SSH Server Private *_key Key Files | AC,AC-6 | Changes permissions to 600
Allow Only SSH Protocol 2 | IA,IA-5,IA-5(1),IA-5(c) |
Set SSH Idle Timeout Interval | AC,AC-2,AC-2(5) | Sets ClientAliveInterval 600
Set SSH Client Alive Count | AC,AC-2,AC-2(5) | Sets ClientAliveCountMax 0
Disable SSH Support for .rhosts Files | AC,AC-3 | One host can allow an attacker to move trivially to other hosts.
Disable Host-Based Authentication | AC-3,AC | One host can allow an attacker to move trivially to other hosts.
Disable SSH Root Login | AC,AC-3,AC-6,AC-6(2),IA,IA-2,IA-2(1) | Stops root from logging in via SSH
Disable SSH Access via Empty Passwords | AC,AC-3 | Sets PermitEmptyPasswords no in the sshd_config
Enable SSH Warning Banner | AC,AC-8,AC-8(a),AC-8(b),AC-8(c) | Sets login banner for SSH
Use Only Approved Ciphers | AC,AC-3,AC-17,AC-17(2) | Limits Ciphers used for SSH
Use Only FIPS Approved MACs  | AC,AC-17,AC-17(2),IA,IA-7,SC,SC-13 | Limits MACs used for SSH
Disable SSH Support for User Known Hosts | CM,CM-6,CM-6(a) | Sets IgnoreUserKnownHosts to YES
Disable SSH Support for Rhosts RSA Authentication | CM,CM-6,CM-6(a) | Sets RhostsRSAAuthentication to NO
Enable Use of StrictModes | AC,AC-6 | Sets StrictModes to YES
Enable Use of Privilege Separation | AC,AC-6 | Set UsePrivilegeSeparation to YES
Disable Compression Or Set Compression to delayed  | CM,CM-6,CM-6(b) | Set Compression to "delayed"
Do Not Allow SSH Environment Options  |  |  Sets PermitUserEnvironment to NO


## SSM
The amazon-ssm-agent is installed through the ssm role. It condigures the agent to enable SSM management of an instance through the AWS console. Note: An instance must have the correct role permissions attached to be managed through ssm.

## Postfix
Postfix is installed on all systems

## Installed Software
The following software and applications required by the USGS will be installed via SSM-Ansible:

Application | Description
----------- | -----------
AWSCLI      | Verifies AWSCLI is installed
AWSLogs     | syslog functionality leveraging AWS CloudWatch
SSM         | Install and Enable

## Supported Systems

Operating System  | IDNotes | Status
---------------- | --- | ------- | ------
AWS Linux 2  | Amazon 2 | Full Support


