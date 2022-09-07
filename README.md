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

## nacos官方提供的helm安装之后是各种注册不进去，各种访问不了的。我做了改进。

## 1、nacos安装(2.1)
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
## 1.1、 nacos SQL
```bash
/*
 * Copyright 1999-2018 Alibaba Group Holding Ltd.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
 
/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info   */
/******************************************/
CREATE TABLE `config_info` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(255) DEFAULT NULL,
  `content` longtext NOT NULL COMMENT 'content',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(50) DEFAULT NULL COMMENT 'source ip',
  `app_name` varchar(128) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  `c_desc` varchar(256) DEFAULT NULL,
  `c_use` varchar(64) DEFAULT NULL,
  `effect` varchar(64) DEFAULT NULL,
  `type` varchar(64) DEFAULT NULL,
  `c_schema` text,
  `encrypted_data_key` text NOT NULL COMMENT '秘钥',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfo_datagrouptenant` (`data_id`,`group_id`,`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info';
 
/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_aggr   */
/******************************************/
CREATE TABLE `config_info_aggr` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(255) NOT NULL COMMENT 'group_id',
  `datum_id` varchar(255) NOT NULL COMMENT 'datum_id',
  `content` longtext NOT NULL COMMENT '内容',
  `gmt_modified` datetime NOT NULL COMMENT '修改时间',
  `app_name` varchar(128) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfoaggr_datagrouptenantdatum` (`data_id`,`group_id`,`tenant_id`,`datum_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='增加租户字段';
 
 
/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_beta   */
/******************************************/
CREATE TABLE `config_info_beta` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL COMMENT 'content',
  `beta_ips` varchar(1024) DEFAULT NULL COMMENT 'betaIps',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(50) DEFAULT NULL COMMENT 'source ip',
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  `encrypted_data_key` text NOT NULL COMMENT '秘钥',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfobeta_datagrouptenant` (`data_id`,`group_id`,`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info_beta';
 
/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_tag   */
/******************************************/
CREATE TABLE `config_info_tag` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `tenant_id` varchar(128) DEFAULT '' COMMENT 'tenant_id',
  `tag_id` varchar(128) NOT NULL COMMENT 'tag_id',
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL COMMENT 'content',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(50) DEFAULT NULL COMMENT 'source ip',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfotag_datagrouptenanttag` (`data_id`,`group_id`,`tenant_id`,`tag_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info_tag';
 
/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_tags_relation   */
/******************************************/
CREATE TABLE `config_tags_relation` (
  `id` bigint(20) NOT NULL COMMENT 'id',
  `tag_name` varchar(128) NOT NULL COMMENT 'tag_name',
  `tag_type` varchar(64) DEFAULT NULL COMMENT 'tag_type',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `tenant_id` varchar(128) DEFAULT '' COMMENT 'tenant_id',
  `nid` bigint(20) NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`nid`),
  UNIQUE KEY `uk_configtagrelation_configidtag` (`id`,`tag_name`,`tag_type`),
  KEY `idx_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_tag_relation';
 
/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = group_capacity   */
/******************************************/
CREATE TABLE `group_capacity` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `group_id` varchar(128) NOT NULL DEFAULT '' COMMENT 'Group ID，空字符表示整个集群',
  `quota` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '配额，0表示使用默认值',
  `usage` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '使用量',
  `max_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个配置大小上限，单位为字节，0表示使用默认值',
  `max_aggr_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '聚合子配置最大个数，，0表示使用默认值',
  `max_aggr_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个聚合数据的子配置大小上限，单位为字节，0表示使用默认值',
  `max_history_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最大变更历史数量',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_group_id` (`group_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='集群、各Group容量信息表';
 
/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = his_config_info   */
/******************************************/
CREATE TABLE `his_config_info` (
  `id` bigint(64) unsigned NOT NULL,
  `nid` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `data_id` varchar(255) NOT NULL,
  `group_id` varchar(128) NOT NULL,
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL,
  `md5` varchar(32) DEFAULT NULL,
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `src_user` text,
  `src_ip` varchar(50) DEFAULT NULL,
  `op_type` char(10) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  `encrypted_data_key` text NOT NULL COMMENT '秘钥',
  PRIMARY KEY (`nid`),
  KEY `idx_gmt_create` (`gmt_create`),
  KEY `idx_gmt_modified` (`gmt_modified`),
  KEY `idx_did` (`data_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='多租户改造';
 
 
/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = tenant_capacity   */
/******************************************/
CREATE TABLE `tenant_capacity` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `tenant_id` varchar(128) NOT NULL DEFAULT '' COMMENT 'Tenant ID',
  `quota` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '配额，0表示使用默认值',
  `usage` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '使用量',
  `max_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个配置大小上限，单位为字节，0表示使用默认值',
  `max_aggr_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '聚合子配置最大个数',
  `max_aggr_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个聚合数据的子配置大小上限，单位为字节，0表示使用默认值',
  `max_history_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最大变更历史数量',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='租户容量信息表';
 
 
CREATE TABLE `tenant_info` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `kp` varchar(128) NOT NULL COMMENT 'kp',
  `tenant_id` varchar(128) default '' COMMENT 'tenant_id',
  `tenant_name` varchar(128) default '' COMMENT 'tenant_name',
  `tenant_desc` varchar(256) DEFAULT NULL COMMENT 'tenant_desc',
  `create_source` varchar(32) DEFAULT NULL COMMENT 'create_source',
  `gmt_create` bigint(20) NOT NULL COMMENT '创建时间',
  `gmt_modified` bigint(20) NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_tenant_info_kptenantid` (`kp`,`tenant_id`),
  KEY `idx_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='tenant_info';
 
CREATE TABLE `users` (
	`username` varchar(50) NOT NULL PRIMARY KEY,
	`password` varchar(500) NOT NULL,
	`enabled` boolean NOT NULL
);
 
CREATE TABLE `roles` (
	`username` varchar(50) NOT NULL,
	`role` varchar(50) NOT NULL,
	UNIQUE INDEX `idx_user_role` (`username` ASC, `role` ASC) USING BTREE
);
 
CREATE TABLE `permissions` (
    `role` varchar(50) NOT NULL,
    `resource` varchar(255) NOT NULL,
    `action` varchar(8) NOT NULL,
    UNIQUE INDEX `uk_role_permission` (`role`,`resource`,`action`) USING BTREE
);
 
INSERT INTO users (username, password, enabled) VALUES ('nacos', '$2a$10$EuWPZHzz32dJN7jexM34MOeYirDdFAZm2kuWj7VEOJhhZkDrxfvUu', TRUE);
 
INSERT INTO roles (username, role) VALUES ('nacos', 'ROLE_ADMIN');
 ```



## 1.2、 nacos 安装（个人自行修改nacos数据库配置）
```bash
[root@master test]# ls
nacos  nacos-0.1.5.tgz
[root@master test]# helm install nacos ./nacos
NAME: nacos
LAST DEPLOYED: Tue Sep  6 16:25:13 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services  nacos-cs)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT/nacos
2. MODE:
   standalone: you need to modify replicaCount in the values.yaml, .Values.replicaCount=1
   cluster: kubectl scale sts default-nacos --replicas=3
[root@master test]# 

 ```





## 1.3、 重点：（请执行当前文件夹下的node.yaml把nacos的地址代理出来，以后代码连接nacos就连接31000这个端口即可）
```bash
[root@master nacos]# kubectl apply -f node.yaml 
service/service-nacos-0 created
service/service-nacos-1 created
[root@master nacos]# kubectl get all
NAME                                          READY   STATUS    RESTARTS       AGE
pod/nacos-0                                   1/1     Running   0              4m1s
pod/nfs-client-provisioner-7f48dbb75b-jgj4k   1/1     Running   18 (53m ago)   13d

NAME                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                                                       AGE
service/kubernetes        ClusterIP   10.96.0.1        <none>        443/TCP                                                       13d
service/nacos-cs          NodePort    10.105.124.118   <none>        8848:32495/TCP,9848:31112/TCP,9849:32498/TCP,7848:30000/TCP   4m1s
service/service-nacos-0   NodePort    10.102.172.245   <none>        8848:31000/TCP,9848:32000/TCP                                 7s
service/service-nacos-1   NodePort    10.108.24.135    <none>        8848:31001/TCP,9848:32001/TCP                                 7s

NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nfs-client-provisioner   1/1     1            1           13d

NAME                                                DESIRED   CURRENT   READY   AGE
replicaset.apps/nfs-client-provisioner-7f48dbb75b   1         1         1       13d

NAME                     READY   AGE
statefulset.apps/nacos   1/1     4m1s
[root@master nacos]# 

 ```
 ![image](https://user-images.githubusercontent.com/29905182/188586592-1212f80f-4bfc-406a-af3a-7cd5f27bd157.png)

 
## 2.1、 Seata1.5.1安装
```bash
添加仓库
[root@master ~]# helm repo ls
NAME    URL                                         
myrepo  https://heidaodageshiwo.github.io/helm-chart
[root@master ~]# 

更新仓库  pull helm包或者是直接去github下载即可
[root@master ~]#  helm search  repo myrepo
NAME                    CHART VERSION   APP VERSION     DESCRIPTION                                       
myrepo/helloworld       0.1.0           1.16.0          A Helm chart for Kubernetes                       
myrepo/nacos            0.1.5           1.0             A Helm chart for Kubernetes                       
myrepo/nginx            13.1.7          1.23.1          NGINX Open Source is a web server that can be a...
myrepo/seata-server     1.0.0           1.0             Seata Server  


[root@master ~]# mkdir seatahelmtest
[root@master ~]# cd seatahelmtest/
[root@master seatahelmtest]# ll
总用量 0
[root@master seatahelmtest]# helm pull myrepo/seata-server
[root@master seatahelmtest]# ls
seata-server-1.0.0.tgz
[root@master seatahelmtest]# tar -zxvf seata-server-1.0.0.tgz 
seata-server/Chart.yaml
seata-server/values.yaml
seata-server/templates/NOTES.txt
seata-server/templates/_helpers.tpl
seata-server/templates/deployment.yaml
seata-server/templates/service.yaml
seata-server/templates/tests/test-connection.yaml
seata-server/.helmignore
seata-server/node.yaml
seata-server/servicesss.yaml
[root@master seatahelmtest]# ls
seata-server  seata-server-1.0.0.tgz
[root@master seatahelmtest]# ll
总用量 4
drwxr-xr-x 3 root root  119 9月   7 16:37 seata-server
-rw-r--r-- 1 root root 2636 9月   7 16:37 seata-server-1.0.0.tgz
[root@master seatahelmtest]# ls
seata-server  seata-server-1.0.0.tgz
[root@master seatahelmtest]# helm install seata ./seata-server
NAME: seata
LAST DEPLOYED: Wed Sep  7 16:39:17 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services seata-seata-server)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
[root@master seatahelmtest]# 

 ```
 
## 2.2、 在安装之前需要注意几点（如果说你们按照官网的部署方式能够把8091端口代理出来也不用参考我这个了）
```bash
1.首先把代理的配置文件给挂载出来。我是用的是nfs： https://blog.csdn.net/hunheidaode/article/details/126623672  
2.需要修改values.yaml里面的ip地址：我的是192.168.56.211
3.我是用的是mysql5.7  mysql8的一直报错
4.可以安装了
 ```
 界面访问  ip:31005  seata可视化界面：
 ![image](https://user-images.githubusercontent.com/29905182/188834943-a4dc675c-8a04-44f0-8cd6-914b010e8a68.png)
![image](https://user-images.githubusercontent.com/29905182/188835012-ad33cc2a-19f9-4b9f-bd34-8d5952c5312f.png)

需要注意这个ip与端口
![image](https://user-images.githubusercontent.com/29905182/188835182-fb293db0-b767-43d5-b906-c4f83e100fef.png)
![image](https://user-images.githubusercontent.com/29905182/188835260-84386d66-04e5-4d74-b5b0-1596adde676d.png)
地址要一样即可。
代码的连接方式在博客中：https://blog.csdn.net/hunheidaode/article/details/126623672 
我也已经提到过了。这里就不贴了。

## 2.3、 测试：
```bash

![image](https://user-images.githubusercontent.com/29905182/188837910-df33c779-464f-4558-a1b9-f1b4a1425888.png)
![image](https://user-images.githubusercontent.com/29905182/188837980-842cd77a-20c4-4e29-a371-c81eeecf040c.png)

如有问题请在博客中发表评论。

 ```
 
 
  
## 2.4、 
```bash
 ```
 
## 2.5、
```bash
 ```
 
## 2.6、 
```bash
 ```
 
 
 
 
 
 
 
