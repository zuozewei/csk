---
keyword: [aliyun-acr-acceleration-suite, aliyun-acr-acceleration-suite组件变更记录]
---

# aliyun-acr-acceleration-suite

本文介绍aliyun-acr-acceleration-suite组件的功能并展示该组件变更记录。

## 组件介绍

aliyun-acr-acceleration-suite是提供镜像按需加载加速能力的客户端插件，以DaemonSet形式部署在Worker节点上。通过配合使用ACR的加速镜像自动转换功能，您可以在业务部署中使用加速镜像，实现镜像数据免全量下载和在线解压，大幅提升应用分发效率，享受极致的弹性体验，详细介绍请参见[按需加载容器镜像]()。

## 变更记录

|版本号|镜像地址|变更时间|变更内容|变更影响|
|---|----|----|----|----|
|v20.09.16.0-3afa0663-aliyun|registry.cn-hangzhou.aliyuncs.com/acs/aliyun-acr-acceleration-suite:v20.09.16.0-3afa0663-aliyun|2020年9月16日|新增组件，主要提供相关镜像存储插件的一键安装和卸载，以及镜像仓库服务的访问配置。|安装和卸载流程会重启您节点上的Docker，不会对您正在运行的容器造成影响。|

