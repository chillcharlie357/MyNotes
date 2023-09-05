# Service

提供访问集群内部pods的接入点。

## port和targetPort

- port：service用这个端口将流量路由到其负责的 pod。

- targetPort：pod上的端口，负责处理从service过来的流量。


```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - name: http
      port: 80
      protocol: TCP
      targetPort: 8080
```
targetPort 字段将流量路由到Service的 pod 上的 8080 端口。
port字段将传入流量导向service的 80 端口。


- 区别：port用于监听来自外部客户端的传入流量，而 targetPort 是服务与负责处理该流量的 pod 之间的内部通信端口。

