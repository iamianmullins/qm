The package configures the bluechi agent within the QM.

- [BlueChi](https://github.com/containers/qm/pull/57)

BlueChi is a systemd service controller intended for multi-node environments with
a predefined number of nodes and with a focus on highly regulated ecosystems such
as those requiring functional safety. Potential use cases can be found in domains
such as transportation, where services need to be controlled across different
edge devices and where traditional orchestration tools are not compliant with
regulatory requirements.

Systems with QM installed will have two systemd's running on them. The QM bluechi-agent
is based on the hosts /etc/bluechi/agent.conf file. By default any changes to the
systems agent.conf file are reflected into the QM /etc/bluechi/agent.conf. You can
further customize the QM bluechi agent by adding content to the
/usr/lib/qm/rootfs/etc/bluechi/agent.conf.d/ directory.

```console
# dnf install -y python3-dnf-plugins-core
# dnf config-manager --set-enabled crb
```