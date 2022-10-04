# Setup CodeReady Containers with KVM behind Proxy Server
- The official documentation is [here](https://code-ready.github.io/crc/).

## Index
- [Prerequisite](#prerequisite)
- [Evaluation Environment](#evaluation-environment)
- [Setup CRC](#setup-CRC)
- [Link](#link)

## Prerequisite
- You need Red Hat account to get the binary file of CodeReady Containers (CRC). You can create an account for free.
- On the download page, copy pull secret in advance. The pull secret is needed to start CRC.
- CRC needs about 9 GB RAM.
  ```sh
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
  23413 root      20   0 9390584   7.7g  13212 S 151.8 24.5 418:02.59 qemu-kvm
   :
  ```
  - For more detail, please refer to [Hardware requirements](https://code-ready.github.io/crc/#minimum-system-requirements-hardware_gsg).

## Evaluation Environment
### CRC 1.20.0
```
+----------------------------------------------------+
| Host OS                                            |
| - CentOS Linux release 7.9.2009 (Core)             |
| - KVM (virsh version)                              |
|   - Compiled against library: libvirt 4.5.0        |
|   - Using library: libvirt 4.5.0                   |
|   - Using API: QEMU 4.5.0                          |
|   - Running hypervisor: QEMU 2.12.0                |
| +------------------------------------------------+ |
| | Guest OS                                       | |
| | - CRC (crc version)                            | |
| |   - crc version: 1.20.0+ef3f80d                | |
| |   - OpenShift version: 4.6.6                   | |
| | - OpenShift (oc version)                       | | 
| |   - Client Version: 4.6.6                      | |
| |   - Server Version: 4.6.6                      | |
| |   - Kubernetes Version: v1.19.0+43983cd        | |
| +------------------------------------------------+ |
+----------------------------------------------------+
```
### CRC 1.18.0
```
+----------------------------------------------------+
| Host OS                                            |
| - CentOS Linux release 7.9.2009 (Core)             |
| - KVM (virsh version)                              |
|   - Compiled against library: libvirt 4.5.0        |
|   - Using library: libvirt 4.5.0                   |
|   - Using API: QEMU 4.5.0                          |
|   - Running hypervisor: QEMU 2.12.0                |
| +------------------------------------------------+ |
| | Guest OS                                       | |
| | - CRC (crc version)                            | |
| |   - crc version: 1.18.0+bb304aa                | |
| |   - OpenShift version: 4.6.1                   | |
| | - OpenShift (oc version)                       | | 
| |   - Client Version: 4.6.1                      | |
| |   - Server Version: 4.6.1                      | |
| |   - Kubernetes Version: v1.19.0+d59ce34        | |
| +------------------------------------------------+ |
+----------------------------------------------------+
```
### CRC 1.10.0
```
+----------------------------------------------------+
| Host OS                                            |
| - CentOS Linux release 7.8.2003 (Core)             |
| - KVM (virsh version)                              |
|   - Compiled against library: libvirt 4.5.0        |
|   - Using library: libvirt 4.5.0                   |
|   - Using API: QEMU 4.5.0                          |
|   - Running hypervisor: QEMU 2.12.0                |
| +------------------------------------------------+ |
| | Guest OS                                       | |
| | - CRC (crc version)                            | |
| |   - crc version: 1.10.0+9025021                | |
| |   - OpenShift version: 4.4.3                   | |
| - OpenShift (oc version)                       | | 
| |   - Client Version: 4.5.0-202004180718-6b061e3 | |
| |   - Server Version: 4.4.3                      | |
| |   - Kubernetes Version: v1.17.1                | |
| +------------------------------------------------+ |
+----------------------------------------------------+
```
### CRC 1.9.0
```
+----------------------------------------------------+
| Host OS                                            |
| - CentOS Linux release 7.8.2003 (Core)             |
| - KVM (virsh version)                              |
|   - Compiled against library: libvirt 4.5.0        |
|   - Using library: libvirt 4.5.0                   |
|   - Using API: QEMU 4.5.0                          |
|   - Running hypervisor: QEMU 2.12.0                |
| +------------------------------------------------+ |
| | Guest OS                                       | |
| | - CRC (crc version)                            | |
| |   - crc version: 1.9.0+a68b5e0                 | |
| |   - OpenShift version: 4.3.10                  | |
| | - OpenShift (oc version)                       | | 
| |   - Client Version: 4.5.0-202004180718-6b061e3 | |
| |   - Server Version: 4.3.10                     | |
| |   - Kubernetes Version: v1.16.2                | |
| +------------------------------------------------+ |
+----------------------------------------------------+
```

## Setup CRC
1. Create a user and add it to the following groups.
   ```sh
   # usermod -aG libvirt <user name>
   # usermod -aG wheel <user name>
   ```
1. Switch to the sudoer user.
   ```sh
   # su <user name>
   ```
1. Run the following command to run virsh command with sudoer user.
   ```sh
   $ export LIBVIRT_DEFAULT_URI="qemu:///system"
   ```
1. Log in Red Hat and download CRC binary file (e.g. crc-linux-amd64.tar.xz).
1. Expand the archive file.
   ```sh
   $ tar xvf crc-linux-amd64.tar.xz
   ```
1. Copy the binary file (crc) to your PATH (e.g. /usr/local/bin).
   ```sh
   $ cp ./crc-linux-1.9.0-amd64/crc /usr/local/bin
   ```
1. Run the following command.
   ```sh
   $ crc setup
   INFO Checking if oc binary is cached
   INFO Caching oc binary
   INFO Checking if podman remote binary is cached
   INFO Caching podman remote binary
   INFO Checking if CRC bundle is cached in '$HOME/.crc'
   INFO Unpacking bundle from the CRC binary
   cd INFO Checking if running as non-root
   INFO Checking if Virtualization is enabled
   INFO Checking if KVM is enabled
   INFO Checking if libvirt is installed
   INFO Checking if user is part of libvirt group
   INFO Checking if libvirt is enabled
   INFO Checking if libvirt daemon is running
   INFO Checking if a supported libvirt version is installed
   INFO Checking if crc-driver-libvirt is installed
   INFO Installing crc-driver-libvirt
   INFO Checking for obsolete crc-driver-libvirt
   INFO Checking if libvirt 'crc' network is available
   INFO Checking if libvirt 'crc' network is active
   INFO Checking if NetworkManager is installed
   INFO Checking if NetworkManager service is running
   INFO Checking if /etc/NetworkManager/conf.d/crc-nm-dnsmasq.conf exists
   INFO Checking if /etc/NetworkManager/dnsmasq.d/crc.conf exists
   Setup is complete, you can now run 'crc start' to start the OpenShift cluster
   ```
1. Setup the prameters related with proxy.
   ```sh
   $ crc config set http-proxy http://example.proxy.com:<port>
   $ crc config set https-proxy http://example.proxy.com:<port>
   $ crc config set no-proxy <comma-separated-no-proxy-entries>
   ```
1. Run the following command to start CRC.
   ```sh
   $crc start
   INFO Checking if oc binary is cached
   INFO Checking if podman remote binary is cached
   INFO Checking if running as non-root
   INFO Checking if Virtualization is enabled
   INFO Checking if KVM is enabled
   INFO Checking if libvirt is installed
   INFO Checking if user is part of libvirt group
   INFO Checking if libvirt is enabled
   INFO Checking if libvirt daemon is running
   INFO Checking if a supported libvirt version is installed
   INFO Checking if crc-driver-libvirt is installed
   INFO Checking if libvirt 'crc' network is available
   INFO Checking if libvirt 'crc' network is active
   INFO Checking if NetworkManager is installed
   INFO Checking if NetworkManager service is running
   INFO Checking if /etc/NetworkManager/conf.d/crc-nm-dnsmasq.conf exists
   INFO Checking if /etc/NetworkManager/dnsmasq.d/crc.conf exists
   ? Image pull secret [? for help] (copy pull secret from the download page and paste it) 
   INFO Extracting bundle: crc_libvirt_4.3.10.crcbundle ...
   INFO Checking size of the disk image /home/(user name)/.crc/cache/crc_libvirt_4.3.10/crc.qcow2 ...
   INFO Creating CodeReady Containers VM for OpenShift 4.3.10...
   INFO Verifying validity of the cluster certificates ...
   INFO Check internal and public DNS query ...
   INFO Check DNS query from host ...
   INFO Copying kubeconfig file to instance dir ...
   INFO Adding proxy configuration to the cluster ...
   INFO Adding proxy configuration to kubelet and crio service ...
   INFO Adding user's pull secret ...
   INFO Updating cluster ID ...
   INFO Starting OpenShift cluster ... [waiting 3m]
   INFO
   INFO To access the cluster, first set up your environment by following 'crc oc-env' instructions
   INFO Then you can access it by running 'oc login -u developer -p developer https://api.crc.testing:6443'
   INFO To login as an admin, run 'oc login -u kubeadmin -p (password) https://api.crc.testing:6443'
   INFO
   INFO You can now run 'crc console' and use these credentials to access the OpenShift web console
   Started the OpenShift cluster
   WARN The cluster might report a degraded or error state. This is expected since several operators have bee   n disabled to lower the resource usage. For more information, please consult the documentation
   ```
1. Run the following command .
   ```sh
   $ crc oc-env
   export PATH="/home/(user name)/.crc/bin:$PATH"
   # Run this command to configure your shell:
   # eval $(crc oc-env)
   $ eval $(crc oc-env)
   ```
1. Run the following command to access to CRC.
   - Log in as developer
     ```sh
     $ oc login -u developer -p developer https://api.crc.testing:6443
     ```
   - Log in as administrator
     ```sh
     $ oc login -u kubeadmin -p (password) https://api.crc.testing:6443
     ```
1. Now, you can run oc command ;-)
   ```sh
   $ oc version
   Client Version: 4.5.0-202004180718-6b061e3
   Server Version: 4.3.10 (If you log in as administrator, you can see it.)
   Kubernetes Version: v1.16.2
   ```
1. Log in the CRC VM wht SSH.
   ```sh
   $ crc ip
   192.168.130.11
   $ cd /home/(user name)/.crc/machines/crc
   $ ssh -i id_rsa -o StrictHostKeyChecking=no core@192.168.130.11
   Red Hat Enterprise Linux CoreOS 43.81.202003310153.0
     Part of OpenShift 4.3, RHCOS is a Kubernetes native operating system
     managed by the Machine Config Operator (`clusteroperator/machine-config`).
     
   WARNING: Direct SSH access to machines is not recommended; instead,
   make configuration changes via `machineconfig` objects:
     https://docs.openshift.com/container-platform/4.3/architecture/architecture-rhcos.html
   
   ---
   Last login: Sun May  3 07:40:11 2020 from 192.168.130.1
   [core@crc-bq5fv-master-0 ~]$
   ```
   1. Create a shell file.
      ```sh
      $ cd /etc/profile.d/
      $ sudo vi http_proxy.sh
      ```
      - http_proxy.sh
        ```sh
        export HTTP_PROXY=http://example.proxy.com:<port>
        export HTTPS_PROXY=http://example.proxy.com:<port>
        export NO_PROXY=127.0.0.1,192.168.130.11,.testing
        ```
1. Restart the CRC VM.
   ```sh
   $ crc stop
   $ crc start
   ```
1. Log in the CRC VM wht SSH and check if podman can access to Docker Hub.
   ```sh
   [core@crc-bq5fv-master-0 ~]$ podman search expresscluster
   INDEX       NAME                                           DESCRIPTION                                       STARS   OFFICIAL   AUTOMATED
   docker.io   docker.io/expresscluster/sss4mariadb           EXPRESSCLUSTER X SingleServerSafe container ...   0
   docker.io   docker.io/expresscluster/sss4postgres          EXPRESSCLUSTER X SingleServerSafe container ...   0
    :
   ```
## Link
- Archives
  - https://mirror.openshift.com/pub/openshift-v4/clients/crc/
