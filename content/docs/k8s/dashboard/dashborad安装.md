## NodePort部署

```shell
wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml
```

```shell
vim recommended.yaml
```

```yaml
kind: Service
apiVersion: v1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard
spec:
  type: NodePort    //添加类型为NodePort
  ports:
    - port: 443
      targetPort: 8443
      nodePort: 31010      //映射到宿主机的端口为31010
  selector:
    k8s-app: kubernetes-dashboard
```

```shell
//执行yaml文件
$ kubectl apply -f recommended.yaml 

//查看Pod是否运行
$ kubectl get pod -n kubernetes-dashboard 

//确保该yaml文件提供的pod都正常运行
$ kubectl get svc -n kubernetes-dashboard 
```

### 访问

创建dashboard的管理用户

```shell
$ kubectl create serviceaccount dashboard-admin -n kube-system
```

绑定用户为集群的管理员

```shell
$ kubectl create clusterrolebinding dashboard-cluster-rolebinding --clusterrole=cluster-admin --serviceaccount=kube-system:dashboard-admin
```

获取刚刚创建的用户Token

```shjavascript
kubectl get secrets -n kube-system | grep dashboard-admin
```

```javascript
kubectl describe secrets -n kube-system dashboard-admin-token-xxx
```

```yaml
eyJhbGciOiJSUzI1NiIsImtpZCI6InVWaHV5OVR4Zkl5bUFtWm1VSV9uWDR1aEhfSkdQQkFadjZPVi0wOVgyVGcifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkYXNoYm9hcmQtYWRtaW4tdG9rZW4tZmo5dG0iLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGFzaGJvYXJkLWFkbWluIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiOTdlNzc2ZDgtMTY3NS00ZjIxLTljMDItNTYxNWM3M2UzZTBlIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmRhc2hib2FyZC1hZG1pbiJ9.WySaF3V1tHckuK2iF6FNO-ZQeqr_8A-4hdB9DuWFE0apdXOAe1iql1YApOmGRgq4KtbLHEtcLD5hX2_xMb-Qz_gYHcFWdZn-XggAI45TWmSznwMzqQxxlTMnJIV2j7FSwynMOLX2kL6INHLamuT4VjX_R3bVGCrL9rapPLtzRXnf1_UH4ztv7F6N2ye8ALkgotmL-EQCTe_iexHg2DfHpfsjV7oLoByfGcZdFdHr-xNnoowfj-TbF82iX1MEgnFSChkvdaArIZhpQqFIwc_P8CKTcZd4LxejSdlG-_V3BGeyUOP0BIxw2M8rAXeCxljk-bdgECoOYQyQqksfu4EZiA
```

