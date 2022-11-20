# [nps](https://github.com/ehang-io/nps)代码分析

本文将从代码层面深入分析nps项目。nps是ehang-io开发的一款轻量级、高性能、功能强大的内网穿透代理服务器。支持tcp、udp、socks5、http等几乎所有流量转发，可用来访问内网网站、本地支付接口调试、ssh访问、远程桌面，内网dns解析、内网socks5代理等……，并带有功能强大的web管理端。

本文创建于2022年11月20日，最近的一次更新时间为2022年11月20日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
          
├─bridge
│      bridge.go
│      
├─client
│      client.go
│      control.go
│      health.go
│      local.go
│      register.go
│      
├─cmd
│  ├─npc
│  │      npc.go
│  │      sdk.go
│  │      
│  └─nps
│          nps.go
│          
├─conf
│      clients.json
│      hosts.json
│      multi_account.conf
│      npc.conf
│      nps.conf
│      server.key
│      server.pem
│      tasks.json
│      
├─gui
│  └─npc
│          AndroidManifest.xml
│          npc.go
│          
├─image
│      
├─lib
│  ├─cache
│  │      lru.go
│  │      
│  ├─common
│  │      const.go
│  │      logs.go
│  │      netpackager.go
│  │      pool.go
│  │      pprof.go
│  │      run.go
│  │      util.go
│  │      
│  ├─config
│  │      config.go
│  │      config_test.go
│  │      
│  ├─conn
│  │      conn.go
│  │      link.go
│  │      listener.go
│  │      snappy.go
│  │      
│  ├─crypt
│  │      clientHello.go
│  │      crypt.go
│  │      tls.go
│  │      
│  ├─daemon
│  │      daemon.go
│  │      reload.go
│  │      
│  ├─file
│  │      db.go
│  │      file.go
│  │      obj.go
│  │      sort.go
│  │      
│  ├─goroutine
│  │      pool.go
│  │      
│  ├─install
│  │      install.go
│  │      
│  ├─pmux
│  │      pconn.go
│  │      plistener.go
│  │      pmux.go
│  │      pmux_test.go
│  │      
│  ├─rate
│  │      conn.go
│  │      rate.go
│  │      
│  ├─sheap
│  │      heap.go
│  │      
│  └─version
│          version.go
│          
├─server
│  │  server.go
│  │  
│  ├─connection
│  │      connection.go
│  │      
│  ├─proxy
│  │      base.go
│  │      http.go
│  │      https.go
│  │      p2p.go
│  │      socks5.go
│  │      tcp.go
│  │      transport.go
│  │      transport_windows.go
│  │      udp.go
│  │      
│  ├─test
│  │      test.go
│  │      
│  └─tool
│          utils.go
│          
└─web
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [container/list](https://pkg.go.dev/container/list)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/aes](https://pkg.go.dev/crypto/aes)
- [ ] [crypto/des](https://pkg.go.dev/crypto/des)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/rand](https://pkg.go.dev/crypto/rand)
- [ ] [crypto/rc4](https://pkg.go.dev/crypto/rc4)
- [ ] [crypto/rsa](https://pkg.go.dev/crypto/rsa)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [crypto/x509](https://pkg.go.dev/crypto/x509)
- [ ] [crypto/x509/pkix](https://pkg.go.dev/crypto/x509/pkix)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [encoding/pem](https://pkg.go.dev/encoding/pem)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [html](https://pkg.go.dev/html)
- [ ] [html/template](https://pkg.go.dev/html/template)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/fs](https://pkg.go.dev/io/fs)
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [math](https://pkg.go.dev/math)
- [ ] [math/big](https://pkg.go.dev/math/big)
- [ ] [math/bits](https://pkg.go.dev/math/bits)
- [ ] [math/rand](https://pkg.go.dev/math/rand)
- [ ] [net](https://pkg.go.dev/net)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/http/cgi](https://pkg.go.dev/net/http/cgi)
- [ ] [net/http/fcgi](https://pkg.go.dev/net/http/fcgi)
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/http/httputil](https://pkg.go.dev/net/http/httputil)
- [ ] [net/http/internal](https://pkg.go.dev/net/http/internal)
- [ ] [net/http/internal/ascii](https://pkg.go.dev/net/http/internal/ascii)
- [ ] [net/http/pprof](https://pkg.go.dev/net/http/pprof)
- [ ] [net/mail](https://pkg.go.dev/net/mail)
- [ ] [net/netip](https://pkg.go.dev/net/netip)
- [ ] [net/smtp](https://pkg.go.dev/net/smtp)
- [ ] [net/textproto](https://pkg.go.dev/net/textproto)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/exec](https://pkg.go.dev/os/exec)
- [ ] [os/signal](https://pkg.go.dev/os/signal)
- [ ] [os/user](https://pkg.go.dev/os/user)
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [regexp/syntax](https://pkg.go.dev/regexp/syntax)
- [ ] [runtime](https://pkg.go.dev/runtime)
- [ ] [runtime/cgo](https://pkg.go.dev/runtime/cgo)
- [ ] [runtime/debug](https://pkg.go.dev/runtime/debug)
- [ ] [runtime/internal/atomic](https://pkg.go.dev/runtime/internal/atomic)
- [ ] [runtime/internal/math](https://pkg.go.dev/runtime/internal/math)
- [ ] [runtime/internal/sys](https://pkg.go.dev/runtime/internal/sys)
- [ ] [runtime/pprof](https://pkg.go.dev/runtime/pprof)
- [ ] [runtime/trace](https://pkg.go.dev/runtime/trace)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [sync/atomic](https://pkg.go.dev/sync/atomic)
- [ ] [syscall](https://pkg.go.dev/syscall)
- [ ] [text/tabwriter](https://pkg.go.dev/text/tabwriter)
- [ ] [text/template](https://pkg.go.dev/text/template)
- [ ] [text/template/parse](https://pkg.go.dev/text/template/parse)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode](https://pkg.go.dev/unicode)
- [ ] [unicode/utf16](https://pkg.go.dev/unicode/utf16)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] https://fyne.io/fyne | GUI图形化编程
- [ ] https://github.com/astaxie/beego | Web框架
- [ ] https://github.com/c4milo/unpackit
- [ ] https://github.com/ccding/go-stun/stun
- [ ] https://github.com/dsnet/compress
- [ ] https://github.com/fredbi/uri
- [ ] https://github.com/fsnotify/fsnotify
- [ ] https://github.com/go-gl/gl
- [ ] https://github.com/go-gl/glfw
- [ ] https://github.com/goki/freetype
- [ ] https://github.com/golang/snappy
- [ ] https://github.com/go-ole/go-ole
- [ ] https://github.com/kardianos/service
- [ ] https://github.com/klauspost/compress/flate
- [ ] https://github.com/klauspost/cpuid
- [ ] https://github.com/klauspost/pgzip
- [ ] https://github.com/klauspost/reedsolomon
- [ ] https://github.com/panjf2000/ants
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/shiena/ansicolor
- [ ] https://github.com/shirou/gopsutil
- [ ] https://github.com/srwiley/oksvg
- [ ] https://github.com/srwiley/rasterx
- [ ] https://github.com/StackExchange/wmi
- [ ] https://github.com/templexxx/cpufeat
- [ ] https://github.com/templexxx/xor
- [ ] https://github.com/tjfoc/gmsm/sm4
- [ ] https://github.com/ulikunitz/xz
- [ ] https://github.com/xtaci/kcp-go
- [ ] https://gopkg.in/yaml.v2

## 04-开发设计

本部分尽可能的列举出nps开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型
- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Ernps