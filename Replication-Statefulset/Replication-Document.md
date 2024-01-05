## Before you begin 
- You need to have a Kubernetes cluster, and the kubectl command-line tool must be configured to communicate with your cluster.
you can create and initialize your cluster with **[kubeadm](https://github.com/engkasra/Arvan/blob/main/Concepts/Initialize%20cluster-kubernetes.md)**.
- You need to have a default **[StorageClass](https://github.com/engkasra/Arvan/blob/main/Concepts/Storage%20class.md)**, or **[statically provision PersistentVolumes](https://github.com/engkasra/Arvan/blob/main/Concepts/statically%20provision%20PersistentVolumes.md)** yourself to satisfy the **[PersistentVolumeClaims](https://github.com/engkasra/Arvan/blob/main/Concepts/Persistent%20volume%20claims.md)** used here.
- This tutorial assumes you are familiar with PersistentVolumes and StatefulSets, as well as other core concepts like Pods, Services, and ConfigMaps.
