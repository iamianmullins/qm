## Examples

Looking for quadlet examples files? 
See our example of [Android container running on top of kvm using quadlet and Wayland](quadlet-examples/virtualization/android/README.md).

## Tutorials

[Network Settings](tutorials/NETWORK.md).
[How to change the variables in qm containers.conf](qm_containers.conf/README.md)

## Realtime

To enable real-time removal of sched_* blockage via seccomp, use the following schema.

```bash
cat << EOF >> /etc/containers/systemd/qm.container.d/rt.conf
> [Container]
SeccompProfile=""
> EOF
```

## Talks and Videos

Let's spread the knowledge regarding QM, if you have any interesting video regarding any
technology related to QM please reach out to us.

Check out some of our talks below
- [Paving the Way for Uninterrupted Car Operations - DevConf Boston 2024](https://www.youtube.com/watch?v=jTrLqpw7E6Q)
- [Security - Sample Risk Analysis according to ISO26262](https://www.youtube.com/watch?v=jTrLqpw7E6Q&t=1268s)
- [ASIL and QM - Simulation and Service Monitoring using bluechi and podman](https://www.youtube.com/watch?v=jTrLqpw7E6Q&t=1680s)
- [Containers in a Car - DevConf.CZ 2023](https://www.youtube.com/watch?v=FPxka5uDA_4)


## RPM Mirrors

Looking for a specific version of QM?
Search in the mirrors list below.

[CentOS Automotive SIG - qm package - noarch](https://mirror.stream.centos.org/SIGs/9-stream/automotive/aarch64/packages-main/Packages/q/)