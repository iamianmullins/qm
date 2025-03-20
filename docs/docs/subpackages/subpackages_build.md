## Building QM sub-packages

Choose one of the following sub-packages and build using make.

```bash
git clone git@github.com:containers/qm.git && cd qm

Example of subpackages: input, kvm, sound, tty7, ttyUSB0, video, windowmanager

make TARGETS=input subpackages
ls rpmbuild/RPMS/noarch/
qm-0.6.7-1.fc40.noarch.rpm  qm_mount_bind_input-0.6.7-1.fc40.noarch.rpm
```