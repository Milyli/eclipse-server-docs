<img src="/img/Milyli Logo_blue product.png" style="height: 80px;" />

# Hosting Eclipse w/ Docker Engine on Ubuntu

## Requirements

The following user guide was built using the Ubuntu Noble 24.04 (LTS, 64-bit) running via Hyper-V on a Windows 11 desktop computer on Milyli’s private network.  

Nearly all steps followed in this guide can be followed regardless of the host machine of the Linux distribution including a Linux server running Ubuntu, a Windows Server with an Ubuntu VM, or a VM hosted on a cloud service such as Azure or AWS.

Native cloud service hosting is supported but not covered via this document.

This guide will install and use Docker Engine via Docker’s apt repository rather than use Docker Desktop for Linux. If you are using an existing server, you should ensure that old docker files are removed. For more details refer to the [Docker removal instructions](https://docs.docker.com/engine/install/ubuntu/#uninstall-old-versions).

Note: For more general information about Docker and its related components, please refer to the [official documentation](https://www.docker.com/).

**Supported Ubuntu Versions (64-bit)**

- Ubuntu Noble 24.04 (LTS)
- Ubuntu Mantic 23.10 (EOL: July 12, 2024)
- Ubuntu Jammy 22.04 (LTS)
- Ubuntu Focal 20.04 (LTS)

## VM Setup and Docker Engine Installation

### Installing Ubuntu VM

Your approach to hosting Linux should not impact the following steps if a supported 64-bit Ubuntu distribution is used. This guide utilizes **Ubuntu 22.04 LTS** installed on a networked computer with **Hyper-V Quick Create**.

Launch the Hyper-V Quick Create tool and select Ubuntu 22.04 LTS.

![CreateUbuntuVirtualMachine](img/CreateUbuntuVirtualMachine.png)
