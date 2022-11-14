# [ffuf](https://github.com/joohoi/ffuf)代码分析

本文将从代码层面深入分析ffuf项目。ffuf是joohoi开发的一款快速fuzz工具。包含目录扫描、虚拟主机发现、Get和Post参数模糊测试等。

本文创建于2022年11月13日，最近的一次更新时间为2022年11月13日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  help.go
│  main.go
│      
└─pkg
    ├─ffuf
    │      autocalibration.go
    │      config.go
    │      interfaces.go
    │      job.go
    │      multierror.go
    │      optionsparser.go
    │      optionsparser_test.go
    │      optrange.go
    │      progress.go
    │      rate.go
    │      request.go
    │      request_test.go
    │      response.go
    │      util.go
    │      util_test.go
    │      valuerange.go
    │      version.go
    │      
    ├─filter
    │      filter.go
    │      filter_test.go
    │      lines.go
    │      lines_test.go
    │      regex.go
    │      regexp_test.go
    │      size.go
    │      size_test.go
    │      status.go
    │      status_test.go
    │      time.go
    │      time_test.go
    │      words.go
    │      words_test.go
    │      
    ├─input
    │      command.go
    │      const.go
    │      const_windows.go
    │      input.go
    │      wordlist.go
    │      wordlist_test.go
    │      
    ├─interactive
    │      posix.go
    │      termhandler.go
    │      windows.go
    │      
    ├─output
    │      const.go
    │      const_windows.go
    │      file_csv.go
    │      file_csv_test.go
    │      file_html.go
    │      file_json.go
    │      file_md.go
    │      output.go
    │      stdout.go
    │      
    └─runner
            runner.go
            simple.go
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [container/ring](https://pkg.go.dev/container/ring)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/csv](https://pkg.go.dev/encoding/csv)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [html/template](https://pkg.go.dev/html/template)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [math/rand](https://pkg.go.dev/math/rand)

- [ ] [net](https://pkg.go.dev/net)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/http/httputil](https://pkg.go.dev/net/http/httputil)
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/textproto](https://pkg.go.dev/net/textproto)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/signal](https://pkg.go.dev/os/signal)
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [runtime](https://pkg.go.dev/runtime)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [syscall](https://pkg.go.dev/syscall)
- [ ] [time](https://pkg.go.dev/time)

## 03-第三方库

- [ ] https://github.com/pelletier/go-toml | TOML 文件格式的 Go 库

## 04-开发设计

本部分尽可能的列举出ffuf开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Erffuf