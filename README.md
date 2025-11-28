# 🚀 NGINX Deployment on Amazon EKS

This project demonstrates how to deploy an **NGINX web server on Amazon EKS** using Kubernetes **Deployment** and **LoadBalancer Service**.

<img width="944" height="542" alt="image" src="https://github.com/user-attachments/assets/24241153-3236-47ed-b006-5a0c62b3e0f1" />


## ✅ Prerequisites

Before starting, make sure you have:

* AWS Account
* AWS CLI configured
* `eksctl` installed
* `kubectl` installed
* An EC2 key pair (optional, for node access)

---

## ✅ Step 1: Create EKS Cluster

```bash
eksctl create cluster \
  --name my-eks-cluster \
  --version 1.34 \
  --region us-east-1 \
  --nodegroup-name standard-workers \
  --node-type t3.small \
  --nodes 2 \
  --nodes-min 2 \
  --nodes-max 2 \
  --managed
```

Verify nodes:

```bash
kubectl get nodes
```

---

## ✅ Step 2: Deploy NGINX Application

Apply the already created NGINX deployment manifest:

```bash
kubectl apply -f nginx-deployment.yaml
```

Verify that the pods are running:

```bash
kubectl get pods
```

---

## ✅ Step 3: Expose NGINX Using LoadBalancer Service

Apply the existing NGINX service configuration:

```bash
kubectl apply -f nginx-service.yaml
```

Verify that the service is created and an external IP is assigned:

```bash
kubectl get services
```

---

## ✅ Step 4: Access NGINX in Browser

After a few minutes, AWS will assign an **External LoadBalancer URL**:

```bash
kubectl get svc nginx-service
```

Example output:

```
a9c72b19984a44243ae4db3fa823bd7e-373007027.us-east-1.elb.amazonaws.com
```

Open this in browser:

```
[http://a9c72b19984a44243ae4db3fa823bd7e-373007027.us-east-1.elb.amazonaws.com/]
```

✅ You should see the **"Welcome to nginx!"** page

---

## ✅ Architecture Overview

* 3 NGINX pods running in Kubernetes
* 1 AWS LoadBalancer automatically created
* 2 EC2 worker nodes
* Traffic flows:

  **Internet → AWS LoadBalancer → Kubernetes Service → NGINX Pods**

---

## ✅ Useful Debugging Commands

```bash
kubectl get pods
kubectl get services
kubectl describe svc nginx-service
kubectl logs <pod-name>
```


## ✅ Cleanup Resources (Very Important to Avoid Charges)

```bash
kubectl delete -f nginx-deployment.yaml
kubectl delete -f nginx-service.yaml
eksctl delete cluster --name my-eks-cluster --region us-east-1
```

---

## ✅ Project Output

* Successfully deployed NGINX on Amazon EKS
* Exposed using AWS LoadBalancer
* Verified using browser

✅ This project demonstrates a complete **EKS + Kubernetes + LoadBalancer + NGINX deployment workflow**.
