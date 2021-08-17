
helm 支持安装多集群, 官方只给了一个用 [istiod 部署的 task](https://istio.io/latest/docs/setup/install/multicluster/primary-remote_multi-network/#configure-cluster2-as-a-remote)

steps:
---
- [生成证书](https://istio.io/latest/docs/tasks/security/cert-management/plugin-ca-cert/)
- 将证书填入到对应 cluster1.yaml 和 cluster2.yaml 中. 可以用 base64 处理下.
- 切换到 primary 集群环境执行命令：`helm install istio-operator manifests/istio-operator -f example/multiCluster/cluster1.yaml`
- 执行命令 `kubectl get svc istio-eastwestgateway -n istio-system` 将这个 svc 暴露出来，可以使用 externalIPs 字段，获取到 svc 对外暴露的 IP. 
- 将上述 IP 填入 cluster2.yaml 中的 remotePilot 字段中,并把环境切换到从集群中安装 cluster2. `helm install istio-operator manifests/istio-operator -f example/multiCluster/cluster1.yaml`
- 按照第二步暴露 svc.
