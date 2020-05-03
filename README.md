# CodeReady Containers (CRC)
- The purpose of this project to open knowledge for CodeReady Containers (CRC) and EXPRESSCLUSTER.

## Setup CodeReady Containers with KVM
- The official documentation is [here](https://code-ready.github.io/crc/).

### Prerequisite
- You need Red Hat account to get the binary file of CRC. You can create an account for free.
- On the download page, copy pull secret in advance. The pull secret is needed to
- CRC needs about 9 GB RAM.
  ```sh
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
  23413 root      20   0 9390584   7.7g  13212 S 151.8 24.5 418:02.59 qemu-kvm
   :
  ```
  - For more detail, please refer to [Hardware requirements](https://code-ready.github.io/crc/#minimum-system-requirements-hardware_gsg).

### Evaluation Environment
- I have used KVM for evaluation.
  ```
  +----------------------------------------+
  | Host OS                                |
  | - CentOS Linux release 7.8.2003 (Core) |
  | +------------------------------------+ | 
  | | Guest OS                           | |
  | | - CRC                              | |
  | |   - crc version: 1.9.0+a68b5e0     | |
  | |   - OpenShift version: 4.3.10      | |
  | +------------------------------------+ |
  +----------------------------------------+
  ```

### Setup
1. Create a user and add it to the following groups.
   ```sh
   # usermod -aG libvirt <user name>
   # usermod -aG wheel <user name>
   ```
1. Switch to the sudoer user.
   ```sh
   # su <user name>
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
   INFO Adding user's pull secret ...
   INFO Updating cluster ID ...
   INFO Starting OpenShift cluster ... [waiting 3m]
   INFO
   INFO To access the cluster, first set up your environment by following 'crc oc-env' instructions
   INFO Then you can access it by running 'oc login -u developer -p developer https://api.crc.testing:6443'
   INFO To login as an admin, run 'oc login -u kubeadmin -p (password) https://api.crc.testing:6   443'
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
   $ export PATH="/home/(user name)/.crc/bin:$PATH"
   ```
1. Run the following command to access to CRC.
   - Log in as developer
     ```sh
     $ oc login -u developer -p developer https://api.crc.testing:6443
     ```
   - Log in as administrator
     ```sh
     $ oc login -u kubeadmin -p (password) https://api.crc.testing:6   443
     ```
1. Now, you can run oc command ;-)
   ```sh
   $ oc version
   Client Version: 4.5.0-202004180718-6b061e3
   Server Version: 4.3.10
   Kubernetes Version: v1.16.2
   ```
1. FIXME
   - Failed to pull image. The issue may be caused my proxy server :-(