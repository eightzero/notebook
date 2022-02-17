
```go
global.DB.AutoMigrate(
   model.SysUser{},
   model.SvcTypes{},
   model.CasbinRule{}, ---> adapter.CasbinRule{},
   model.Machine{},
)
```

## yurtctl convert错误

```shell
[root@master ~]# yurtctl convert  --provider kubeadm --kubeadm-conf-path  /usr/lib/systemd/system/kubelet.service.d/10-kubeadm.conf --cloud-nodes master
I1201 02:01:31.538410   42272 convert.go:318] mark master as the cloud-node
I1201 02:02:51.677158   42272 util.go:543] servant job(yurtctl-disable-node-controller-master) has succeeded
I1201 02:02:51.677200   42272 convert.go:343] complete disabling node-controller
I1201 02:02:51.678398   42272 convert.go:443] kube-public/cluster-info configmap already exists, skip to prepare it
I1201 02:02:51.690122   42272 convert.go:408] deploying the yurt-hub and resetting the kubelet service on edge nodes...
E1201 02:04:51.694906   42272 util.go:540] fail to run servant job(yurtctl-servant-convert-node1): wait for job to be complete timeout
I1201 02:04:51.694954   42272 convert.go:414] complete deploying yurt-hub on edge nodes
I1201 02:04:51.694963   42272 convert.go:417] deploying the yurt-hub and resetting the kubelet service on cloud nodes
E1201 02:06:51.700340   42272 util.go:540] fail to run servant job(yurtctl-servant-convert-master): wait for job to be complete timeout
I1201 02:06:51.700441   42272 convert.go:423] complete deploying yurt-hub on cloud nodes
```

```shell
[root@master ~]# kubectl get pod -A
NAMESPACE      NAME                                              READY   STATUS      RESTARTS   AGE
kube-system    cilium-89zdb                                      1/1     Running     17         23d
kube-system    cilium-bt9zk                                      1/1     Running     41         23d
kube-system    cilium-operator-57b67c8d5f-rldrh                  1/1     Running     1          21h
kube-system    cilium-operator-57b67c8d5f-tdp8m                  1/1     Running     123        23d
kube-system    coredns-7f89b7bc75-t6vq7                          1/1     Running     1          21h
kube-system    coredns-7f89b7bc75-v259v                          1/1     Running     1          21h
kube-system    etcd-master                                       1/1     Running     1          21h
kube-system    kube-apiserver-master                             1/1     Running     1          21h
kube-system    kube-controller-manager-master                    1/1     Running     0          10m
kube-system    kube-scheduler-master                             1/1     Running     1          21h
kube-system    yurt-controller-manager-77b97fd47b-m2n6l          1/1     Running     0          11m
kube-system    yurt-hub-master                                   1/1     Running     0          8m25s
kube-system    yurt-hub-node1                                    1/1     Running     0          9m53s
kube-system    yurtctl-servant-convert-master-tz8qm              0/1     Completed   1          8m26s
kube-system    yurtctl-servant-convert-node1-mtrgk               0/1     Completed   2          10m
```





