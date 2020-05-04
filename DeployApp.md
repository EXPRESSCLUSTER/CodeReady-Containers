# Deploy Applications
- [Prerequisite](#prerequisite)

## Prerequisite
1. Log in CRC.
   - Log in as developer
     ```sh
     $ oc login -u developer -p developer https://api.crc.testing:6443
     ```
   - Log in as administrator
     ```sh
     $ oc login -u kubeadmin -p (password) https://api.crc.testing:6443
     ```
1. Create a project.
   ```sh
   $ oc project test
   ```
## Hello OpenShift 
1. Dowonload [hello-pod.json](https://github.com/openshift/origin/blob/master/examples/hello-openshift/hello-pod.json) file.
1. Apply the manifest file.
   ```sh
   $ oc apply -f examples/hello-openshift/hello-pod.json
   ```
1. Check IP address of the pod.
   ```sh
   $ oc get pod hello-openshift -o yaml |grep IP
   hostIP: 192.168.130.11
   podIP: 10.128.0.52
   podIPs:
   ```
1. Log in the CRC VM with SSH.
   ```sh
   $ crc ip
   192.168.130.11
   $ cd /home/(user name)/.crc/machines/crc
   $ ssh -i id_rsa -o StrictHostKeyChecking=no core@192.168.130.11
   ```
1. Run curl command.
   ```sh
   $ curl 10.128.0.52:8080
   Hello OpenShift!
   ```
