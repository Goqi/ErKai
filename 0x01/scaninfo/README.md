# [scaninfo](https://github.com/redtoolskobe/scaninfo)代码分析

本文将从代码层面深入分析scaninfo项目。scaninfo是redtoolskobe开发的一款开源、轻量、快速、跨平台的红队内外网打点扫描器。包含更好的web探测、指纹识别和更好的报告输出。

本文创建于2022年11月13日，最近的一次更新时间为2022年11月13日。

- [01-项目结构]()
- [02-官方包库]()
- [02-第三方库]()
- [03-代码分析]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
├─cmd //程序入口主函数
│      main.go
│      
├─finger
│  │  run.go
│  │  
│  ├─gonmap
│  │      type-httpfinger.go
│  │      
│  ├─lib
│  │  ├─chinese
│  │  │      chinese.go
│  │  │      
│  │  ├─httpfinger
│  │  │      type-faviconHash.go
│  │  │      type-keywordFinger.go
│  │  │      
│  │  ├─iconhash
│  │  │      iconhash.go
│  │  │      
│  │  ├─misc
│  │  │      misc.go
│  │  │      
│  │  ├─shttp
│  │  │      shttp.go
│  │  │      
│  │  └─slog
│  │          slog.go
│  │          
│  └─urlparse
│          urlparse.go
│          
├─global
│      common.go
│      
├─imcp // imcp
│      icmp.go
│          
├─model
│      finger.go
│      result.go
│      
├─pkg
│  ├─common
│  │  │  constant.go
│  │  │  engine.TXT
│  │  │  log.go
│  │  │  parser.go
│  │  │  parser_test.go
│  │  │  Progress.go
│  │  │  service.go
│  │  │  service_test.go
│  │  │  vulconstant.go
│  │  │  
│  │  ├─ipparser
│  │  │      ipprocess.go
│  │  │      ipprocess_test.go
│  │  │      
│  │  └─rangectl
│  │          range.go
│  │          range_test.go
│  │          
│  ├─conversion
│  │      conversion.go
│  │      
│  ├─Ginfo
│  │  ├─Ghttp
│  │  │      encodings.go
│  │  │      Ghttp.go
│  │  │      title.go
│  │  │      
│  │  └─Gnbtscan
│  │          Gnbtscan_test.go
│  │          GNetBIOS.go
│  │          
│  ├─log
│  │      zap.go
│  │      
│  ├─options
│  │      options.go
│  │      
│  ├─output
│  │      output.go
│  │      
│  ├─Plugins
│  │      base.go
│  │      CVE-2020-0796.go
│  │      findnet.go
│  │      ftp.go
│  │      memcached.go
│  │      mongodb.go
│  │      ms17017.go
│  │      mssql.go
│  │      mysql.go
│  │      NetBIOS.go
│  │      postgres.go
│  │      redis.go
│  │      smb.go
│  │      ssh.go
│  │      webtitle.go
│  │      
│  └─report
│          report.go
│          
├─port
│  │  build.sh
│  │  buildAll.sh
│  │  
│  ├─options
│  │      options.go
│  │      
│  └─runner
│          engine.go
│          scanner.go
│          worker.go
│          
├─run
│      scan.go
│      
├─scanvul
│  ├─cmd
│  │      run.txt
│  │      ScanVul.go
│  │      ScanVul.TXT
│  │      
│  └─utils
│          IsContain.go
│          
└─utils
        GetFingerList.go
        IsContain.go
        Redurl.go
```

## 02-官方包库

- [ ] [archive/zip](https://pkg.go.dev/archive/zip)
- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [crypto/x509](https://pkg.go.dev/crypto/x509)
- [ ] [database/sql/driver](https://pkg.go.dev/database/sql/driver)
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [math/big](https://pkg.go.dev/math/big)
- [ ] [math/rand](https://pkg.go.dev/math/rand)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/exec](https://pkg.go.dev/os/exec)
- [ ] [os/signal](https://pkg.go.dev/os/signal)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [runtime](https://pkg.go.dev/runtime)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [sync/atomic](https://pkg.go.dev/sync/atomic)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)

## 03-第三方库

- [ ] https://github.com/denisenkom/go-mssqldb | MsSQL驱动
- [ ] https://github.com/gookit/color | CLI 控制台颜色渲染工具库
- [ ] https://github.com/go-sql-driver/mysql | MySQL驱动
- [ ] https://github.com/jlaffaye/ftp | FTP驱动
- [ ] https://github.com/lib/pq | Postgres驱动
- [ ] https://github.com/projectdiscovery/clistats | 用于简单显示统计信息的基于命令的包
- [ ] https://github.com/projectdiscovery/gologger | 日志库
- [ ] https://github.com/pterm/pterm | 美化控制台输出
- [ ] https://github.com/PuerkitoBio/goquery | HTML解析
- [ ] https://github.com/stacktitan/smb | SMB驱动
- [ ] https://github.com/twmb/murmur3 | Hash算法
- [ ] https://github.com/xuri/excelize/v2 | Excel输出
- [ ] https://go.uber.org/ratelimit | 速率限制
- [ ] https://go.uber.org/zap | 日志包

## 04-开发设计

本部分尽可能的列举出scaninfo开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Erscaninfo