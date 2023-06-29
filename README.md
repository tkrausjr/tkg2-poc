# tkg2-poc



## SETUP A CLUSTER and deploy TEST Application
1. kubectl vsphere login --server 192.168.199.2 --tanzu-kubernetes-cluster-namespace application-a --vsphere-username administrator@vsphere.local --insecure-skip-tls-verify 
2. cd github/tkg2-poc/clusters/
3. kubectl apply -f ./gc-small-classy.yaml
2. kubectl vsphere login --server 192.168.199.2 --tanzu-kubernetes-cluster-namespace application-a --tanzu-kubernetes-cluster-name <CLUSTER-NAME> --vsphere-username administrator@vsphere.local --insecure-skip-tls-verify
3. Configure workable PSP - # Reference - https://docs.vmware.com/en/VMware-vSphere/8.0/vsphere-with-tanzu-tkg/GUID-0AEC33DE-DCE2-4FBE-A33F-73C4EDCCAB88.html#GUID-0AEC33DE-DCE2-4FBE-A33F-73C4EDCCAB88
    a. Create RB to run Privileged Workloads in default Namespace - 
        1. kubectl create rolebinding rolebinding-default-privileged-sa-ns_default --namespace=default --clusterrole=psp:vmware-system-privileged --group=system:serviceaccounts

    b. Create CRB to run Privileged in ANY namespace - 
        1. kubectl create clusterrolebinding psp:authenticated --clusterrole=psp:vmware-system-restricted --group=system:authenticated
4. cd ../apps/guestbook-sc-lb
3.  kubectl apply -f guestbook-all-in-one.yaml
6.  watch kubectl get pods -o wide
7.  kubectl get sc
8.  kubectl get pvc
9.  kubectl get pv
10.  kubectl describe sc thin-disk1
11. In vCenter goto Datastore and see the two dynamically provisioned VMDK's.
    1. nfs-ubuntu-01 —> kubevols —> Refresh
12.  watch kubectl get pods -o wide
13.  kubectl get services
    1. NAME           TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
    2. frontend       LoadBalancer   10.100.200.46    10.40.14.39   80:32324/TCP   3m
14.  Chrome - Access and test the Application - using Chrome
    1. Chrome http://10.40.13.39  
    2. Enter some text into the application which will be stored in the REDIS DB on Persistent Volumes.
    3. Show the Load Balancer that was created and the Virtual Servers etc.
15. kubectl get po -o wide
    1. redis-master-89d7df6bf-6wrlb   1/1       Running   0          3m
    2. redis-slave-7cf6774dbb-srdxg   1/1       Running   0          3m
16. kubectl delete po redis-master-89d7df6bf-6wrlb  redis-slave-7cf6774dbb-srdxg
17. watch kubectl get po -o wide
    1. Pods are recreated by the DEPLOYMENT automatically and should reconnect to the same Persistent Volumes
18.  Chrome - TEST !!! - Access the Application again - It should have the same text you entered.
    

