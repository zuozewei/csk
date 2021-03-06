# 使用HTTPS来访问Ingress Gateway

本文详细讲述如何使用HTTPS来访问Ingress Gateway。

## 环境准备

1.  通过[CloudShell](/cn.zh-CN/Kubernetes集群用户指南/集群管理/管理与访问集群/在CloudShell上通过kubectl管理Kubernetes集群.md) 连接 Kubernetes 集群。

2.  执行如下命令`curl --version | grep LibreSSL`命令，确认测试环境。

    如果输出结果包含LibreSSL的版本信息，表示使用的是用LibreSSL库编译的curl，即，表示环境已准备就绪。

    ```
    curl 7.54.0 (x86_64-apple-darwin18.0) libcurl/7.54.0 LibreSSL/2.6.4 zlib/1.2.11 nghttp2/1.24.1
    ```

3.  部署一个可用的Bookinfo应用，请参考[部署Bookinfo应用](/cn.zh-CN/最佳实践/Istio/使用阿里云容器服务部署Bookinfo示例.md)。

4.  执行如下命令，设置访问入口。

    ```
    export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http")].port}')
    export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
    ```

5.  执行如下命令，清理旧的 Gateway 和 VirtualService。

    ```
    kubectl delete gateway --all
    kubectl delete virtualservice --all
    ```


## 生成证书

通过[CloudShell](/cn.zh-CN/Kubernetes集群用户指南/集群管理/管理与访问集群/在CloudShell上通过kubectl管理Kubernetes集群.md)执行如下命令，使用[https://github.com/nicholasjackson/mtls-go-example](https://github.com/nicholasjackson/mtls-go-example)来生成需要的证书。

```
1. git clone https://github.com/nicholasjackson/mtls-go-example
2. pushd mtls-go-example
3. ./generate.sh bookinfo.com  &lt;password&gt;
4. mkdir ~+1/bookinfo.com && mv 1_root 2_intermediate 3_application 4_client ~+1/bookinfo.com
5. popd
```

## 配置TLS Ingress Gateway

1.  执行如下命令，创建secret来保存证书的内容。

    ```
    kubectl create -n istio-system secret tls istio-ingressgateway-certs --key bookinfo.com/3_application/private/bookinfo.com.key.pem --cert bookinfo.com/3_application/certs/bookinfo.com.cert.pem
    ```

2.  创建并拷贝内容到book-info-gateway.yaml文件中，并执行`kubectl apply -f book-info-gateway.yaml`命令，定义一个Gateway对象bookinfo-gateway。

    ```
    apiVersion: networking.istio.io/v1alpha3
    kind: Gateway
    metadata:
      name: bookinfo-gateway
    spec:
      selector:
        istio: ingressgateway # use istio default ingress gateway
      servers:
      - port:
          number: 443
          name: https
          protocol: HTTPS
        tls:
          mode: SIMPLE
          serverCertificate: /etc/istio/ingressgateway-certs/tls.crt
          privateKey: /etc/istio/ingressgateway-certs/tls.key
        hosts:
        - "bookinfo.com"
    ```

3.  创建并拷贝内容到book-info-vs.yaml文件中，并执行`kubectl apply -f book-info-vs.yaml`命令，配置一个关联此Gateway的路由规则。

    ```
    apiVersion: networking.istio.io/v1alpha3
    kind: VirtualService
    metadata:
      name: bookinfo
    spec:
      hosts:
      - "bookinfo.com"
      gateways:
      - bookinfo-gateway
      http:
      - match:
        - uri:
            exact: /productpage
        - uri:
            exact: /login
        - uri:
            exact: /logout
        - uri:
            prefix: /api/v1/products
        route:
        - destination:
            host: productpage
            port:
              number: 9080
    ```

4.  执行如下命令，访问服务。

    ```
    curl -v -HHost:bookinfo.com --resolve bookinfo.com:$SECURE_INGRESS_PORT:$INGRESS_HOST --cacert bookinfo.com/2_intermediate/certs/ca-chain.cert.pem https://bookinfo.com:$SECURE_INGRESS_PORT/status/418
    ```

    **说明：** 如果要在浏览器里访问服务，请先配置好DNS或者添加hosts条目。


