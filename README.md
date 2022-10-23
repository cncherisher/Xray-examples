
# Xray-examples

Forked from [v2ray-examples](https://github.com/v2fly/v2ray-examples)

## Xray-install 一键安装脚本

```bash
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install -u root
```
## 删除Xray

```bash
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ remove --purge
```
## 以多配置启动 Xray：

```bash
$ xray run -confdir /usr/local/etc/xray/conf
```

## 推荐的多文件列表

```bash
.
├── 00_log.json
├── 01_api.json
├── 02_dns.json
├── 03_routing.json
├── 04_policy.json
├── 05_inbounds.json
├── 06_outbounds.json
├── 07_transport.json
├── 08_stats.json
└── 09_reverse.json

0 directories, 10 files
```
## grpc/ws等带有路径的需要配合nginx/caddy前置分流
## 否则会报 501/404等错误