# helm-chart

## 张强仓库 添加方式：


helm repo add myrepo  https://heidaodageshiwo.github.io/helm-chart


使用方式：
[root@master ~]# helm search repo myrepo
NAME                    CHART VERSION   APP VERSION     DESCRIPTION                                       
myrepo/helloworld       0.1.0           1.16.0          A Helm chart for Kubernetes                       
myrepo/nginx            13.1.7          1.23.1          NGINX Open Source is a web server that can be a...
[root@master ~]# 


![c51d6917f82b751126cce9ebba14685](https://user-images.githubusercontent.com/29905182/187143154-95c8cbde-54a7-4d59-be07-2c22725e4410.jpg)

## 查询 
```bash
helm search  repo myrepo
```
## 更新 

```bash
 helm repo update
 ```
## 查询 nacos
```bash
 helm search  repo myrepo
 ```

![image](https://user-images.githubusercontent.com/29905182/188583345-8437d05b-98e4-4a2b-b473-5e6089a709a1.png)


## 1、nacos安装
```bash

[root@master test]# helm pull myrepo/nacos
[root@master test]# ls
nacos-0.1.5.tgz
[root@master test]# tar -zxvf nacos-0.1.5.tgz 
nacos/Chart.yaml
nacos/values.yaml
nacos/templates/NOTES.txt
nacos/templates/_helpers.tpl
nacos/templates/configmap.yaml
nacos/templates/ingress.yaml
nacos/templates/service.yaml
nacos/templates/statefulset.yaml
nacos/.helmignore
nacos/README.md
nacos/node.yaml
[root@master test]# 
 ```


