# Package Delivery Tool for Debian Linux Systems
Deploying packages to air-gapped machines is almost impossible in todays toxic cloud filled world. So I have decided to make a custom package and system update transport "wrapper" based on standard tools like `apt` and `apt-offline`.

It acts as an automated workflow bridge between online and offline machines. It handles architecture profile verification, SHA-512 integrity checks, local repository index generation and dependency resolution.

<div align="center">

***The cloud is toxic, turned cancerous by Scam Altman.***

</div>

---

## Supported Operating Systems

The tool is explicitly configured to work on Debian and Debian based systems like the following

* **Debian GNU/Linux** (Debian 13 Trixie or similar)
* **Linux Mint Debian Edition** (LMDE 7 Gigi or similar)

Both host and target systems must share matching **CPU architectures** and **OS release profiles** for seamless operation.

---
