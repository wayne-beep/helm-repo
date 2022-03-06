# 使用 GitHub 作为 Helm 仓库
使用 GitHub 作为 Helm 的仓库；在创建前需要按照 Helm（Helm3）

## 准备工作

- 在 GitHub 上创建名为 helm-chart的仓库`git@github.com:wayne-beep/helm-repo.git`
- clone 仓库到本地 `git clone git@github.com:wayne-beep/helm-repo.git`
- 进入仓库目录，并执行以下命令创建 Helm 包
	```shell
	$ mkdir fantastic-charts
	helm create fantastic-charts/helloworld
	```
	此时，已经在 fantastic-charts 目录下创建出了 helloworld 这个包的配置文件
```shell
$ tree fantastic-charts
fantastic-charts
└── helloworld
    ├── Chart.yaml
    ├── charts
    ├── templates
    │   ├── NOTES.txt
    │   ├── _helpers.tpl
    │   ├── deployment.yaml
    │   ├── hpa.yaml
    │   ├── ingress.yaml
    │   ├── service.yaml
    │   ├── serviceaccount.yaml
    │   └── tests
    │       └── test-connection.yaml
    └── values.yaml
4 directories, 10 files
```

检查配置

```shell
helm lint fantastic-charts/*
==> Linting fantastic-charts/helloworld
[INFO] Chart.yaml: icon is recommended

1 chart(s) linted, 0 chart(s) failed
```

## 打包发布应用

### 打包应用
`$ helm package fantastic-charts/*`

### 添加描述文件 index.yaml
`helm repo index --url https://github.com/wayne-beep/helm-repo .`
对应的 url 即为 repo 的 url

```shell
cat index.yaml
apiVersion: v1
entries:
  helloworld:
  - apiVersion: v2
    appVersion: 1.16.0
    created: "2022-03-04T18:03:32.243429+08:00"
    description: A Helm chart for Kubernetes
    digest: ba76f60a124566b05918a224ecf1df6583c00b44a3d7a5cbbe0f6b2cbbe674ad
    name: helloworld
    type: application
    urls:
    - https://wayne-beep.github.io/helm-repo/helloworld-0.1.0.tgz
    version: 0.1.0
generated: "2022-03-04T18:03:32.241635+08:00
```

### 提交并推送到仓库中

### 配置仓库开启 GitHub Pages

### 客户端添加安装
添加仓库到 Helm 客户端
`$ helm repo add testrepo https://wayne-beep.github.io/helm-repo/`

### 查找应用
```shell
helm search repo testrepo
NAME               	CHART VERSION	APP VERSION	DESCRIPTION
testrepo/helloworld	0.1.0        	1.16.0     	A Helm chart for Kubernetes
```

### 安装应用
`$ helm install helloworld testrepo/helloworld`

### 升级 Helm 版本
修改版本号为 0.1.1
`$ vim fantastic-charts/helloworld/Chart.yaml`

打包
`$ helm package fantastic-charts/*`
修改描述文件 index.yaml
`$ helm repo index  --url https://wayne-beep.github.io/helm-repo/ --merge index.yaml  .`

此时 index.yaml 内容变为
```shell
cat index.yaml
apiVersion: v1
entries:
  helloworld:
  - apiVersion: v2
    appVersion: 1.16.0
    created: "2022-03-06T09:14:23.011386+08:00"
    description: A Helm chart for Kubernetes
    digest: fe0a3aaf5fa2ce1dd23ab74ebff6c654e706716eb291a1fd4575a9a5cbf28e64
    name: helloworld
    type: application
    urls:
    - https://wayne-beep.github.io/helm-repo/helloworld-0.1.1.tgz
    version: 0.1.1
  - apiVersion: v2
    appVersion: 1.16.0
    created: "2022-03-04T18:03:32.243429+08:00"
    description: A Helm chart for Kubernetes
    digest: ba76f60a124566b05918a224ecf1df6583c00b44a3d7a5cbbe0f6b2cbbe674ad
    name: helloworld
    type: application
    urls:
    - https://wayne-beep.github.io/helm-repo/helloworld-0.1.0.tgz
    version: 0.1.0
generated: "2022-03-06T09:14:23.010684+08:00"
```
再次提交，即完成 Helm 包的升级

### 本地更新
```shell
$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "testrepo" chart repository
Update Complete. ⎈Happy Helming!⎈

$ helm search repo testrepo
NAME               	CHART VERSION	APP VERSION	DESCRIPTION
testrepo/helloworld	0.1.1        	1.16.0     	A Helm chart for Kubernetes
```
