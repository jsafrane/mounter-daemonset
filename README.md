This is an attempt to use a DaemonSet with NFS, Gluster and Ceph utilities
to mount volumes by Kubernetes on a system that does not have `mount.nfs`,
`mount.glusterfs` or `rbd` utilities.

## Usage
1. Get Kubernetes and deploy it somewhere with MountPropagation and MountContainers features enabled, e.g.:

   ```shell
   $ ALLOW_SECURITY_CONTEXT=true ALLOW_PRIVILEGED=true LOG_LEVEL=5 hack/local-up-cluster.sh
   ```

2. Deploy the daemon set in `kube-mount` namespace.
   ```
   $ kubectl create ns kube-mount
   $ kubectl create -f daemon.yaml
   ```

   This will spawn a privileged pod that has access to /var/lib/kubelet on
   each node that kubelet will use to mount stuff.

3. Profit, e.g. run a test that creates a Ceph server and a pod that uses CephFS mount:
   ```
   $ export KUBERNETES_PROVIDER=local
   $ go run hack/e2e.go  -- --test --test_args="--ginkgo.focus=CephFS"
   ```
