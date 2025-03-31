## QM is a containerized environment for running Functional Safety QM (Quality Management) software

The main purpose of this package is to allow users to set up an environment which
prevents applications and container tools from interfering with other processes
on the system. For example ASIL (Automotive Safety Integrity Level) environments.

The QM environment uses containerization tools like cgroups, namespaces, and
security isolation to prevent accidental interference by processes in the qm.

The QM will run its own version of systemd and Podman to isolate not only the
applications and containers launched by systemd and Podman but systemd and
Podman commands themselves.

This package requires the Podman package to establish the containerized
environment and uses quadlet to set it up.

Software install into the qm environment under /usr/lib/qm/rootfs will
be automatically isolated from the host. But if developers want to further
isolate these processes from other processes in the QM they can use container
tools like Podman to further isolate.

## Development

If you're looking for contribute to the project use our [development README guide](docs/docs/devel/README.md) as start point.

## Talks and Videos

Let's spread the knowledge regarding QM, if you have any interesting video regarding any
technology related to QM please with us.

[Talks and Videos](#talks-and-videos)
- [Paving the Way for Uninterrupted Car Operations - DevConf Boston 2024](https://www.youtube.com/watch?v=jTrLqpw7E6Q)
- [Security - Sample Risk Analysis according to ISO26262](https://www.youtube.com/watch?v=jTrLqpw7E6Q&t=1268s)
- [ASIL and QM - Simulation and Service Monitoring using bluechi and podman](https://www.youtube.com/watch?v=jTrLqpw7E6Q&t=1680s)
- [Containers in a Car - DevConf.CZ 2023](https://www.youtube.com/watch?v=FPxka5uDA_4)


## RPM Mirrors

Looking for a specific version of QM?
Search in the mirrors list below.

[CentOS Automotive SIG - qm package - noarch](https://mirror.stream.centos.org/SIGs/9-stream/automotive/aarch64/packages-main/Packages/q/)
