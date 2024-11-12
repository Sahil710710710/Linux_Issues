# Configuring NFS

## 1. **Configure NFS (Network File System)**

### Step 1: Install NFS server package

dnf install nfs-utils -y

### Step 2: Start and enable NFS server

systemctl start nfs-server
systemctl enable nfs-server
systemctl status nfs-server

### Step 3: Configure shared directory
Edit the `/etc/exports` file to specify which directories to share and configure access permissions.

vi /etc/exports
/path/to/share  *(rw,sync,no_root_squash)

### Step 4: Export the directories
Run a command to export the shared directories so that clients can mount them.

exportfs -a

### Step 5: Allow NFS service through the firewall
Ensure that the NFS service is allowed through the system firewall.

firewall-cmd --permanent --add-service=nfs
firewall-cmd --reload

### Step 6: Test the NFS share
From a client machine, mount the shared directory from the NFS server to verify the setup.

mount server_ip:/path/to/share

---xxx---


