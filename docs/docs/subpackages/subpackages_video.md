## QM sub-package Video

The video sub-package exposes `/dev/video0` (or many video devices required) to the container. This feature is useful for demonstrating how to share a camera from the host system into a container using Podman drop-in. To showcase this functionality, we provide the following demo:

### Building the video sub-package, installing, and restarting QM

```bash
make TARGETS=video subpackages

sudo podman restart qm
sudo dnf install ./rpmbuild/RPMS/noarch/qm_mount_bind_video-0.6.7-1.fc40.noarch.rpm
```

This simulates a rear camera when the user shifts into reverse gear.

In this simulation, we created a systemd service that, every time it is started, captures a snapshot from the webcam, simulating the action of a rear camera. (Feel free to start and restart the service multiple times!)

```bash
host> sudo podman exec -it qm bash

bash-5.2# systemctl daemon-reload
bash-5.2# systemctl start rear-camera

# ls -la /tmp/screenshot.jpg
-rw-r--r--. 1 root root 516687 Oct 13 04:05 /tmp/screenshot.jpg
bash-5.2#
```

### Copy the screenshot to the host and view it

```bash
host> sudo podman cp qm:/tmp/screenshot.jpg .
```

Great job! Now imagine all the possibilities this opens up!