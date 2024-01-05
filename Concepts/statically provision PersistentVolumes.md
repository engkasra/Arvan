- There are two ways PVs may be provisioned: statically or dynamically.

### Static
A cluster administrator creates a number of PVs. They carry the details of the real storage, which is available for use by cluster users. They exist in the Kubernetes API and are available for consumption.
### Dynamic
When none of the static PVs the administrator created match a user's PersistentVolumeClaim, the cluster may try to dynamically provision a volume specially for the PVC. For more information go to this **[page](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#provisioning)**
