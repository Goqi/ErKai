# [afrog](https://github.com/zan8in/afrog)代码分析

本文将从代码层面深入分析afrog项目。afrog是zan8in开发的一款性能卓越、快速稳定、PoC 可定制的漏洞扫描工具，PoC 包含 CVE、CNVD、默认口令、信息泄露、指纹识别、未授权访问、任意文件读取、命令执行等多种漏洞类型，帮助网络安全从业者快速验证并及时修复漏洞。。

本文创建于2022年11月11日，最近的一次更新时间为2022年11月12日。

- [01-项目结构]()
- [02-官方包库]()
- [02-第三方库]()
- [03-代码分析]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
├─cmd
│  └─afrog
│          main.go
│      
├─internal
│  └─runner
│          banner.go
│          runner.go
│          
├─pkg
│  ├─catalog
│  │      catalog.go
│  │      find.go
│  │      folder.go
│  │      path.go
│  │      
│  ├─config
│  │      config.go
│  │      options.go
│  │      
│  ├─core
│  │      cel.go
│  │      celcompile.go
│  │      celprogram.go
│  │      checker.go
│  │      engine.go
│  │      excute.go
│  │      result.go
│  │      workpool.go
│  │      
│  ├─errors
│  │      errors.go
│  │      
│  ├─fingerprint
│  │      fingerprint.go
│  │      web_fingerprint_v3.json
│  │      
│  ├─gopoc
│  │      gopoc.go
│  │      memcache.go
│  │      mongodb.go
│  │      redis.go
│  │      rsync.go
│  │      tomcat.go
│  │      zookeeper.go
│  │      
│  ├─html
│  │      html.go
│  │      
│  ├─log
│  │      color.go
│  │      zap.go
│  │      
│  ├─poc
│  │      poc.go
│  │      
│  ├─proto
│  │      http.pb.go
│  │      http.proto
│  │      
│  ├─protocols
│  │  ├─http
│  │  │      http.go
│  │  │      
│  │  └─raw
│  │          http.go
│  │          raw.go
│  │          
│  ├─scan
│  │      cdn.go
│  │      ips.go
│  │      ports.go
│  │      scan.go
│  │      
│  ├─targets
│  │      targets.go
│  │      
│  ├─upgrade
│  │      upgrade.go
│  │      
│  └─utils
│          file.go
│          iconhash.go
│          rand.go
│          severity.go
│          stringslice.go
│          syncfile.go
│          targetLive.go
│          tcp.go
│          tips.go
│          unzip.go
│          utils.go
│          version.go
│          
└─pocs
    │  poc.go
    │  
    ├─afrog-pocs
```

## 02-官方包库

- [ ] [archive/zip](https://pkg.go.dev/archive/zip)
- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [database/sql/driver](https://pkg.go.dev/database/sql/driver)
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [hash](https://pkg.go.dev/hash)
- [ ] [html](https://pkg.go.dev/html)
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
- [ ] [net/netip](https://pkg.go.dev/net/netip)
- [ ] [net/textproto](https://pkg.go.dev/net/textproto)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/user](https://pkg.go.dev/os/user)
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [runtime](https://pkg.go.dev/runtime)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [sync/atomic](https://pkg.go.dev/sync/atomic)
- [ ] [syscall](https://pkg.go.dev/syscall)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode](https://pkg.go.dev/unicode)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] https://github.com/axgle/mahonia | Go 中实现的字符集转换库
- [ ] https://github.com/cavaliergopher/grab | 下载管理器/程序自动更新
- [ ] https://github.com/dlclark/regexp2 | 全功能正则表达式
- [ ] https://github.com/google/cel-go | 通用表达式语言
- [ ] https://github.com/gookit/color| CLI控制台颜色渲染工具
- [ ] https://github.com/karrick/godirwalk | 快速目录遍历
- [ ] https://github.com/natefinch/lumberjack | 日志滚动包
- [ ] https://github.com/panjf2000/ants | 是一个高性能低损耗的goroutine池
- [ ] https://github.com/pkg/errors | 简单的错误处理
- [ ] https://github.com/projectdiscovery/rawhttp | HTTP请求库
- [ ] https://github.com/remeh/sizedwaitgroup | goroutine相关
- [ ] https://github.com/urfave/cli | 命令行控制
- [ ] https://github.com/valyala/fasthttp | HTTP请求处理
- [ ] https://go.uber.org/zap | 快速、结构化、分级的日志记录
- [ ] https://gopkg.in/yaml.v2 | yaml文件解析
- [ ] https://github.com/spaolacci/murmur3

## 04-开发设计

本部分尽可能的列举出afrog开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型
- [ ] 函数方法
- [ ] 并发协程

项目亮点：

- [ ] Poc更新较快
- [ ] Poc静态资源打包
- [ ] 指纹识别-iconhash计算

## 05-不足之处

- 命令行混乱-优化命令行参数输入
- poc支持较少-优化大量poc

## 06-二开计划

- https://github.com/Goqi/Erafrog