```shell
[root@master ~]# yurtctl revert --kubeadm-conf-path  /usr/lib/systemd/system/kubelet.service.d/10-kubeadm.conf
I1201 02:14:33.097932   44924 revert.go:172] yurt controller manager is removed
I1201 02:14:33.100463   44924 revert.go:182] serviceaccount for yurt controller manager is removed
I1201 02:14:33.102799   44924 revert.go:192] clusterrole for yurt controller manager is removed
I1201 02:14:33.106710   44924 revert.go:202] clusterrolebinding for yurt controller manager is removed
I1201 02:14:33.123802   44924 revert.go:353] deployment for yurt app manager is removed
I1201 02:14:33.124798   44924 revert.go:363] Role for yurt app manager is removed
I1201 02:14:33.125830   44924 revert.go:372] ClusterRole for yurt app manager is removed
I1201 02:14:33.129656   44924 revert.go:381] ClusterRoleBinding for yurt app manager is removed
I1201 02:14:33.130883   44924 revert.go:391] RoleBinding for yurt app manager is removed
I1201 02:14:33.275224   44924 revert.go:401] secret for yurt app manager is removed
I1201 02:14:33.474971   44924 revert.go:411] Service for yurt app manager is removed
I1201 02:14:33.476301   44924 revert.go:421] MutatingWebhookConfiguration for yurt app manager is removed
I1201 02:14:33.477440   44924 revert.go:431] ValidatingWebhookConfiguration for yurt app manager is removed
I1201 02:14:33.492314   44924 revert.go:440] crd for yurt app manager is removed
I1201 02:14:33.502202   44924 revert.go:449] UnitedDeploymentcrd for yurt app manager is removed
I1201 02:14:33.502231   44924 revert.go:221] yurt app manager is removed
I1201 02:15:23.523798   44924 util.go:543] servant job(yurtctl-enable-node-controller-master) has succeeded
I1201 02:15:23.523830   44924 revert.go:234] complete enabling node-controller
I1201 02:15:43.535988   44924 util.go:543] servant job(yurtctl-servant-revert-node1) has succeeded
I1201 02:15:43.536038   44924 revert.go:247] complete removing yurt-hub and resetting kubelet service on edge nodes
I1201 02:15:53.544282   44924 util.go:543] servant job(yurtctl-servant-revert-master) has succeeded
I1201 02:15:53.544320   44924 revert.go:255] complete removing yurt-hub and resetting kubelet service on cloud nodes
I1201 02:15:53.551972   44924 revert.go:263] delete yurthub clusterrole and clusterrolebinding
```

```shell
[root@master ~]# kubectl get pod -A
NAMESPACE      NAME                                              READY   STATUS      RESTARTS   AGE
awx            awx-demo-7df55fb78c-hr9mz                         4/4     Running     24         6d23h
awx            awx-demo-postgres-0                               1/1     Running     6          6d23h
awx            awx-operator-controller-manager-877886796-ljlvv   2/2     Running     28         7d
default        details-v1-66b6955995-67fhq                       2/2     Running     80         23d
default        httpbin-66cdbdb6c5-zqrd7                          2/2     Running     18         23d
default        productpage-v1-5d9b4c9849-nbjzp                   2/2     Running     79         23d
default        ratings-v1-fd78f799f-4rxhw                        2/2     Running     78         23d
default        reviews-v1-6549ddccc5-fctws                       2/2     Running     79         23d
default        reviews-v2-76c4865449-jdzr2                       2/2     Running     78         23d
default        reviews-v3-6b554c875-l7rj5                        2/2     Running     77         23d
istio-system   istio-egressgateway-797b55644-r8v47               1/1     Running     40         23d
istio-system   istio-ingressgateway-5ff88fbcc7-9bbdx             1/1     Running     40         23d
istio-system   istiod-8fd46755b-dww2b                            1/1     Running     40         23d
kube-system    cilium-89zdb                                      1/1     Running     17         23d
kube-system    cilium-bt9zk                                      1/1     Running     41         23d
kube-system    cilium-operator-57b67c8d5f-rldrh                  1/1     Running     1          21h
kube-system    cilium-operator-57b67c8d5f-tdp8m                  1/1     Running     123        23d
kube-system    coredns-7f89b7bc75-t6vq7                          1/1     Running     1          21h
kube-system    coredns-7f89b7bc75-v259v                          1/1     Running     1          21h
kube-system    etcd-master                                       1/1     Running     1          21h
kube-system    hubble-relay-6fcf45468f-pls4w                     1/1     Running     277        23d
kube-system    hubble-ui-7cd68fdf6d-c25b7                        3/3     Running     119        23d
kube-system    kube-apiserver-master                             1/1     Running     1          21h
kube-system    kube-controller-manager-master                    1/1     Running     0          2m20s
kube-system    kube-scheduler-master                             0/1     Running     1          21h
kube-system    yurtctl-servant-convert-master-tz8qm              0/1     Completed   1          12m
kube-system    yurtctl-servant-convert-node1-mtrgk               0/1     Completed   2          14m
```

```shell
[root@master ~]# kubectl get nodes
NAME     STATUS   ROLES                  AGE   VERSION
master   Ready    control-plane,master   23d   v1.20.13
node1    Ready    <none>                 23d   v1.20.13
```





## VCluster

