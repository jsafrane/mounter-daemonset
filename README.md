This is an attempt to use a DaemonSet with NFS, Gluster and Ceph utilities
to mount volumes by Kubernetes on a system that does not have `mount.nfs`,
`mount.glusterfs` or `rbd` utilities.

## Usage
1. Get Kubernetes from PR #TBD, compile it and deploy it somewhere, e.g.:

   ```shell
   $ ALLOW_PRIVILEGED=true LOG_LEVEL=5 hack/local-up-cluster.sh
   ```

2. Deploy the daemon set in `kube-mount` namespace.
   ```
   $ kubectl create ns kube-mount
   $ kubectl create -f daemon.yaml
   ```

   This will spawn a privileged pod that has access to /var/lib/kubelet on
   each node that kubelet will use to mount stuff. Note labels of the pods,
   that's how kubelet finds out which pod to use for what volume plugin.

3. Profit, e.g. create a pod that uses nfs.
   TBD, run nfs/gluster/ceph e2e test?
