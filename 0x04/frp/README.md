# [frp](https://github.com/fatedier/frp)代码分析

本文将从代码层面深入分析frp项目。frp是fatedier开发的一款快速反向代理工具，可以将NAT或防火墙后面的本地服务器暴露在互联网上。

本文创建于2022年11月16日，最近的一次更新时间为2022年11月18日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
├─assets
│  │  assets.go
│  │  
│  ├─frpc
│  │  │  embed.go
│  │  └─static
│  │          
│  └─frps
│      │  embed.go
│      └─static
│              
├─client
│  │  admin.go
│  │  admin_api.go
│  │  control.go
│  │  service.go
│  │  visitor.go
│  │  visitor_manager.go
│  │  
│  ├─event
│  │      event.go
│  │      
│  ├─health
│  │      health.go
│  │      
│  └─proxy
│          proxy.go
│          proxy_manager.go
│          proxy_wrapper.go
│          
├─cmd
│  ├─frpc
│  │  │  main.go
│  │  │  
│  │  └─sub
│  │          http.go
│  │          https.go
│  │          reload.go
│  │          root.go
│  │          status.go
│  │          stcp.go
│  │          sudp.go
│  │          tcp.go
│  │          tcpmux.go
│  │          udp.go
│  │          verify.go
│  │          xtcp.go
│  │          
│  └─frps
│          main.go
│          root.go
│          verify.go
│          
├─conf
│      frpc.ini
│      frpc_full.ini
│      frps.ini
│      frps_full.ini
│      
├─pkg
│  ├─auth
│  │      auth.go
│  │      oidc.go
│  │      token.go
│  │      
│  ├─config
│  │      client.go
│  │      client_test.go
│  │      parse.go
│  │      proxy.go
│  │      proxy_test.go
│  │      README.md
│  │      server.go
│  │      server_test.go
│  │      types.go
│  │      types_test.go
│  │      utils.go
│  │      value.go
│  │      visitor.go
│  │      visitor_test.go
│  │      
│  ├─consts
│  │      consts.go
│  │      
│  ├─errors
│  │      errors.go
│  │      
│  ├─metrics
│  │  │  metrics.go
│  │  │  
│  │  ├─aggregate
│  │  │      server.go
│  │  │      
│  │  ├─mem
│  │  │      server.go
│  │  │      types.go
│  │  │      
│  │  └─prometheus
│  │          server.go
│  │          
│  ├─msg
│  │      ctl.go
│  │      msg.go
│  │      
│  ├─nathole
│  │      nathole.go
│  │      
│  ├─plugin
│  │  ├─client
│  │  │      http2https.go
│  │  │      https2http.go
│  │  │      https2https.go
│  │  │      http_proxy.go
│  │  │      plugin.go
│  │  │      socks5.go
│  │  │      static_file.go
│  │  │      unix_domain_socket.go
│  │  │      
│  │  └─server
│  │          http.go
│  │          manager.go
│  │          plugin.go
│  │          tracer.go
│  │          types.go
│  │          
│  ├─proto
│  │  └─udp
│  │          udp.go
│  │          udp_test.go
│  │          
│  ├─transport
│  │      tls.go
│  │      
│  └─util
│      ├─limit
│      │      reader.go
│      │      writer.go
│      │      
│      ├─log
│      │      log.go
│      │      
│      ├─metric
│      │      counter.go
│      │      counter_test.go
│      │      date_counter.go
│      │      date_counter_test.go
│      │      metrics.go
│      │      
│      ├─net
│      │      conn.go
│      │      dial.go
│      │      http.go
│      │      kcp.go
│      │      listener.go
│      │      tls.go
│      │      udp.go
│      │      websocket.go
│      │      
│      ├─tcpmux
│      │      httpconnect.go
│      │      
│      ├─util
│      │      http.go
│      │      util.go
│      │      util_test.go
│      │      
│      ├─version
│      │      version.go
│      │      version_test.go
│      │      
│      ├─vhost
│      │      http.go
│      │      https.go
│      │      https_test.go
│      │      resource.go
│      │      router.go
│      │      vhost.go
│      │      
│      └─xlog
│              ctx.go
│              xlog.go
│              
├─server
│  │  control.go
│  │  dashboard.go
│  │  dashboard_api.go
│  │  service.go
│  │  
│  ├─controller
│  │      resource.go
│  │      
│  ├─group
│  │      group.go
│  │      http.go
│  │      tcp.go
│  │      tcpmux.go
│  │      
│  ├─metrics
│  │      metrics.go
│  │      
│  ├─ports
│  │      ports.go
│  │      
│  ├─proxy
│  │      http.go
│  │      https.go
│  │      proxy.go
│  │      stcp.go
│  │      sudp.go
│  │      tcp.go
│  │      tcpmux.go
│  │      udp.go
│  │      xtcp.go
│  │      
│  └─visitor
│          visitor.go
│              
└─web
    ├─frpc           
    └─frps
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/rand](https://pkg.go.dev/crypto/rand)
- [ ] [crypto/rc4](https://pkg.go.dev/crypto/rc4)
- [ ] [crypto/rsa](https://pkg.go.dev/crypto/rsa)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [crypto/x509](https://pkg.go.dev/crypto/x509)
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/fs](https://pkg.go.dev/io/fs)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [math/big](https://pkg.go.dev/math/big)
- [ ] [math/rand](https://pkg.go.dev/math/rand)
- [ ] [net](https://pkg.go.dev/net)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/http/httputil](https://pkg.go.dev/net/http/httputil)
- [ ] [net/http/pprof](https://pkg.go.dev/net/http/pprof)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/signal](https://pkg.go.dev/os/signal)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [regexp/syntax](https://pkg.go.dev/regexp/syntax)
- [ ] [runtime](https://pkg.go.dev/runtime)
- [ ] [runtime/debug](https://pkg.go.dev/runtime/debug)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [sync/atomic](https://pkg.go.dev/sync/atomic)
- [ ] [syscall](https://pkg.go.dev/syscall)
- [ ] [text/template](https://pkg.go.dev/text/template)
- [ ] [time](https://pkg.go.dev/time)

## 03-第三方库

- [ ] https://gopkg.in/ini.v1 | ini配置文件
- [ ] https://github.com/spf13/cobra | 命令行参数
- [ ] https://github.com/armon/go-socks5 | Golang 中的 SOCKS5 服务器
- [ ] https://github.com/cespare/xxhash | 64 位 xxHash 算法 (XXH64) 的 Go 实现
- [ ] https://github.com/coreos/go-oidc | Go OpenID Connect 客户端
- [ ] https://github.com/fatedier/beego | 高性能Web 框架
- [ ] https://github.com/fatedier/golib | 常见的Go包
- [ ] https://github.com/fatedier/kcp-go | 可靠的UDP包
- [ ] https://github.com/go-playground/validator | 包验证器
- [ ] https://github.com/gorilla/mux | HTTP 路由器和 URL 匹配器
- [ ] https://github.com/hashicorp/yamux | Golang连接复用库
- [ ] https://github.com/matttproud/golang_protobuf_extensions | Protocol Buffer 功能扩展
- [ ] https://github.com/pires/go-proxyproto| PROXY 协议版本 1 和 2的 Go 库实现
- [ ] https://github.com/prometheus/client_golang | Prometheus 检测库
- [ ] https://gopkg.in/square/go-jose.v2| 在 Go 中实现 JOSE 标准（JWE、JWS、JWT）

## 04-开发设计

本部分尽可能的列举出frp开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型
- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

- socks5第三方包年代比较久远

## 06-二开计划

- https://github.com/Goqi/Erfrp