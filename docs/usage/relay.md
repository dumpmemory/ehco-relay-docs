# 流量转发使用指南

## 前提条件

为了使用隧道功能，您需要准备至少两台主机，并在这两台主机上安装 ehco。

-   中转机器 A：设定该机器的 IP 地址为 1.1.1.1。
-   落地机器 B：设定该机器的 IP 地址为 2.2.2.2，且在端口 5555 上运行着某个 TCP/UDP 服务。

## 案例介绍

### 案例一：不使用隧道，直接通过中转机器转发用户流量

在中转机器 A 上运行以下命令：

```shell
ehco -l 0.0.0.0:1234 -r 2.2.2.2:5555
```

该命令的作用是将所有从中转机器 A 的 1234 端口接收到的流量直接转发至落地机器 B 的 5555 端口。

通过这种方式，用户可以直接通过中转机器 A 的 1234 端口访问落地机器 B 的 5555 端口上的服务。

### 案例二：使用 mwss 隧道转发用户流量

首先，在落地机器 B 上运行以下命令：

```shell
ehco -l 0.0.0.0:443 -lt mwss -r 127.0.0.1:5555
```

此命令的目的是将所有通过落地机器 B 的 443 端口接收到的 wss 流量解密后转发至同一机器上的 5555 端口。

接着，在中转机器 A 上执行以下命令：

```shell
ehco -l 0.0.0.0:1234 -r wss://2.2.2.2:443 -tt mwss
```

该命令的含义是将所有从中转机器 A 的 1234 端口接收到的流量通过 wss 加密后转发至落地机器 B 的 443 端口。

通过这种设置，用户便能通过中转机器 A 的 1234 端口安全访问落地机器 B 的 5555 端口上的服务。
