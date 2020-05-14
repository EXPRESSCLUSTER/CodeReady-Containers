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
   $ oc new-project test
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
1. Create ServiceAccount.
   ```sh
   $ oc create sa with-anyuid
   ```
1. Set anyuid as system:admin.
   ```sh
   $ oc adm policy add-scc-to-user anyuid -z with-anyuid --as system:admin
   ```
1. Refer to th following page to deploy SingleSeverSafe.
   - https://github.com/EXPRESSCLUSTER/kubernetes/blob/master/HowToDeploySSS.md
     - Use oc command instead of kubectl.
     - Add the following lines to run a pod with root account.
       ```yaml
        :
           spec:
             shareProcessNamespace: true
             restartPolicy: Always
             serviceAccount: with-anyuid
             serviceAccountName: with-anyuid
             containers:       
        :
       ```
     - There are sample yaml files on the above page to create three replicas. Please modify the following line and apply it.
       ```yaml
         replicas: 1
       ```
