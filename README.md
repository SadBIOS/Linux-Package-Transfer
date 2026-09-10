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

## Command Line Reference

| Option | Purpose | Requirements / Input |
| :--- | :--- | :--- |
| `--prep-dep-pack` | Bundles core bootstrap tools (`apt-offline`, `gnupg`) | Run on Online machine |
| `--resolve-deps <archive>` | Installs core bootstrap tools on offline target | Run on Offline machine with preload archive |
| `--gen-pkglist <file>` | Creates a target package bundle based on a text list | Run on Online machine with package list file |
| `--dgst-pkglist <archive>` | Ingests and installs target package bundle | Run on Offline machine with package archive |
| `--gen-sys-meta-req` | Prepares an APT list update request archive | Run on Offline machine |
| `--gen-sys-upgrd-req` | Prepares a full system upgrade request archive | Run on Offline machine |
| `--fetch-sys-upgrd <req-archive>` | Downloads updates/packages using a request archive | Run on Online machine with request archive |
| `--dgst-sys-upgrd-arch <bundle>` | Ingests update metadata or performs system upgrade | Run on Offline machine with fetched bundle |
| `--cleanup-env` | Cleans up local temporary caches and builds | Run on any machine |

---
