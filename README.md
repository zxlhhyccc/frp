# frp搭建及使用
`````
https://github.com/fatedier/frp/releases
`````
一、服务端配置：
--------
用一键脚本搭建，搭建后修改frpc.ini，将默认全部删除，加入自己的配置：

[common]

bind_addr = 0.0.0.0

bind_port = 7000

kcp_bind_port = 7000

bind_udp_port = 7001

token = www.nat.ee

vhost_http_port = 8080

vhost_https_port = 443

allow_ports = 10001-19999

#subdomain_host = nat.ee

max_pool_count = 6

max_ports_per_client = 3

tcp_mux=true

heartbeat_timeout = 90

authentication_timeout = 900

#[admin]

dashboard_port = 7500

dashboard_user = admin

dashboard_pwd = admin
#[log]

#log_file = ./frps.log

log_level = info

log_max_days = 7

把 443 和 token 改下就可以,
其中：把443改成其他端口（比如4443），为了反向代理使用;token=后面的连接密码改成其他的（如123456，并给客户端使用）

再启动frpc，启动命令有四种方式：

1、一键脚本列出的方式，frps restart

2、./frps -c ./frps.ini

3、nohup ./frps -c ./frps.ini &

4、screen ./frpc -c frpc.ini

二、客户端配置：
-------
在路由、linux系统或电脑端加入下列参数：

# 主配置

[common]

server_addr = xxx.cf

server_port = 7000

token = 123456

user = xxx              #“xxx”为登录标识，可以任意

# 日志

log_file = ./frpc.log

log_level = info

log_max_days = 7


[web]

type = http

local_port = 80

local_ip = 127.0.0.1

custom_domains = xxx.cf , s.xxx.cf


[ssh]

type = tcp

local_port = 22

remote_port = 11122

local_ip = 127.0.0.1

custom_domains = s.xxx.cf

这里的服务器域名在域名里只要二级域名CNAME即可，可以任意添加域名名称，

如：名称PC，域名为：xxx.cf,使用时将xxx.cf改成相应的pc.xxx.cf,就可以进行内网穿透

 使用域名s.jxv2ray.cf可以同步ssh以tcp模式登录

注：电脑端启动服务命令：

管理员运行cmd，进入frp目录输入命令：.\frpc.exe -c .\frpc.ini 或者frps -c frps.ini &
