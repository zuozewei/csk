# 使用镜像创建无状态Deployment应用

您可以使用镜像创建一个无状态应用。本文以Nginx为例说明如何通过镜像来创建无状态应用。

[创建Kubernetes托管版集群](/cn.zh-CN/Kubernetes集群用户指南/集群管理/创建集群/创建Kubernetes托管版集群.md)

## 步骤一：配置应用基本信息

1.  登录[容器服务管理控制台](https://cs.console.aliyun.com)。

2.  在控制台左侧导航栏中，单击**集群**。

3.  在集群列表页面中，单击目标集群名称或者目标集群右侧**操作**列下的**详情**。

4.  在集群管理页左侧导航栏中，单击**工作负载**。

5.  在**无状态**页签中，单击**使用镜像创建**。

6.  在**应用基本信息**页签中，设置应用的基本信息。

    ![基本配置](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0885659951/p10973.png)

    |配置项|描述|
    |---|--|
    |应用名称|设置应用的名称。|
    |命名空间|设置应用部署所处的命名空间，默认使用default命名空间。|
    |副本数量|即应用包含的Pod数量，默认数量为2。|
    |类型|定义资源对象的类型，可选择**无状态**、**有状态**、**任务**、**定时**、**定时任务**、**守护进程集**。|
    |标签|为该应用添加一个标签，标识该应用。|
    |注解|为该应用添加一个注解（annotation）。|
    |时区同步|容器与节点是否使用相同的时区。|

    **说明：** 本例中选择**无状态**类型，即Deployment类型。

7.  单击**下一步**。

    进入**容器配置**页面。


## 步骤二：配置容器

在**容器配置**页签中，配置容器的镜像与资源、端口、环境变量、健康检查、生命周期、数据卷和日志。

**说明：** 在**容器配置**页签上方，单击**添加容器**为应用的Pod设置多个容器。

1.  在**基本配置**区域，完成容器的基本配置。

    ![设置容器基本信息](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3506659951/p12305.png)

    |配置项|描述|
    |---|--|
    |镜像名称|    -   您可以单击**选择镜像**，选择所需的镜像并单击**确定**。本例中为官方Nginx镜像，可以在**搜索**页签中，选择**Docker镜像**，然后在文本框中输入nginx进行搜索。
        -   容器镜像服务：在**容器镜像服务**页签中，可以选择托管在容器镜像服务ACR中的镜像。需要选择镜像所属地域，以及镜像服务实例。关于ACR的更多信息，请参见[ACR 企业版概述]()。

**说明：** 您可以在**容器镜像服务**页签，根据名称搜索托管在容器镜像服务ACR中的镜像。

        -   Docker官方镜像：在**Docker官方镜像**页签中，可以选择Docker官方提供的容器镜像。
        -   用户收藏：在**用户收藏**页签中，可以选择您收藏的Docker官方镜像。
        -   搜索：在**搜索**页签中，可以分别在ACR的特定地域和Docker官方镜像中搜索容器镜像。
    -   您还可以填写私有镜像。填写的格式为`domainname/namespace/imagename:tag`。 |
    |镜像Tag

|    -   您可以单击**选择镜像Tag**选择镜像的版本。若不指定，默认为最新版。
    -   ACK支持以下三种镜像拉取策略（imagePullPolicy）：

        -   **优先使用本地镜像（ifNotPresent）**：如果本地有该镜像，则使用本地镜像，本地不存在时拉取镜像。
        -   **总是拉取镜像（Always）**：表示每次部署或扩容都会从容器镜像服务重新拉取镜像，而不会从本地拉取镜像。
        -   **仅使用本地镜像（Never）**：仅使用本地镜像。
**说明：** 当您选中**镜像拉取策略**时，默认不设置镜像拉取策略。

    -   单击**设置镜像密钥**，您可以实现免密拉取镜像，详情请参见[使用免密组件拉取容器镜像](/cn.zh-CN/Kubernetes集群用户指南/应用管理/使用免密组件拉取容器镜像.md)。 |
    |资源限制|可指定该应用所能使用的资源上限，包括CPU、内存和ephemeral-storage三种资源，防止占用过多资源。|
    |所需资源|即为该应用预留资源额度，包括CPU和内存两种资源，即容器独占该资源，防止因资源不足而被其他服务或进程争夺资源，导致应用不可用。|
    |容器启动项|    -   stdin：将控制台输入发送到容器。
    -   tty：将标准输入控制台作为容器的控制台输入。 |
    |特权容器|    -   选择特权容器，则privileged=true，开启特权模式。
    -   不选择特权容器，则privileged=false，关闭特权模式。 |
    |Init Container|勾选该项，表示创建一个Init Container。Init Container包含一些实用的工具，详情请参见[Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/?spm=a2c4g.11186623.2.13.3fdd30dfnyevPx)。|

2.  在**端口设置**区域，单击**新增**设置容器的端口。

    -   名称：设置容器端口名称。
    -   容器端口：设置暴露的容器访问端口或端口名，端口号必须介于1~65535。
    -   协议：支持TCP和UDP。
3.  在**环境变量**区域，单击**新增**设置环境变量。

    支持通过键值对的形式为Pod配置环境变量。用于给Pod添加环境标志或传递配置等，详情请参见[Pod variable](https://kubernetes.io/docs/tasks/inject-data-application/environment-variable-expose-pod-information/?spm=0.0.0.0.8VJbrE)。

    -   类型：设置环境变量的类型，支持**自定义**、**配置项**、**密钥**、**变量/变量引用**、**资源引用**。配置项、密钥支持全部文件的引用，以密钥为例。

        选择**密钥**，选择**变量**，默认全部文件引用。

        ![环境变量](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3506659951/p130410.png)

        对应的yaml，则引用了整个Secret。

        ![yaml](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3506659951/p130413.jpg)

    -   变量名称：设置环境变量名称。
    -   变量/变量引用：设置变量引用的值。
4.  在**健康检查**区域，根据需要开启**存活检查**和**就绪检查**。

    关于健康检查的更多信息，请参见[Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)。

    -   存活检查（Liveness）：用于检测何时重启容器。
    -   就绪检查（Readiness）：确定容器是否已经就绪，且可以接受流量。
    |请求类型|配置说明|
    |----|----|
    |HTTP请求|即向容器发送一个HTTP Get请求，支持的参数包括：    -   协议：HTTP/HTTPS。
    -   路径：访问HTTP Server的路径。
    -   端口：容器暴露的访问端口或端口名，端口号必须介于1~65535。
    -   HTTP头：即HTTP Headers，HTTP请求中自定义的请求头，HTTP允许重复的Header。支持键值对的配置方式。
    -   延迟探测时间（秒）：即initialDelaySeconds，容器启动后第一次执行探测时需要等待多少秒，默认为3秒。
    -   执行探测频率（秒）：即periodSeconds，指执行探测的时间间隔，默认为10秒，最小为1秒。
    -   超时时间（秒）：即timeoutSeconds，探测超时时间。默认1秒，最小1秒。
    -   健康阈值：探测失败后，最少连续探测成功多少次才被认定为成功。默认是1，最小值是1。对于存活检查（liveness）必须是1。
    -   不健康阈值：探测成功后，最少连续探测失败多少次才被认定为失败。默认是3，最小值是1。 |
    |TCP连接|即向容器发送一个TCP Socket，Kubelet将尝试在指定端口上打开容器的套接字。 如果可以建立连接，容器被认为是健康的，如果不能就认为是失败的。支持的参数包括：    -   端口：容器暴露的访问端口或端口名，端口号必须介于1~65535。
    -   延迟探测时间（秒）：即initialDelaySeconds，容器启动后第一次执行探测时需要等待多少秒，默认为15秒。
    -   执行探测频率（秒）：即periodSeconds，指执行探测的时间间隔，默认为10秒，最小为1秒。
    -   超时时间（秒）：即timeoutSeconds，探测超时时间。默认1秒，最小1秒。
    -   健康阈值：探测失败后，最少连续探测成功多少次才被认定为成功。默认是1，最小值是1。对于存活检查（liveness）必须是1。
    -   不健康阈值：探测成功后，最少连续探测失败多少次才被认定为失败。默认是3，最小值是1。 |
    |命令行|通过在容器中执行探针检测命令，来检测容器的健康情况。支持的参数包括：    -   命令行：用于检测容器健康情况的探测命令。
    -   延迟探测时间（秒）：即initialDelaySeconds，容器启动后第一次执行探测时需要等待多少秒，默认为5秒。
    -   执行探测频率（秒）：即periodSeconds，指执行探测的时间间隔，默认为10秒，最小为1秒。
    -   超时时间（秒）：即timeoutSeconds，探测超时时间。默认1秒，最小1秒。
    -   健康阈值：探测失败后，最少连续探测成功多少次才被认定为成功。默认是1，最小值是1。对于存活检查（liveness）必须是1。
    -   不健康阈值：探测成功后，最少连续探测失败多少次才被认定为失败。默认是3，最小值是1。 |

5.  在**生命周期**区域，设置容器的生命周期。

    您可以为容器的生命周期配置启动执行、启动后处理和停止前处理。具体参见[配置生命周期](https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/)。

    -   **启动执行**：为容器设置预启动命令和参数。
    -   **启动后处理**：为容器设置启动后的命令。
    -   **停止前处理**：为容器设置预结束命令。
    ![生命周期](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7975659951/p134220.png)

6.  在**数据卷**区域，增加本地存储或云存储声明PVC（Persistent Volume Claim）。

    -   本地存储：支持主机目录（hostpath）、配置项（configmap）、保密字典（secret）和临时目录，将对应的挂载源挂载到容器路径中。更多信息参见[volumes](https://kubernetes.io/docs/concepts/storage/volumes/?spm=0.0.0.0.8VJbrE)。
    -   云存储声明（PVC）：支持挂载云存储。
    本例中配置了一个云存储类型的数据卷声明disk-ssd，将其挂载到容器的/tmp路径下。

    ![配置数据卷](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3506659951/p12307.png)

7.  在**日志配置**区域，进行采集配置和自定义Tag设置。

    **说明：** 请确保已部署Kubernetes集群，并且在此集群上已安装日志插件。

    |配置项|描述|
    |---|--|
    |采集配置|日志库：即在日志服务中生成一个对应的Logstore，用于存储采集到的日志。|
    |容器内日志路径：支持Stdout和文本日志。

    -   Stdout：表示采集容器的标准输出日志。
    -   文本日志：表示收集容器内指定路径的日志，本例中表示收集/var/log/nginx下所有的文本日志，也支持通配符的方式。 |
    |自定义Tag|您还可以设置自定义Tag，设置Tag后，会将该Tag一起采集到容器的日志输出中。自定义Tag可帮助您给容器日志打上Tag，方便进行日志统计和过滤等分析操作。|

8.  单击**下一步**。

    进入**高级配置**页面。


## 步骤三：完成高级配置

在**高级配置**页签中设置访问、伸缩、调度和标签注解。

1.  在**访问设置**区域，设置暴露后端Pod的方式。

    **说明：**

    针对应用的通信需求，您可灵活进行访问设置：

    -   内部应用：对于只在集群内部工作的应用，您可以在创建服务时，根据需要**虚拟集群IP**或**节点端口**类型的服务，来进行内部通信。
    -   外部应用：对于需要暴露到公网的应用，您可以采用两种方式进行访问设置。
        -   创建负载均衡类型的服务：您可以在创建服务时，选择**负载均衡**类型的服务。通过阿里云提供的负载均衡服务SLB（Server Load Balancer），使得该服务提供公网访问能力。
        -   创建路由（Ingress）：通过创建路由（Ingress）提供公网访问能力。关于Ingress的更多信息请参见[Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/?spm=a2c4g.11186623.2.23.3fdd30dfnyevPx)。
    您可以设置暴露后端Pod的方式。本例中选择虚拟集群IP和路由（Ingress），构建一个公网可访问的Nginx应用。

    -   配置服务（Service）：在**服务（Service）**右侧，单击**创建**设置创建服务配置项。

        |配置项|描述|
        |---|--|
        |名称|输入服务的名称。本例为nginx-svc。|
        |类型|选择服务类型，即服务访问的方式。本例中选择**虚拟集群IP**。        -   **虚拟集群IP**：即ClusterIP，指通过集群的内部IP暴露服务，选择该值，服务只能够在集群内部可以访问，这也是默认的ServiceType。

**说明：** 您的服务类型为**虚拟集群IP**时，才能设置**实例间发现服务（Headless Service）**。

        -   **节点端口**：即NodePort，通过每个Node上的IP和静态端口（NodePort）暴露服务。NodePort服务会路由到ClusterIP服务，该ClusterIP服务会自动创建。通过请求`<NodeIP>:<NodePort>`，可以从集群的外部访问一个NodePort服务。
        -   **负载均衡**：即LoadBalancer，指阿里云提供的负载均衡服务（SLB），可选择公网访问或内部访问。阿里云负载均衡服务可以路由到NodePort服务和ClusterIP服务。

            -   新建SLB：您可以通过单击**修改**，修改SLB规格。
            -   使用已有负载均衡：您可以在已有的列表中选择SLB规格。
**说明：** 负载均衡类型支持新建SLB和使用已有SLB，且多个Kubernetes Service可以复用同一个SLB，但是存在以下限制：

            -   使用已有的负载均衡实例会强制覆盖已有监听。
            -   Kubernetes通过Service创建的SLB不能复用（会导致SLB被意外删除）。只能复用您手动在控制台（或调用OpenAPI）创建的SLB。
            -   复用同一个SLB的多个Service不能有相同的前端监听端口，否则会造成端口冲突。
            -   复用SLB时，监听的名字以及虚拟服务器组的名字被Kubernetes作为唯一标识符。请勿修改监听和虚拟服务器组的名字。
            -   不支持跨集群复用SLB。 |
        |端口映射|添加服务端口和容器端口。容器端口需要与后端的Pod中暴露的容器端口一致。|
        |外部流量策略|        -   Local：流量只发给本机的Pod。
        -   Cluster：流量可以转发到其他节点上的Pod。
**说明：** 您的服务类型为**节点端口**或**负载均衡**时，才能设置**外部流量策略**。 |
        |注解|为该服务添加一个注解（annotation），配置负载均衡的参数。例如设置`service.beta.kubernetes.io/alicloud-loadbalancer-bandwidth:20`表示将该服务的带宽峰值设置为20Mbit/s，从而控制服务的流量。更多参数请参见[通过Annotation配置负载均衡](/cn.zh-CN/Kubernetes集群用户指南/网络管理/Service管理/通过Annotation配置负载均衡.md)。|
        |标签|为该服务添加一个标签，标识该服务。|

    -   配置路由（Ingress）：在**路由（Ingress）**右侧，单击**创建**设置后端Pod的路由规则。

        详细的路由配置信息，请参见[路由配置说明](/cn.zh-CN/Kubernetes集群用户指南/网络管理/Ingress管理/路由配置说明.md)。

        **说明：** 通过镜像创建应用时，您仅能为一个服务创建路由（Ingress）。本例中使用一个虚拟主机名称作为测试域名，您需要在hosts文件中添加一条域名映射（<Ingress外部端点\> + <Ingress域名\>）。在实际工作场景中，请使用备案域名。

        ```
        101.37.xxx.xxx   foo.bar.com    #即ingress的IP
        ```

        |配置项|描述|
        |---|--|
        |名称|输入路由的名称。本例为nginx-ingress。|
        |规则|路由规则是指授权入站到达集群服务的规则。详情请参见[路由配置说明](/cn.zh-CN/Kubernetes集群用户指南/网络管理/Ingress管理/路由配置说明.md)。        -   **域名**：输入Ingress域名。本例中使用测试域名`foo.bar.com`。
        -   **路径**：指定服务访问的URL路径，默认为根路径/，本例中不做配置。每个路径 （path）都关联一个backend（服务），在阿里云SLB将流量转发到backend之前，所有的入站请求都要先匹配域名和路径。
        -   **服务**：选择服务的名称和对应端口。本例中为nginx-svc.
        -   **开启TLS**：配置安全的路由服务。具体可参见[配置Ingress](/cn.zh-CN/Kubernetes集群用户指南/网络管理/Ingress管理/配置Ingress.md)。 |
        |服务权重|设置该路径下多个服务的权重。服务权重采用相对值计算方式，默认值为100。|
        |灰度发布策略|设置灰度规则后，请求头中满足灰度发布匹配规则的请求才能被路由到新版本服务中。如果该服务设置了100%以下的权重比例，满足灰度规则的请求会继续依据权重比例路由到对应服务。容器服务支持多种流量切分方式，适用于灰度发布以及AB测试场景。        -   基于Request Header的流量切分
        -   基于Cookie的流量切分
        -   基于Query Param的流量切分
**说明：** 目前阿里云容器服务Kubernetes Ingress Controller需要0.12.0-5及其以上版本才支持流量切分特性。

配置参数如下所示：        -   **服务**：路由规则配置的服务。
        -   **类型**：支持Header（请求头）、Cookie和Query（请求参数）的匹配规则。
        -   **名称**和**匹配值**：用户自定义的请求字段，名称和匹配值为键值对。
        -   **匹配规则**：支持正则匹配和完全匹配。 |
        |注解|        -   单击**重定向注解**，可为路由添加一条典型的重定向注解。即`nginx.ingress.kubernetes.io/rewrite-target：/`，表示将/path路径重定向到后端服务能够识别的根路径/上面。
        -   您也可以单击**添加**按钮，输入注解名称和值，即Ingress的annotation键值对，Ingress的注解参见[Annotations](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/)。 |
        |标签|为Ingress添加对应的标签，标示该Ingress的特点。|

    在**访问设置**区域，您可以看到创建完毕的服务和路由，您可单击**变更**和**删除**进行二次配置。

    ![变更或删除路由](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5975659951/p10982.png)

2.  在**伸缩配置**区域，勾选是否开启**容器组水平伸缩**，从而满足应用在不同负载下的需求。

    容器服务支持容器组的弹性伸缩，即根据容器CPU和内存资源占用情况自动调整容器组数量。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3506659951/p10978.png)

    **说明：** 若要启用自动伸缩，您必须为容器设置所需资源，否则容器自动伸缩无法生效。

    |配置项|描述|
    |---|--|
    |指标|支持CPU和内存，需要和设置的所需资源类型相同。|
    |触发条件|资源使用率的百分比，超过该使用量，容器开始扩容。|
    |最大副本数量|该应用可扩容的容器数量上限。|
    |最小副本数量|该应用可缩容的容器数量下限。|

3.  在**调度设置**区域，设置升级方式、节点亲和性、应用亲和性和应用非亲和性，详情请参见[Affinity and anti-affinity](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/?spm=a2c4g.11186623.2.31.3fdd30dfnyevPx#affinity-and-anti-affinity)。

    **说明：** 亲和性调度依赖节点标签和Pod标签。您可使用内置的标签进行调度；也可预先为节点、Pod配置相关的标签。

    |配置项|描述|
    |---|--|
    |升级方式|升级方式包括滚动升级（rollingupdate）和替换升级（recreate），详细请参见详情请参见[Deployments](https://kubernetes.io/zh/docs/concepts/workloads/controllers/deployment/?spm=a2c4g.11186623.2.32.3fdd30dfnyevPx)。 |
    |节点亲和性|设置**节点亲和性**，通过Worker节点的Label标签进行设置。节点调度支持硬约束和软约束（Required和Preferred），以及丰富的匹配表达式（In, NotIn, Exists, DoesNotExist, Gt, and Lt）：

    -   **必须满足**，即硬约束，一定要满足，对应requiredDuringSchedulingIgnoredDuringExecution，效果与`NodeSelector`相同。本例中Pod只能调度到具有对应标签的Worker节点。您可以定义多条硬约束规则，但只需满足其中一条。
    -   **尽量满足**，即软约束，不一定满足，对应preferredDuringSchedulingIgnoredDuringExecution。本例中，调度会尽量不调度Pod到具有对应标签的Node节点。您还可为软约束规则设定权重，具体调度时，若存在多个符合条件的节点，权重最大的节点会被优先调度。您可定义多条软约束规则，但必须满足全部约束，才会进行调度。 |
    |应用亲和性|决定应用的Pod可以和哪些Pod部署在同一拓扑域。例如，对于相互通信的服务，可通过应用亲和性调度，将其部署到同一拓扑域（如同一个主机）中，减少它们之间的网络延迟。根据节点上运行的Pod的标签（Label）来进行调度，支持硬约束和软约束，匹配的表达式有：`In, NotIn, Exists, DoesNotExist`。

    -   **必须满足**，即硬约束，一定要满足，对应requiredDuringSchedulingIgnoredDuringExecution，Pod的亲和性调度必须要满足后续定义的约束条件。
        -   **命名空间**：该策略是依据Pod的Label进行调度，所以会受到命名空间的约束。
        -   **拓扑域**：即topologyKey，指定调度时作用域，这是通过Node节点的标签来实现的，例如指定为`kubernetes.io/hostname`，那就是以Node节点为区分范围；如果指定为`beta.kubernetes.io/os`，则以Node节点的操作系统类型来区分。
        -   **选择器**：单击选择器右侧的加号按钮，您可添加多条硬约束规则。
        -   **查看应用列表**：单击**应用列表**，弹出对话框，您可在此查看各命名空间下的应用，并可将应用的标签导入到亲和性配置页面。
        -   硬约束条件：设置已有应用的标签、操作符和标签值。本例中，表示将待创建的应用调度到该主机上，该主机运行的已有应用具有`app:nginx`标签。
    -   **尽量满足**，即软约束，不一定满足，对应preferredDuringSchedulingIgnoredDuringExecution。Pod的亲和性调度会尽量满足后续定义的约束条件。对于软约束规则，您可配置每条规则的权重，其他配置规则与硬约束规则相同。

**说明：** **权重**：设置一条软约束规则的权重，介于1~100，通过算法计算满足软约束规则的节点的权重，将Pod调度到权重最大的节点上。 |
    |应用非亲和性|决定应用的Pod不与哪些Pod部署在同一拓扑域。应用非亲和性调度的场景包括：

    -   将一个服务的Pod分散部署到不同的拓扑域（如不同主机）中，提高服务本身的稳定性。
    -   给予Pod一个节点的独占访问权限来保证资源隔离，保证不会有其它Pod来分享节点资源。
    -   把可能会相互影响的服务的Pod分散在不同的主机上。
**说明：** 应用非亲和性调度的设置方式与亲和性调度相同，但是相同的调度规则代表的意思不同，请根据使用场景进行选择。 |
    |调度容忍|容忍被应用于Pod，允许这个Pod被调度到相对应的污点上。|
    |调度到虚拟节点|设置是否调度到虚拟节点。如果您集群中没有虚拟节点，则无法设置该配置项。|

4.  在**标签和注释**区域，单击**添加**设置容器组的标签和注释。

    -   Pod标签：为该Pod添加一个标签，标识该应用。
    -   Pod注解：为该Pod添加一个注解（annotation）。
5.  单击**创建**。


## 步骤四：查看应用

在**创建完成**页签中查看应用任务。

1.  在**创建完成**页签中单击**查看应用详情**。

    ![查看详情](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3506659951/p10983.png)

    默认进入新建的nginx-deployment的详情页面。

    **说明：** 您也可以通过以下操作创建路由与服务。如上图所示，在**访问方式**页签。

    -   单击服务右侧的**创建**，也可以创建服务。
    -   单击路由右侧的**创建**，也可以创建路由。
2.  单击左侧导航栏的**路由与负载均衡** \> **路由**，可以看到路由列表下出现一条规则。

    ![路由规则](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3506659951/p10985.png)

3.  在浏览器中访问路由测试域名，您可访问Nginx欢迎页。

    ![访问nginx](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3506659951/p10986.png)


## 相关操作

在左侧导航栏单击**集群**，单击目标集群名称或目标集群**操作**列下的**详情**，选择**工作负载**，在无状态页面单击目标应用名称或目标应用**操作**列下的**详情**，在应用详情页面您可以**编辑**、**伸缩**、**查看Yaml**、**重新部署**、**刷新**应用。

-   编辑：在应用详情页面单击**编辑**，您可以修改应用信息。
-   伸缩：在应用详情页面单击**伸缩**，您可以修改所需容器组数量。
-   查看Yaml：在应用详情页面单击**查看Yaml**，您可以**更新**、**下载**、**另存为**Yaml文件。
-   重新部署：在应用详情页面单击**重新部署**，您可以重新部署应用。
-   刷新：在应用详情页面单击**刷新**，您可以刷新应用。

## 相关文档

-   [使用镜像创建有状态StatefulSet应用](/cn.zh-CN/Kubernetes集群用户指南/应用管理/使用镜像创建有状态StatefulSet应用.md)
-   [通过编排模板创建Linux应用](/cn.zh-CN/Kubernetes集群用户指南/应用管理/通过编排模板创建Linux应用.md)
-   [通过Annotation配置负载均衡](/cn.zh-CN/Kubernetes集群用户指南/网络管理/Service管理/通过Annotation配置负载均衡.md)

