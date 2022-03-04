# helm-chart

helm的chart仓库地址为：https://wayne-beep.github.io/helm-repo/

## 本Chart仓库的使用方法
1、添加chart仓库

`# helm repo add myrepo https://wayne-beep.github.io/helm-repo`

2、添加成功

```shell
# helm repo list
NAME  	URL                                   
myrepo	https://wayne-beep.github.io/helm-repo/
```

3、搜索chart包

```shell
# helm search repo
NAME                              	CHART VERSION	APP VERSION	DESCRIPTION                                   
myrepo/edgex                      	0.1.0        	1.0        	A Helm chart for EdgeX Geneva                 
myrepo/kubernetes-dashboard       	2.3.0        	2.0.3      	General-purpose web UI for Kubernetes clusters
myrepo/test                       	0.1.0        	1.16.0     	A Helm chart for Kubernetes 
```

4、安装chart包

# helm install xxx myrepo/test
xxx为relaese名字

