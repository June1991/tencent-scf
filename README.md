[![Serverless Components](https://img.serverlesscloud.cn/202047/1586246008932-Tencent%20SCF_%E9%95%BF.png)](http://serverless.com)

<br/>

## 组件概述

**腾讯云 SCF 云函数组件** ⎯⎯⎯ 通过使用 [Tencent Serverless Framework](https://github.com/serverless/components/tree/cloud)，基于云上 Serverless 服务（云函数及触发器等），实现“0”配置，便捷开发，极速部署你的第一个云函数，该组件支持丰富的配置扩展，提供了目前最易用、低成本并且弹性伸缩的云函数的开发、配置及部署能力。

<br/>

## 快速入门

### 前提

- 已安装Serverless Framework。参考[《Serverless Framework安装》]()
- 账号开通Serverless相关权限。参考[《Serverless Framework账号权限》]()

<br/>

### 操作步骤

#### 创建

方法一：按照[《快速部署》](https://cloud.tencent.com/document/product/1154/41775)操作，选择SCF项目模板进行创建。

方法二：直接使用sls init 命令创建。快速创建一个nodejs的scf示例：

```
sls init scf-demo
```

命令中的scf-demo可以更换成其他语言模板。目前scf组件支持的模板：go1-helloworld 、nodejs1015-helloworld、php72-helloworld、python36-helloworld。

#### 部署

执行以下命令，将会弹出二维码，直接扫码授权进行部署：

```
sls deploy
```

>? 如果鉴权失败，请参考[权限配置](https://cloud.tencent.com/document/product/1154/43006)进行授权。

#### 查看

执行以下命令，查看你部署的项目信息：

```
sls info
```

#### 移除

执行以下命令，移除你已经部署的项目：

```
sls remove
```



## 进阶指导

### serverless.yml

执行`sls deploy` 时，根据serverless.yml文件中的配置对云函数资源进行创建或更新。一份简单的serverlesss.yml文件如下：

> 说明：
>
> 配置详情请参考 [全量配置文档](https://github.com/serverless-components/tencent-scf/blob/master/docs/configure.md)。

```
#scf组件配置样例
#全量配置参考https://github.com/serverless-components/tencent-scf/blob/master/docs/configure.md

#组件信息
component: scf # (必填) 引用 component 的名称，当前用到的是 tencent-scf 组件
name: scfdemo # (必填) 创建的实例名称，请修改成您的实例名称

#组件参数
inputs:
  name: ${name}-${stage}-${app} #函数名称
  src: ./  #代码路径
  handler: index.main_handler #入口
  runtime: Nodejs10.15 # 云函数运行时的环境
  region: ap-guangzhou # 云函数所在区域
  events: # 触发器
    - apigw: # 网关触发器
        parameters:
          endpoints:
            - path: /
              method: GET
```

serverless.yml文件包含的信息：

- **组件信息**

  component:  (必填) 引用 component 的名称，使用`sls registry`查询你可以引入的组件。

  name:  (必填) 创建的实例名称，每个组件在部署时将创建一个实例。

- **参数信息**

  inputs下的参数为组件配置参数。一个最简单SCF组件参数配置由以下几部分：

  name： 云函数名称。犹豫云函数又是资源 ID，为了保证资源的唯一性，建议采用 `${name}-${stage}-${app}` 变量方式。 

  src:  代码路径。

  handler： 函数处理方法名称 。

  runtime:  云函数运行环境。 目前支持: `Nodejs6.10，Nodejs8.9，Nodejs10.15，Nodejs12.16，Python2.7，Python3.6，PHP5，PHP7，Go1，Java8` 。

  region:  云函数所在的区域。

  events:  触发器。 支持的触发器为：timer、apigw、cos、cmq、ckafka 。



### 账号权限

部署实例时需要账号授权去操作具体的云资源，目前可以通过两种方式进行授权：**扫码授权**和**密钥授权**。

**扫码授权** 能快速进行授权部署，但生成的凭证是临时凭证，过期后需要重新扫码。

**密钥授权 **能够获得永久授权，需要预先配置账号的SecretId 和 SecretKey 。

配置详情可参考[权限配置](https://cloud.tencent.com/document/product/1154/43006)**



### 开发调试

在 `serverless.yml`文件所在的目录下运行 `sls dev` 可以实时输出云端日志，每次部署完毕后，对项目进行访问，即可在命令行中实时输出调用日志，便于查看业务情况和排障。nodejs支持开启开发调试能力，将会对本地代码的改动进行检测和自动上传。详情参考[开发与调试](https://cloud.tencent.com/document/product/1154/43220)。



### 应用管理

Serverless部署一个组件实例实质是部署了一个单组件实例的应用。

在应用项目开发过程中，一个应用下可能会存在多个组件实例，如何管理组件实例进行应用项目开发，请参考[应用管理](https://cloud.tencent.com/document/product/1154/48261)。

