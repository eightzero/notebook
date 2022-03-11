# Metrics Server部署失败

## 问题现象

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

## 解决方式

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

# Mysql初始化CasbinRule表失败

## 问题现象

```shell
$ Error 1071: Specified key was too long; max key length is 3072 bytes
[14.968ms] [rows:0] CREATE TABLE `casbin_rules` (`id` bigint unsigned AUTO_INCREMENT,`created_at` datetime(3) NULL,`updated_at` datetime(3) NULL,`deleted_at` datetime(3) NULL,`ptype` varchar(512),`v0` varchar(512),`v1` varchar(512),`v2` varchar(512),PRIMARY KEY (`id`),INDEX idx_casbin_rules_deleted_at (`deleted_at`),UNIQUE INDEX unique_index (`ptype`,`v0`,`v1`,`v2`))
$ Error 1071: Specified key was too long; max key length is 3072 bytes

```

## 解决方式

使用官方的结构体 `adapter.CasbinRule`而不是自己声明的Casbin Model





# Swaggo Failed to load API definition 

## 问题现象

Swagger Failed to load API definition

Fetch error Internal Server Error doc.json

![image-20220217165902474](https://gitee.com/eightzero/pico/raw/master/image-20220217165902474.png)

## 解决方式

`main.go`中导入`swag init`生成的docs文件夹

```go
// main.go
import (
	_ "yourProject/docs"
)

// @title Swagger Example API
// @version 0.0.1
// @description This is a sample Server pets
// @securityDefinitions.apikey ApiKeyAuth
// @in header
// @name x-token
// @BasePath /
func main() {
	cmd.StartServer()
}
```



# docker build fail

## 现象

```sh
  => [builder 4/5] RUN go mod download                                                                              139.6s
 => ERROR [builder 5/5] RUN GOOS=linux GOARCH=amd64 go build .                                                      86.1s
------    
 > [builder 5/5] RUN GOOS=linux GOARCH=amd64 go build .:                                                                                                                                   
#12 72.17 cgo: C compiler "gcc" not found: exec: "gcc": executable file not found in $PATH
```

## 解决方式

`Dockerfile`中不要使用alpine版本编译

`FROM golang:1.17-alpine AS builder` --> `FROM golang:1.17 AS builder`

```dockerfile
RUN mkdir /lib64 && ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2
```

# Docker run mysql fail

## 现象

```sh
docker: Error response from daemon: driver failed programming external connectivity on endpoint mysql:  (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 3306 -j DNAT --to-destination 172.17.0.3:3306 ! -i docker0: iptables: No chain/target/match by that name.
 (exit status 1)).
```

## 解决方式

使用host模式

```sh
--net=host 
```

