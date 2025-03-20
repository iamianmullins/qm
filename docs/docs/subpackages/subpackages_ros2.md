## QM sub-package ROS2

The QM sub-package ROS2 (a.k.a "The Robot Operating System" or middleware for robots) is widely used by open-source projects, enterprises, companies, edge env and government agencies, including NASA, to advance robotics and autonomous systems. Enabled by Quadlet in QM, ROS2 on top of QM provides a secure environment where robots can operate and communicate safely, benefiting from QM's "Freedom from Interference" frequently tested layer. This ensures robots can function without external interference, enhancing their reliability and security.

The types of robots compatible with this environment are extensive, ranging from medical devices and aerial drones to aqua drones and space rockets. ROS2 within QM supports high availability, meaning these robotic systems can maintain continuous operations, crucial for mission-critical and industrial applications. This versatility makes it ideal for environments that demand robust communication and operational safety, from healthcare and aerospace to underwater exploration and autonomous land vehicles.

How to test this env?

```bash
git clone https://github.com/containers/qm.git && cd qm
make TARGETS=ros2_rolling subpackages
sudo dnf install rpmbuild/RPMS/noarch/qm_ros2_rolling-0.6.7-1.fc40.noarch.rpm  -y
sudo podman restart qm  # if you have qm already running

Testing using talked and listener examples
$host> sudo podman exec -it qm bash
QM> ros2 run demo_nodes_cpp talker &
QM> ros2 run demo_nodes_cpp listener
```