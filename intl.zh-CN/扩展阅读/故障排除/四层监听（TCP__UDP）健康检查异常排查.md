# 四层监听（TCP/UDP）健康检查异常排查 {#task_j5n_rgz_xfb .task}

健康检查用于探测您的后端服务器是否处于正常工作状态，当出现健康检查异常时，通常说明您的后端服务器出现了异常，但也有可能是您的健康检查配置不正确导致，本文介绍四层监听（TCP/UDP）健康检查异常排查详细步骤。

1.  确保后端服务器上没有针对100.64.0.0/10地址段进行任何形式的屏蔽，包括iptables或其他任何第三方防火墙/安全策略软件。 

    负载均衡SLB通过100.64.0.0/10内部保留地址段中的IP地址与后端服务器通信，如被屏蔽则会导致健康检查异常，负载均衡无法正常工作。

2.  执行telnet命令，探测后端服务器。 
    1.  登录负载均衡控制台，查看健康检查配置。 

        其中，**健康检查端口**默认使用后端服务器端口进行健康检查，也可以手动设置端口。此处示例使用后端服务器端口，端口号为80。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65040/154358805133070_zh-CN.png)

    2.  执行如下命令，尝试连接健康检查端口，负载均衡上配置的健康检查端口要与后端服务器上的监听的端口保持一致。 

        `telnet 172.17.58.131 80`

        此处172.17.58.131为后端服务器的内网IP地址，80为健康检查端口，如保持默认健康检查端口设置，则使用后端服务器的端口，请根据实际情况配置。

        -   正常情况下，会返回类似`Connected to xxx.xxx.xxx.xxx`信息，表示后端服务器上指定端口处于正常工作（监听）状态，此时健康检查是正常的，如下图所示。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65040/154358805133071_zh-CN.png)

        -   异常示例：假设负载均衡上的监听配置保持不变，但是停止后端服务器上的80端口监听进程，执行telnet命令后，系统提示无法连接到该主机，连接被拒绝，表示80端口监听的进程不再工作，此时健康检查会出现异常，如下图所示。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65040/154358805133072_zh-CN.png)

3.  四层监听支持HTTP方式健康检查，如果使用HTTP方式健康检查，请参考[七层监听（HTTP/HTTPS）健康检查异常排查](intl.zh-CN/.md#)，排查方式与七层监听一致。 

