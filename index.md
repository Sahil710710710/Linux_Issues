# Common Issues and Solutions on Red Hat Servers

## Issue 1: Migration of CentOS 7 to RHEL 7
**Description:** Encountered a repository error during the migration from CentOS 7 to RHEL 7.

**Solution:** Configured the correct RHEL repository and package compatability to proceed with the update.

**Note:** Direct migration from CentOS to RHEL does not support certain packages, which need to be removed beforehand to avoid conflicts..

---

## Issue 2: Data Corruption in Simultaneous Access to ext4 on iSCSI Shared Storage
**Description:** Faced data corruption issues when files in an ext4-partitioned iSCSI shared storage were accessed simultaneously by multiple servers.

**Solution:** The ext4 filesystem is not designed for simultaneous access across multiple servers, as it can lead to data corruption. To allow safe simultaneous access, configure the servers in a cluster and use a clustered filesystem (such as GFS2 or BTRFS), which supports concurrent access in a shared storage environment.

---

## Issue 3: SELinux Permissions Blocking Access
**Description:** SELinux blocks certain services or applications from accessing files or ports, resulting in "permission denied" errors even when file permissions are correct.

**Solution:** Temporarily disable SELinux (`setenforce 0`) to troubleshoot, but re-enable (`setenforce 1`) and apply appropriate policies for a secure environment.

---

## Issue 4: YUM/DNF Repository Errors
**Description:** Errors occur when using `yum` or `dnf` due to missing or misconfigured repositories, causing installation or update failures.

**Solution:** Check and update the repository configuration files in `/etc/yum.repos.d/`. Ensure the server has internet access or a valid repository mirror. Run `yum clean all` or `dnf clean all` to clear the cache, and try the command again.

---

## Issue 5: Time Synchronization Issues
**Description:** System time does not stay synchronized with network time protocol (NTP) servers, potentially causing issues with applications relying on accurate timestamps.

**Solution:** Ensure that `chronyd` or `ntpd` services are installed and running. Check the configuration in `/etc/chrony.conf` (for `chronyd`) or `/etc/ntp.conf` (for `ntpd`) to confirm valid NTP servers are specified. Use `timedatectl` to verify the system clock status.

---

## Issue 6: Disk Space Running Out on Root Filesystem
**Description:** The root filesystem fills up, preventing applications and the OS from functioning correctly.

**Solution:** Use `df -h` to check disk usage. Identify large files or directories with `du -sh /* | sort -h`. Consider clearing logs in `/var/log`, removing unused packages, and, if possible, resizing or expanding the filesystem. Enabling log rotation can also help prevent future issues.

---

## Issue 7: Network Connectivity Problems
**Description:** The server experiences intermittent or complete loss of network connectivity, which impacts access to resources, repositories, and other services.

**Solution:** Use commands like `ping`, `ip a`, and `nmcli` to troubleshoot connectivity and network interface configurations. Verify that the correct IP, gateway, and DNS are set in `/etc/sysconfig/network-scripts/`. If using firewalls, ensure that essential ports are open. Restart the network service with `systemctl restart NetworkManager` (on RHEL 8 and above) or `nmcli n off && nmcli n on` (on RHEL 8 and later) to reinitialize connections.

---

## Issue 8: SSH Login Failure Due to Configuration
**Description:** Users cannot log in via SSH, encountering "Permission denied" errors or getting disconnected immediately upon connection.

**Solution:** Check the SSH configuration in `/etc/ssh/sshd_config`. Ensure settings such as `PermitRootLogin`, `PasswordAuthentication`, and `AllowUsers` are configured as needed. Restart the SSH service with `systemctl restart sshd` after making changes. Verify that the server’s firewall allows traffic on port 22 (or a custom SSH port). You can also inspect logs in `/var/log/secure` for details on SSH login attempts.

---

## Issue 9: File Permissions Causing Application Errors
**Description:** Applications fail to start or encounter errors due to insufficient permissions on required files or directories.

**Solution:** Verify ownership and permissions using `ls -l` on the problematic files and directories. Adjust permissions with `chmod` and ownership with `chown` as needed. For example, to grant read and write permissions to the owner and group, use `chmod 770 /path/to/file`. For web applications, ensure that files are accessible by the `apache` or `nginx` user, depending on the server.

---
## Issue 10: `createrepo` was Not Working on RHEL 9
**Description:** Encountered an issue where `createrepo` was not functioning correctly on RHEL 9, preventing the creation of a local repository for offline package installations.

**Solution:** Mounted the ISO to another directory and created a local repository from it. This allowed the installation of packages without internet access.

