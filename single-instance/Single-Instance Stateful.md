This page shows you how to run a single-instance stateful application in Kubernetes using a PersistentVolume and a Deployment.

# Objectives
- Create a PersistentVolume referencing a disk in your environment.
- Create a MySQL Deployment.
- Expose MySQL to other pods in the cluster at a known DNS name.

# Deploy MySQL
You can run a stateful application by creating a Kubernetes Deployment and connecting it to an existing PersistentVolume using a PersistentVolumeClaim.
For example, this YAML file describes a Deployment that runs MySQL and references the PersistentVolumeClaim. The file defines a volume mount for /var/lib/mysql, and then creates a PersistentVolumeClaim that looks for a 20G volume. This claim is satisfied by any existing volume that meets the requirements, or by a dynamic provisioned.

Note: The password is defined in the config yaml, and this is insecure. See Kubernetes **[Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)** for a secure solution.

### Ref:https://kubernetes.io/docs/tasks/run-application/run-single-instance-stateful-application/
