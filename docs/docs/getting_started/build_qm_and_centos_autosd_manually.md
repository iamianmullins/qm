## Building CentOS AutoSD and QM manually

During development, it is common to conduct integration tests to ensure your
changes work well with other components within the overall solution.
In our case, it's best to test against the CentOS Automotive Stream
Distribution (AutoSD) image.

Once you have the new [RPM](#building-qm-rpm-manually-with-changes), follow these steps:

**1.** Make sure the new rpm is located in **/USER/rpmbuild/RPMS/**

Example

```bash
ls /root/rpmbuild/RPMS/noarch/qm-1.0-1.noarch.rpm
/root/rpmbuild/RPMS/noarch/qm-1.0-1.noarch.rpm
```

**2.** Create a local repository with the new package

```bash
dnf install createrepo_c -y
cd /root/rpmbuild/RPMS/
createrepo .
```

**4.** Clone the CentOS Automotive distro for the build

Ensure you meet the requirements for the CentOS Automotive Stream by
referring to [this link](https://sigs.centos.org/automotive/building/).

The following commands will execute:

- Cleanups before a fresh build
- Remove old qcow2 images used (regular and ostree)
- Finally creates a new image (BASED ON target name, ostree or regular)
  NOTE:
  - The path for the new QM rpm file (/root/rpmbuild/RPMS/noarch)
  - extra_rpms - useful for debug (do not use spaces between packages or will break)
  - ssh enabled

```bash
dnf install podman -y && dnf clean all
git clone https://gitlab.com/CentOS/automotive/sample-images.git
git submodule update --init
cd sample-images/
rm -rf _build
rm -f cs9-qemu-qmcontainer-regular.x86_64.qcow2
rm -f cs9-qemu-qmcontainer-ostree.x86_64.qcow2
./build --distro cs9 --target qemu --define 'extra_repos=[{\"id\":\"local\",\"baseurl\":\"file:///root/rpmbuild/RPMS/noarch\"}]' --define 'extra_rpms=[\"qm-1.0\",\"vim-enhanced\",\"strace\",\"dnf\",\"gdb\",\"polkit\",\"rsync\",\"python3\",\"openssh-server\",\"openssh-clients\"]' --define 'ssh_permit_root_login=true' --define 'ssh_permit_password_auth=true' cs9-qemu-qmcontainer-regular.x86_64.qcow2
```

Run the virtual machine, default user: root, pass: password.
To change default values, use the [defaults.ipp.yml](https://gitlab.com/CentOS/automotive/src/automotive-image-builder/-/blob/main/include/defaults.ipp.yml) file.

```bash
./runvm --nographics ./cs9-qemu-qm-minimal-regular.x86_64.qcow2
```