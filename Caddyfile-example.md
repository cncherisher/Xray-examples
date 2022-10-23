# global options block 全局属性

```
{
	# General Options
	debug
	http_port    <port>
	https_port   <port>
	default_bind <hosts...>
	order <dir1> first|last|[before|after <dir2>]
	storage <module_name> {
		<options...>
	}
	storage_clean_interval <duration>
	renew_interval <duration>
	ocsp_interval  <duration>
	admin   off|<addr> {
		origins <origins...>
		enforce_origin
	}
	log [name] {
		output  <writer_module> ...
		format  <encoder_module> ...
		level   <level>
		include <namespaces...>
		exclude <namespaces...>
	}
	grace_period   <duration>
	shutdown_delay <duration>

	# TLS Options
	auto_https off|disable_redirects|ignore_loaded_certs|disable_certs
	email <yours>
	default_sni <name>
	local_certs
	skip_install_trust
	acme_ca <directory_url>
	acme_ca_root <pem_file>
	acme_eab <key_id> <mac_key>
	acme_dns <provider> ...
	on_demand_tls {
		ask      <endpoint>
		interval <duration>
		burst    <n>
	}
	key_type ed25519|p256|p384|rsa2048|rsa4096
	cert_issuer <name> ...
	ocsp_stapling off
	preferred_chains [smallest] {
		root_common_name <common_names...>
		any_common_name  <common_names...>
	}

	# Server Options
	servers [<listener_address>] {
		listener_wrappers {
			<listener_wrappers...>
		}
		timeouts {
			read_body   <duration>
			read_header <duration>
			write       <duration>
			idle        <duration>
		}
		metrics
		max_header_size <size>
		log_credentials
		protocols [h1|h2|h2c|h3]
		strict_sni_host [on|insecure_off]
	}

	# PKI Options
	pki {
		ca [<id>] {
			name            <name>
			root_cn         <name>
			intermediate_cn <name>
			root {
				format <format>
				cert   <path>
				key    <path>
			}
			intermediate {
				format <format>
				cert   <path>
				key    <path>
			}
		}
	}

	# Event options
	events {
		on <event> <handler...>
	}
}
```

# 可复用片段，用小括号括起来

```
(redirect) {
	@http {
		protocol http
	}
	redir @http https://{host}{uri}
}
```

# 站点块

```
example.com { # Addresses 位于站点块的头部
	@grpc {
		protocol grpc
		path /xxx
	}
	reverse_proxy @grpc unix//dev/shm/Xray-Trojan-gRPC.socket {
		transport http {
			versions h2c
		}
	}
	root * /var/www
	file_server
}
```

## 监听地址举例

```
localhost
example.com
:443
http://example.com
localhost:8080
127.0.0.1
[::1]:2015
*.example.com
http://
```

## 指令，必须位于站点块内
指令 | 描述
----------|------------
**[abort](https://caddyserver.com/docs/caddyfile/directives/abort)** | Aborts the HTTP request
**[acme_server](https://caddyserver.com/docs/caddyfile/directives/acme_server)** | An embedded ACME server
**[basicauth](https://caddyserver.com/docs/caddyfile/directives/basicauth)** | Enforces HTTP Basic Authentication
**[bind](https://caddyserver.com/docs/caddyfile/directives/bind)** | Customize the server's socket address
**[encode](https://caddyserver.com/docs/caddyfile/directives/encode)** | Encodes (usually compresses) responses
**[error](https://caddyserver.com/docs/caddyfile/directives/error)** | Trigger an error
**[file_server](https://caddyserver.com/docs/caddyfile/directives/file_server)** | Serve files from disk
**[forward_auth](https://caddyserver.com/docs/caddyfile/directives/forward_auth)** | Delegate authentication to an external service
**[handle](https://caddyserver.com/docs/caddyfile/directives/handle)** | A mutually-exclusive group of directives
**[handle_errors](https://caddyserver.com/docs/caddyfile/directives/handle_errors)** | Defines routes for handling errors
**[handle_path](https://caddyserver.com/docs/caddyfile/directives/handle_path)** | Like handle, but strips path prefix
**[header](https://caddyserver.com/docs/caddyfile/directives/header)** | Sets or removes response headers
**[import](https://caddyserver.com/docs/caddyfile/directives/import)** | Include snippets or files
**[log](https://caddyserver.com/docs/caddyfile/directives/log)** | Enables access/request logging
**[map](https://caddyserver.com/docs/caddyfile/directives/map)** | Maps an input value to one or more outputs
**[method](https://caddyserver.com/docs/caddyfile/directives/method)** | Change the HTTP method internally
**[metrics](https://caddyserver.com/docs/caddyfile/directives/metrics)** | Configures the Prometheus metrics exposition endpoint
**[php_fastcgi](https://caddyserver.com/docs/caddyfile/directives/php_fastcgi)** | Serve PHP sites over FastCGI
**[push](https://caddyserver.com/docs/caddyfile/directives/push)** | Push content to the client using HTTP/2 server push
**[redir](https://caddyserver.com/docs/caddyfile/directives/redir)** | Issues an HTTP redirect to the client
**[request_body](https://caddyserver.com/docs/caddyfile/directives/request_body)** | Manipulates request body
**[request_header](https://caddyserver.com/docs/caddyfile/directives/request_header)** | Manipulates request headers
**[respond](https://caddyserver.com/docs/caddyfile/directives/respond)** | Writes a hard-coded response to the client
**[reverse_proxy](https://caddyserver.com/docs/caddyfile/directives/reverse_proxy)** | A powerful and extensible reverse proxy
**[rewrite](https://caddyserver.com/docs/caddyfile/directives/rewrite)** | Rewrites the request internally
**[root](https://caddyserver.com/docs/caddyfile/directives/root)** | Set the path to the site root
**[route](https://caddyserver.com/docs/caddyfile/directives/route)** | A group of directives treated literally as single unit
**[skip_log](https://caddyserver.com/docs/caddyfile/directives/skip_log)** | Skip access logging for matched requests
**[templates](https://caddyserver.com/docs/caddyfile/directives/templates)** | Execute templates on the response
**[tls](https://caddyserver.com/docs/caddyfile/directives/tls)** | Customize TLS settings
**[tracing](https://caddyserver.com/docs/caddyfile/directives/tracing)** | Integration with OpenTelemetry tracing
**[try_files](https://caddyserver.com/docs/caddyfile/directives/try_files)** | Rewrite that depends on file existence
**[uri](https://caddyserver.com/docs/caddyfile/directives/uri)** | Manipulate the URI
**[vars](https://caddyserver.com/docs/caddyfile/directives/vars)** | Set arbitrary variables

## matcher 匹配器
eg:
```
* :匹配全部
/path :匹配某个路径
@name :自定义匹配规则
```
```
@grpc {
			protocol grpc
			path /xxx 
		}
```
```
protocol 可选http|https|grpc|http/<version>[+]
```

## 常用指令
```
route

```
```
reverse_proxy #反向代理
eg:
reverse_proxy @grpc unix//dev/shm/Xray-Trojan-gRPC.socket {
			transport http {
				versions h2c
			}
		}
```
```
格式
reverse_proxy [<matcher>] [<upstreams...>] {}
上游地址可选:
localhost:4000
127.0.0.1:4000
http://localhost:4000
https://example.com
h2c://127.0.0.1
example.com
unix//var/php.sock
unix+h2c//var/grpc.sock
```