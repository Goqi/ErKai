# [gopoc](https://github.com/jjf012/gopoc)代码分析

本文将从代码层面深入分析理解gopoc项目。gopoc是jjf012开发的一款基于模板的漏洞扫描工具，用户可以方便快速的进行漏洞扫描。也可以使用YAML文件快速的编写漏洞扫描规则。

本文会持续更新，创建于2021年3月16日，最近的一次更新时间为2022年11月14日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  main.go
│  poc.yaml
│      
├─cmd
│      cmd.go
│      
├─lib
│      check.go
│      eval.go
│      http.go
│      http.pb.go
│      http.proto
│      poc.go
│      
└─utils
        helper.go
        log.go
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [html](https://pkg.go.dev/html)
- [ ] [html/template](https://pkg.go.dev/html/template)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/fs](https://pkg.go.dev/io/fs)
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [math/rand](https://pkg.go.dev/math/rand)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [regexp/syntax](https://pkg.go.dev/regexp/syntax)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [sync/atomic](https://pkg.go.dev/sync/atomic)
- [ ] [time](https://pkg.go.dev/time)

## 03-第三方库

- [ ] https://github.com/urfave/cli | 命令控制
- [ ] https://github.com/antlr/antlr4 | 功能强大的解析器生成器
- [ ] https://github.com/cpuguy83/go-md2man | 将markdown转换为roff
- [ ] https://github.com/fatih/colorr | 命令行彩色包装
- [ ] https://github.com/golang/protobuf | Gadgets的协议缓冲区
- [ ] https://github.com/google/cel-go | 通用表达语言
- [ ] https://github.com/konsorten/go-windows-terminal-sequences
- [ ] https://github.com/mattn/go-colorable | Windows的彩色书写器
- [ ] https://github.com/mattn/go-isatty  | golang 的 isatty
- [ ] https://github.com/mgutz/ansii | 用于创建ANSI彩色的字符串和代码
- [ ] https://github.com/russross/blackfriday | Go 的降价处理器
- [ ] https://github.com/sirupsen/logrus | 日志包
- [ ] https://github.com/thoas/go-funk | 个基于反射的现代 Go 库
- [ ] https://github.com/x-cray/logrus-prefixed-formatter | Logrus前缀日志格式化程序
- [ ] https://github.com/xrash/smetrics | Go 编写的字符串度量库


## 04-开发设计

本部分尽可能的列举出ffuf开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Ergopoc