```shell
[root@master ~]# kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/cluster-api-provider-nested/master/virtualcluster/config/sampleswithspec/clusterversion_v1_nodeport.yaml
clusterversion.tenancy.x-k8s.io/cv-sample-np created
[root@master ~]# kubectl vc create -f https://raw.githubusercontent.com/kubernetes-sigs/cluster-api-provider-nested/master/virtualcluster/config/sampleswithspec/virtualcluster_1_nodeport.yaml -o vc-1.kubeconfig

2021/12/05 23:56:04 etcd is ready
cannot find sts/apiserver in ns default-56325d-vc-sample-1: default-56325d-vc-sample-1/apiserver is not ready in 120 seconds
```



```
[root@master ~]# kubectl get pod -ndefault-56325d-vc-sample-1
NAME          READY   STATUS              RESTARTS   AGE
apiserver-0   0/1     ContainerCreating   0          3m16s
etcd-0        1/1     Running             0          4m6s
```

```
[root@master ~]# kubectl describe pod -ndefault-56325d-vc-sample-1 apiserver-0
Name:           apiserver-0
Namespace:      default-56325d-vc-sample-1
Priority:       0
Node:           node1/192.168.92.135
Start Time:     Sun, 05 Dec 2021 23:56:03 -0500
Labels:         component-name=apiserver
                controller-revision-hash=apiserver-77b4b4dfbf
                statefulset.kubernetes.io/pod-name=apiserver-0
Annotations:    <none>
Status:         Pending
IP:
IPs:            <none>
Controlled By:  StatefulSet/apiserver
Containers:
  apiserver:
    Container ID:
    Image:         virtualcluster/apiserver-v1.16.2
    Image ID:
    Port:          6443/TCP
    Host Port:     0/TCP
    Command:
      kube-apiserver
    Args:
      --bind-address=0.0.0.0
      --allow-privileged=true
      --anonymous-auth=true
      --client-ca-file=/etc/kubernetes/pki/root/tls.crt
      --tls-cert-file=/etc/kubernetes/pki/apiserver/tls.crt
      --tls-private-key-file=/etc/kubernetes/pki/apiserver/tls.key
      --kubelet-https=true
      --kubelet-client-certificate=/etc/kubernetes/pki/apiserver/tls.crt
      --kubelet-client-key=/etc/kubernetes/pki/apiserver/tls.key
      --enable-bootstrap-token-auth=true
      --etcd-servers=https://etcd-0.etcd:2379
      --etcd-cafile=/etc/kubernetes/pki/root/tls.crt
      --etcd-certfile=/etc/kubernetes/pki/apiserver/tls.crt
      --etcd-keyfile=/etc/kubernetes/pki/apiserver/tls.key
      --service-account-key-file=/etc/kubernetes/pki/service-account/tls.key
      --service-cluster-ip-range=10.32.0.0/16
      --service-node-port-range=30000-32767
      --authorization-mode=Node,RBAC
      --runtime-config=api/all
      --enable-admission-plugins=NamespaceLifecycle,NodeRestriction,LimitRanger,ServiceAccount,DefaultStorageClass,ResourceQuota
      --apiserver-count=1
      --endpoint-reconciler-type=master-count
      --enable-aggregator-routing=true
      --requestheader-client-ca-file=/etc/kubernetes/pki/root/tls.crt
      --requestheader-allowed-names=front-proxy-client
      --requestheader-username-headers=X-Remote-User
      --requestheader-group-headers=X-Remote-Group
      --requestheader-extra-headers-prefix=X-Remote-Extra-
      --proxy-client-key-file=/etc/kubernetes/pki/frontproxy/tls.key
      --proxy-client-cert-file=/etc/kubernetes/pki/frontproxy/tls.crt
      --v=2
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Liveness:       tcp-socket :6443 delay=15s timeout=15s period=10s #success=1 #failure=8
    Readiness:      http-get https://:6443/healthz delay=5s timeout=30s period=2s #success=1 #failure=8
    Environment:    <none>
    Mounts:
      /etc/kubernetes/pki/apiserver from apiserver-ca (ro)
      /etc/kubernetes/pki/frontproxy from front-proxy-ca (ro)
      /etc/kubernetes/pki/root from root-ca (ro)
      /etc/kubernetes/pki/service-account from serviceaccount-rsa (ro)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-8xcwl (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             False
  ContainersReady   False
  PodScheduled      True
Volumes:
  apiserver-ca:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  apiserver-ca
    Optional:    false
  root-ca:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  root-ca
    Optional:    false
  front-proxy-ca:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  front-proxy-ca
    Optional:    false
  serviceaccount-rsa:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  serviceaccount-rsa
    Optional:    false
  default-token-8xcwl:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-8xcwl
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason       Age                  From               Message
  ----     ------       ----                 ----               -------
  Normal   Scheduled    3m29s                default-scheduler  Successfully assigned default-56325d-vc-sample-1/apiserver-0 to node1
  Warning  FailedMount  86s                  kubelet            Unable to attach or mount volumes: unmounted volumes=[front-proxy-ca], unattached volumes=[default-token-8xcwl apiserver-ca front-proxy-ca root-ca serviceaccount-rsa]: timed out waiting for the condition
  Warning  FailedMount  81s (x9 over 3m29s)  kubelet            MountVolume.SetUp failed for volume "front-proxy-ca" : secret "front-proxy-ca" not found
```



