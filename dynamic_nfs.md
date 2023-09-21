# Dynamic NFS Provisioning in Kubernetes Lab Guide

---

## Part 1: Introduction and Initial Setup

**Objective:**  
Understand the basics of dynamic NFS provisioning and prepare a Kubernetes cluster for the lab.

### Prerequisites:
- A fresh Kubernetes cluster
- An existing NFS server

### Steps:

1. **Start Kubernetes Cluster:**  
   - Command: `minikube start`

2. **Understand Manual Provisioning:**  
   - Note: Manual provisioning is time-consuming and prone to errors.

3. **Why Dynamic NFS Provisioning:**  
   - Note: Dynamic provisioning automates volume creation, making the process more efficient.

4. **NFS Server Setup:**  
   - Note: An NFS server must be set up and accessible by the Kubernetes cluster.

5. **Deploy NFS Client Provisioner:**  
   - Command: `kubectl apply -f nfs-client-provisioner.yaml`

6. **Kubernetes Role Setup:**  
   - Service Account: `kubectl create sa nfs-client-provisioner`
   - Role: `kubectl create role nfs-role --verb=get,list,watch --resource=pods`
   - Role Binding: `kubectl create rolebinding nfs-rolebinding --role=nfs-role --serviceaccount=default:nfs-client-provisioner`

---

## Part 2: Kubernetes Object Configuration

**Objective:**  
Configure the necessary Kubernetes objects for dynamic NFS provisioning.

### Steps:

1. **Service Account Creation:**  
   - Command: `kubectl create sa nfs-client-provisioner`

2. **Cluster Role and Binding:**  
   - Cluster Role: `kubectl create clusterrole nfs-clusterrole --verb=get,list,watch,create,delete --resource=persistentvolumes`
   - Cluster Role Binding: `kubectl create clusterrolebinding nfs-clusterrolebinding --clusterrole=nfs-clusterrole --serviceaccount=default:nfs-client-provisioner`

3. **Storage Class Deployment:**  
   - Command: `kubectl apply -f storage-class.yaml`

4. **Verify Setup:**  
   - Command: `kubectl get all`

---

## Part 3: Provisioning and Consuming Volumes

**Objective:**  
Demonstrate the process of dynamically provisioning and consuming NFS volumes in Kubernetes.

### Steps:

1. **Persistent Volume Claim (PVC):**  
   - Command: `kubectl apply -f pvc1.yaml`

2. **Directory Verification:**  
   - Command: `ls /path/to/nfs/volume`

3. **Deploy Test Pod (Busybox):**  
   - Command: `kubectl apply -f busybox.yaml`

4. **Interact with Pod:**  
   - Command: `kubectl exec -it busybox -- /bin/sh`

5. **Cleanup and Reclaim Policy:**  
   - Delete PVC: `kubectl delete pvc pvc1`
   - Delete PV: `kubectl delete pv pv1`

---

By following this lab guide, you should gain a comprehensive understanding of how to implement dynamic NFS provisioning in Kubernetes.
