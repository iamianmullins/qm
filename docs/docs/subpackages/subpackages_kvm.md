## QM sub-package KVM

The QM sub-package KVM includes drop-in configuration that enables the integration of Kernel-based Virtual Machine (KVM) management into the QM (Quality Management) container environment. This configuration allows users to easily configure and manage KVM virtual machines within the QM system, streamlining virtualization tasks in containerized setups.

Below example step by step:

Step 1:  clone QM repo, install libvirt packages, prepare some files inside QM and start the libvirt daemon.

```bash
git clone https://github.com/containers/qm.git && cd qm
make TARGETS=kvm subpackages
sudo dnf install rpmbuild/RPMS/noarch/qm_mount_bind_kvm-0.6.7-1.fc40.noarch.rpm
sudo podman restart qm  # if you have qm already running
sudo dnf --installroot /usr/lib/qm/rootfs/ install virt-install libvirt-daemon libvirt-daemon-qemu libvirt-daemon-kvm -y
sudo restorecon -R -v /usr/lib/qm

# Copy default network settings to /root dir inside QM (/usr/lib/qm/rootfs/root)
$host> sudo cp /usr/share/libvirt/networks/default.xml /usr/lib/qm/rootfs/root

Step 2: Preparing cloudinit files inside QM (/usr/lib/qm/rootfs/root)

# Cloud-init files
------------------------------
$host> cd /usr/lib/qm/rootfs/root/
$host> cat meta-data
instance-id: fedora-cloud
local-hostname: fedora-vm

# We are setting to user fedora the default password as fedora
$host> cd /usr/lib/qm/rootfs/root/
$host> cat user-data
#cloud-config
password: fedora
chpasswd: { expire: False }
ssh_pwauth: True

# Download the Fedora Cloud image for tests and save it /usr/lib/qm/rootfs/var/lib/libvirt/images/
$ wget -O /usr/lib/qm/rootfs/root/Fedora-Cloud-Base-Generic.qcow2 https://download.fedoraproject.org/pub/fedora/linux/releases/40/Cloud/x86_64/images/Fedora-Cloud-Base-Generic.x86_64-40-1.14.qcow2

# Generate the cloud-init.iso and move it to /usr/lib/qm/rootfs/var/lib/libvirt/images/
$host> cloud-localds cloud-init.iso user-data meta-data
$host> mv cloud-init.iso /usr/lib/qm/rootfs/var/lib/libvirt/images/

# Change permission to qemu:qemu
$host> chown qemu:qemu /usr/lib/qm/rootfs/var/lib/libvirt/*

Step 3: Starting libvirtd and checking if it's active inside QM

##################################################################
# Keep in mind for the next steps:
# Depending on the distro you are running SELinux might complain
# about libvirtd running on QM / udev errors
##################################################################

# Going inside QM
$ sudo podman exec -it qm bash

# Starting libvirtd
bash-5.2# systemctl start libvirt

# Check if it's running:
bash-5.2# systemctl is-active libvirtd
active
```

Step 4: Creating a script inside QM and running the VM

```bash
$host> cd /usr/lib/qm/rootfs/root/
$host> vi run
##### START SCRIPT ############
# Set .cache to /tmp
export XDG_CACHE_HOME=/tmp/.cache

# Remove previous instance
virsh destroy fedora-cloud-vm 2> /dev/null
virsh undefine fedora-cloud-vm 2> /dev/null

# Network
virsh net-define ./default.xml 2> /dev/null
virsh net-start default 2> /dev/null
virsh net-autostart default 2> /dev/null

# Install
virt-install \
--name fedora-cloud-vm \
--memory 20048 \
--vcpus 4 \
--disk path=/var/lib/libvirt/images/Fedora-Cloud-Base-Generic.qcow2,format=qcow2 \
--disk path=/var/lib/libvirt/images/cloud-init.iso,device=cdrom \
--os-variant fedora-unknown \
--network network=default \
--import \
--graphics none \
--console pty,target_type=serial \
--noautoconsole

##### END SCRIPT ############
```

Step 5: Running the script

```bash
qm$ sudo podman exec -it qm bash
bash-5.2# cd /root
bash-5.2# ./run
Domain 'fedora-cloud-vm' destroyed

Domain 'fedora-cloud-vm' has been undefined

Network default marked as autostarted

Starting install...
Creating domain...                                                                                        |    0 B  00:00:00
Domain creation completed.
bash-5.2# virsh list
 Id   Name              State
---------------------------------
 4    fedora-cloud-vm   running

bash-5.2# virsh console fedora-cloud-vm

fedora-vm login: fedora
Password:

Last login: Tue Oct  8 06:01:18 on ttyS0
[fedora@fedora-vm ~]$
```