```shell
[root@master ~]# kubectl get all -n vc-manager
NAME                              READY   STATUS    RESTARTS   AGE
pod/vc-manager-76c5878465-25s5m   1/1     Running   0          67m
pod/vc-syncer-55c5bc5898-47vzz    1/1     Running   0          67m
pod/vn-agent-t7wg9                1/1     Running   0          67m

NAME                                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/virtualcluster-webhook-service   ClusterIP   10.10.180.101   <none>        9443/TCP   65m

NAME                      DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
daemonset.apps/vn-agent   1         1         1       1            1           <none>          67m

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/vc-manager   1/1     1            1           67m
deployment.apps/vc-syncer    1/1     1            1           67m

NAME                                    DESIRED   CURRENT   READY   AGE
replicaset.apps/vc-manager-76c5878465   1         1         1       67m
replicaset.apps/vc-syncer-55c5bc5898    1         1         1       67m
```

## Karmada

```sh
[root@alicloud-node karmada]# hack/local-up-karmada.sh
kind not exists, will install kind v0.11.1
Installing 'kind v0.11.1' for you
which: no kind in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/lib/golang/bin:/root/bin)
Installing 'kubectl v1.18.0' for you
Preparing kindClusterConfig in path: /tmp/tmp.nKvTwSYbZE
Creating cluster karmada-host
Creating cluster member1
Creating cluster member2
Creating cluster member3
make: Entering directory '/root/Develop/gopath/src/karmada'
CGO_ENABLED=0 GOOS=linux go build \
	-ldflags "-X github.com/karmada-io/karmada/pkg/version.gitVersion=v0.9.0-115-g1f87ca95 -X github.com/karmada-io/karmada/pkg/version.gitCommit=1f87ca9519957e76fccb660079bfc44d58a09286 -X github.com/karmada-io/karmada/pkg/version.gitTreeState="clean" -X github.com/karmada-io/karmada/pkg/version.buildDate=2021-12-24T07:38:15Z" \
	-o karmada-controller-manager \
	cmd/controller-manager/controller-manager.go
VERSION=latest hack/docker.sh karmada-controller-manager
Sending build context to Docker daemon  160.8MB
Step 1/4 : FROM alpine:3.7
3.7: Pulling from library/alpine
5d20c808ce19: Pull complete
Digest: sha256:8421d9a84432575381bfabd248f1eb56f3aa21d9d7cd2511583c68c9b7511d10
Status: Downloaded newer image for alpine:3.7
 ---> 6d1ef012b567
Step 2/4 : RUN apk add --no-cache ca-certificates
 ---> Running in 21581f969cf1
fetch http://dl-cdn.alpinelinux.org/alpine/v3.7/main/x86_64/APKINDEX.tar.gz
WARNING: Ignoring http://dl-cdn.alpinelinux.org/alpine/v3.7/main/x86_64/APKINDEX.tar.gz: operation timed out
fetch http://dl-cdn.alpinelinux.org/alpine/v3.7/community/x86_64/APKINDEX.tar.gz
ERROR: unsatisfiable constraints:
  ca-certificates (missing):
    required by: world[ca-certificates]
The command '/bin/sh -c apk add --no-cache ca-certificates' returned a non-zero code: 1
make: *** [Makefile:104: image-karmada-controller-manager] Error 1
make: Leaving directory '/root/Develop/gopath/src/karmada'
```





