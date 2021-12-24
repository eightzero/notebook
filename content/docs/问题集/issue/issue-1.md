问题1：Metrics Server部署失败

```shell
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  14m                   default-scheduler  Successfully assigned kube-system/metrics-server-7898cc74cb-jdwps to master
  Normal   Pulling    14m                   kubelet            Pulling image "k8s.gcr.io/metrics-server/metrics-server:v0.5.2"
  Normal   Pulled     13m                   kubelet            Successfully pulled image "k8s.gcr.io/metrics-server/metrics-server:v0.5.2" in 16.557781304s
  Normal   Created    13m                   kubelet            Created container metrics-server
  Normal   Started    13m                   kubelet            Started container metrics-server
  Warning  Unhealthy  3m54s (x57 over 13m)  kubelet            Readiness probe failed: HTTP probe failed with statuscode: 500
```



修改components.yaml, 添加`--kubelet-insecure-tls`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    k8s-app: metrics-server
  name: metrics-server
  namespace: kube-system
spec:
  selector:
    matchLabels:
      k8s-app: metrics-server
  strategy:
    rollingUpdate:
      maxUnavailable: 0
  template:
    metadata:
      labels:
        k8s-app: metrics-server
    spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        - --kubelet-insecure-tls
```

