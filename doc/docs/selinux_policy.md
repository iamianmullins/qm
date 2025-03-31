This policy is used to isolate Quality Management parts of the operating system
from the other Domain-Specific Functional Safety Levels (ASIL).

The main purpose of this policy is to prevent applications and container tools
with interfering with other processes on the system. The QM needs to support
further isolate containers run within the qm from the qm_t process and from
each other.

For now all of the control processes in the qm other then containers will run
with the same qm_t type.

Still would like to discuss about a specific selinux prevision?
Please open an [QM issue](https://github.com/containers/qm/issues) with the output of selinux error from a recent operation related to QM. The output of the following commands are appreciated for understanding the root cause.

```console
ausearch -m avc -ts recent | audit2why
journalctl -t setroubleshoot
sealert -a /var/log/audit/audit.log
```