```sh
[root@alicloud-node karmada]# hack/local-up-karmada.sh
kind not exists, will install kind v0.11.1
Installing 'kind v0.11.1' for you
which: no kind in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/lib/golang/bin:/root/bin)
Installing 'kubectl v1.18.0' for you
Preparing kindClusterConfig in path: /tmp/tmp.axBYDaONXq
Creating cluster karmada-host
Creating cluster member1
Creating cluster member2
Creating cluster member3
make: Entering directory '/root/Develop/gopath/src/karmada'
VERSION=latest hack/docker.sh karmada-controller-manager
Sending build context to Docker daemon  399.8MB
Step 1/4 : FROM alpine:3.7
 ---> 6d1ef012b567
Step 2/4 : RUN apk add --no-cache ca-certificates
 ---> Using cache
 ---> 987b2c96f3e3
Step 3/4 : ADD karmada-controller-manager /bin/
 ---> Using cache
 ---> 4c8132a35cf3
Step 4/4 : CMD ["/bin/karmada-controller-manager"]
 ---> Using cache
 ---> 50d6149c965b
Successfully built 50d6149c965b
Successfully tagged swr.ap-southeast-1.myhuaweicloud.com/karmada/karmada-controller-manager:latest
VERSION=latest hack/docker.sh karmada-scheduler
Sending build context to Docker daemon  399.8MB
Step 1/4 : FROM alpine:3.7
 ---> 6d1ef012b567
Step 2/4 : RUN apk add --no-cache ca-certificates
 ---> Using cache
 ---> 987b2c96f3e3
Step 3/4 : ADD karmada-scheduler /bin/
 ---> Using cache
 ---> fe3334c67cb4
Step 4/4 : CMD ["/bin/karmada-scheduler"]
 ---> Using cache
 ---> 47c9db1b4870
Successfully built 47c9db1b4870
Successfully tagged swr.ap-southeast-1.myhuaweicloud.com/karmada/karmada-scheduler:latest
VERSION=latest hack/docker.sh karmada-webhook
Sending build context to Docker daemon  399.8MB
Step 1/4 : FROM alpine:3.7
 ---> 6d1ef012b567
Step 2/4 : RUN apk add --no-cache ca-certificates
 ---> Using cache
 ---> 987b2c96f3e3
Step 3/4 : ADD karmada-webhook /bin/
 ---> Using cache
 ---> 836cbce55275
Step 4/4 : CMD ["/bin/karmada-webhook"]
 ---> Using cache
 ---> baa6f4c411c2
Successfully built baa6f4c411c2
Successfully tagged swr.ap-southeast-1.myhuaweicloud.com/karmada/karmada-webhook:latest
VERSION=latest hack/docker.sh karmada-agent
Sending build context to Docker daemon  399.8MB
Step 1/4 : FROM alpine:3.7
 ---> 6d1ef012b567
Step 2/4 : RUN apk add --no-cache ca-certificates
 ---> Using cache
 ---> 987b2c96f3e3
Step 3/4 : ADD karmada-agent /bin/
 ---> Using cache
 ---> 857b355f9fab
Step 4/4 : CMD ["/bin/karmada-agent"]
 ---> Using cache
 ---> 1686da0562fc
Successfully built 1686da0562fc
Successfully tagged swr.ap-southeast-1.myhuaweicloud.com/karmada/karmada-agent:latest
VERSION=latest hack/docker.sh karmada-scheduler-estimator
Sending build context to Docker daemon  399.8MB
Step 1/4 : FROM alpine:3.7
 ---> 6d1ef012b567
Step 2/4 : RUN apk add --no-cache ca-certificates
 ---> Using cache
 ---> 987b2c96f3e3
Step 3/4 : ADD karmada-scheduler-estimator /bin/
 ---> Using cache
 ---> aea9856d3e77
Step 4/4 : CMD ["/bin/karmada-scheduler-estimator"]
 ---> Using cache
 ---> af44d54da19a
Successfully built af44d54da19a
Successfully tagged swr.ap-southeast-1.myhuaweicloud.com/karmada/karmada-scheduler-estimator:latest
make: Leaving directory '/root/Develop/gopath/src/karmada'
Waiting for the host clusters to be ready...
Waiting for kubeconfig file /root/.kube/karmada.config and clusters karmada-host to be ready...
```

