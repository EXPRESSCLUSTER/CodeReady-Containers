# Deploy Applications
- [Prerequisite](#prerequisite)
- [Hello OpenShift](#hello-openshift)
- [SingleServerSafe](#singleserversafe)

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
### SingleServerSafe 
1. Log in CRC with kubeadmin user.
   ```sh
   $ oc login -u kubeadmin -p (password) https://api.crc.testing:6443
   ```
1. Create a project to run SingleServerSafe container with root account.
   ```sh
   $ oc project root-test
   ```
1. Set anyuid to the project.
   ```sh
   $ oc adm policy add-scc-to-user anyuid -z default -n root-test
   ```
1. Log in CRC with developer user.
   ```sh
   $ oc login -u developer -p developer https://api.crc.testing:6443
   ```
1. Refer to th following page to deploy SingleSeverSafe.
   - https://github.com/EXPRESSCLUSTER/kubernetes/blob/master/HowToDeploySSS.md
     - There are sample yaml files on the above page to create three replicas. Please modify the following line and apply it.
       ```yaml
         replicas: 1
       ```