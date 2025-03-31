## How the OOM Score Adjustment is used in QM

The om_score_adj refers to the "Out of Memory score adjustment" in Linux operating systems. This parameter is used by the Out of Memory (OOM) killer to decide which processes to terminate when the system is critically low on memory.

### Why use oomscoreadj in QM?

By fine-tuning which processes are more likely to be terminated during low memory situations, critical processes can be protected, thereby enhancing the overall stability of the system. For instance only, ASIL (Automotive Safety Integrity Level) applications, which are critical for ensuring functional safety in automotive systems, will be preserved in case of low resources.

### OOM Score Adjustment in QM

#### Nested Containers

- All nested containers created inside QM will have their OOM score adjustment set to *750*.

```console
$ cat /usr/share/qm/containers.conf | grep oom_score_adj
oom_score_adj = 750
```

#### QM Process

- The QM process has a default OOM score adjustment value set to *500*, configured via the *qm.container* file.

```console
cat /usr/share/containers/systemd/qm.container | grep OOMScoreAdjust
# OOMScoreAdjust=500
```

### ASIL Applications

If we consider the example of ASIL (Automotive Safety Integrity Level) applications, which are essential for maintaining functional safety in automotive systems, their OOM score adjustment values can range from -1 to -1000. Setting the value to -1000 makes the process immune to the OOM killer. This ensures that ASIL applications are the last to be terminated by the OOM killer, thus prioritizing their operation even in low memory situations.

#### Highlights

- Nested Containers inside QM: OOM score adjustment set to 750. (/usr/share/qm/containers.conf)
- QM Process: OOM score adjustment value set to 500, configured via the qm.container file.
- ASIL Applications: Can explore a range from -1 to -1000, with -1000 making the process immune to the OOM killer.

#### ASCII Diagram

```txt
+-------------------------------------------------------------+
| The Priority Process of OOM Killer in the QM Context        |
+-------------------------------------------------------------+

------------------------------------ Kernel space -----------------------------------------------

                           +--------------------------------+
                           | Out of Memory Killer Mechanism |
                           |          (OOM Killer)          |
                           +--------------------------------+
                                          |
                                          v
                           +--------------------------------+
                           |       Kernel Scheduler         |
                           +--------------------------------+

------------------------------------ User space -------------------------------------------------

                    +----------------------------------------+
                    |       Out of Memory Score Adjustment   |
                    |            (oom_score_adj)             |
                    +----------------------------------------+
                                    |
                                    |
                                    v (Processes Priority side by side)
      +-----------------------------+--------------------------+-----------------------+
      |                             |                          |                       |
      v                             v                          v                       v
+------------------+  +----------------------------+   +-----------------+     +-----------------+
|                  |  |                            |   |                 |     |                 |
|    QM Container  |  |  Nested Containers by QM   |   |  ASIL Apps      |     | Other Processes |
|                  |  |                            |   |                 |     |                 |
|     OOM Score    |  |         OOM Score          |   |   OOM Score     |     |    OOM Score    |
|        500       |  |            750             |   |   -1 to -1000   |     |    (default: 0) |
+------------------+  +----------------------------+   +-----------------+     +-----------------+
          |                         |                           |                      |
          v                         v                           v                      v
   +----------------+      +----------------+         +--------------------+    +-----------------+
   | Lower priority |      | Higher priority|         | Very low priority  |    | Default priority|
   | for termination|      | for termination|         | for termination    |    | for termination |
   +----------------+      +----------------+         +--------------------+    +-----------------+
                                    |
                                    |
                                    |
                                    v
         +-------------------------------------------------------------+
         |                                                             |
         | In conclusion, all nested containers created inside QM have |
         | their OOM score adjustment set to 750, making them more     |
         | likely to be terminated first compared to the QM process.   |
         |                                                             |
         | When compared to ASIL applications, nested containers       |
         | will have an even higher likelihood of being terminated.    |
         |                                                             |
         | Compared to other processes with the default adjustment     |
         | value of 0, nested containers are still more likely to be   |
         | terminated first, ensuring the system and ASIL Apps are     |
         | kept as safe as possible.                                   |
         |                                                             |
         +-------------------------------------------------------------+

------------------------------------ User space -------------------------------------------------

------------------------------------ Kernel space -----------------------------------------------
```