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

## Workflow Examples

### 1. Initial Offline Bootstrap (Dependency Resolution)

Before performing advanced offline operations, the air-gapped machine needs `apt-offline` and its baseline dependencies.

1. On the **online machine**, generate the bootstrap archive:

   ```bash
   ./deploy_tool.sh --prep-dep-pack
   ```

   This produces an archive named `depsys-preload-TIMESTAMP.tar.gz`.

2. Transfer `depsys-preload-TIMESTAMP.tar.gz` to the **offline machine**.

3. On the **offline machine**, resolve and install the bootstrap dependencies:

   ```bash
   ./deploy_tool.sh --resolve-deps depsys-preload-TIMESTAMP.tar.gz
   ```

---

### 2. Custom Package Deployment

To install specific packages (such as build toolchains or development libraries) on an air-gapped host:

1. Create a plain text file containing your desired packages (for example, `pkglist.txt`), with one package per line or space-separated names. Comment lines starting with `#` are ignored.

2. On the **online machine**, generate the package bundle:

   ```bash
   ./deploy_tool.sh --gen-pkglist pkglist.txt
   ```

   This produces an archive named `depsys-custpkg-TIMESTAMP.tar.gz`.

3. Transfer `depsys-custpkg-TIMESTAMP.tar.gz` to the **offline machine**.

4. On the **offline machine**, install the packages:

   ```bash
   ./deploy_tool.sh --dgst-pkglist depsys-custpkg-TIMESTAMP.tar.gz
   ```

---

### 3. Full System Update Workflow

System upgrades on air-gapped nodes require a two-stage request and fetch cycle.

#### Stage A: Refreshing APT Metadata Lists

1. On the **offline machine**, generate an APT metadata update request:

   ```bash
   ./deploy_tool.sh --gen-sys-meta-req
   ```

   This produces `depsys-meta-req-TIMESTAMP.tar.gz`.

2. Transfer `depsys-meta-req-TIMESTAMP.tar.gz` to the **online machine** and fetch the metadata bundle:

   ```bash
   ./deploy_tool.sh --fetch-sys-upgrd depsys-meta-req-TIMESTAMP.tar.gz
   ```

   This produces `depsys-meta-bundle-TIMESTAMP.tar.gz`.

3. Transfer `depsys-meta-bundle-TIMESTAMP.tar.gz` back to the **offline machine** and digest it:

   ```bash
   ./deploy_tool.sh --dgst-sys-upgrd-arch depsys-meta-bundle-TIMESTAMP.tar.gz
   ```

#### Stage B: Performing the Upgrade

1. On the updated **offline machine**, generate the system upgrade request:

   ```bash
   ./deploy_tool.sh --gen-sys-upgrd-req
   ```

   This produces `depsys-upgrd-req-TIMESTAMP.tar.gz`.

2. Transfer `depsys-upgrd-req-TIMESTAMP.tar.gz` to the **online machine** and fetch the upgrades:

   ```bash
   ./deploy_tool.sh --fetch-sys-upgrd depsys-upgrd-req-TIMESTAMP.tar.gz
   ```

   This produces `depsys-final-TIMESTAMP.tar.gz`.

3. Transfer `depsys-final-TIMESTAMP.tar.gz` to the **offline machine** and execute the upgrade:

   ```bash
   ./deploy_tool.sh --dgst-sys-upgrd-arch depsys-final-TIMESTAMP.tar.gz
   ```
