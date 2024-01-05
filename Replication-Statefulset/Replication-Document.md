## Before you begin 
- You need to have a Kubernetes cluster, and the kubectl command-line tool must be configured to communicate with your cluster.
you can create and initialize your cluster with **[kubeadm](https://github.com/engkasra/Arvan/blob/main/Concepts/Initialize%20cluster-kubernetes.md)**.
- You need to have a default **[StorageClass](https://github.com/engkasra/Arvan/blob/main/Concepts/Storage%20class.md)**, or **[statically provision PersistentVolumes](https://github.com/engkasra/Arvan/blob/main/Concepts/statically%20provision%20PersistentVolumes.md)** yourself to satisfy the **[PersistentVolumeClaims](https://github.com/engkasra/Arvan/blob/main/Concepts/Persistent%20volume%20claims.md)** used here.
- This tutorial assumes you are familiar with PersistentVolumes and StatefulSets, as well as other core concepts like Pods, Services, and ConfigMaps.

## Objectives
- Deploy a replicated MySQL topology with a StatefulSet.
- Send MySQL client traffic.
- Observe resistance to downtime.
- Scale the StatefulSet up and down.
## Deploy MySQL
The example MySQL deployment consists of a ConfigMap, two Services, and a StatefulSet.
### Create a ConfigMap
Create the ConfigMap from the following YAML configuration file:
```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql
  labels:
    app: mysql
    app.kubernetes.io/name: mysql
data:
  primary.cnf: |
    # Apply this config only on the primary.
    [mysqld]
    log-bin    
  replica.cnf: |
    # Apply this config only on replicas.
    [mysqld]
    super-read-only    
```
This ConfigMap provides my.cnf overrides that let you independently control configuration on the primary MySQL server and its replicas. In this case, you want the primary server to be able to serve replication logs to replicas and you want replicas to reject any writes that don't come via replication.

There's nothing special about the ConfigMap itself that causes different portions to apply to different Pods. Each Pod decides which portion to look at as it's initializing, based on information provided by the StatefulSet controller.

### Create Services
Create the Services from the following YAML configuration file:
```yml
# Headless service for stable DNS entries of StatefulSet members.
apiVersion: v1
kind: Service
metadata:
  name: mysql
  labels:
    app: mysql
    app.kubernetes.io/name: mysql
spec:
  ports:
  - name: mysql
    port: 3306
  clusterIP: None
  selector:
    app: mysql
---
# Client service for connecting to any MySQL instance for reads.
# For writes, you must instead connect to the primary: mysql-0.mysql.
apiVersion: v1
kind: Service
metadata:
  name: mysql-read
  labels:
    app: mysql
    app.kubernetes.io/name: mysql
    readonly: "true"
spec:
  ports:
  - name: mysql
    port: 3306
  selector:
    app: mysql
```
