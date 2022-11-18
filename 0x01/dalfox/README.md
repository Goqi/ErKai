# [dalfox](https://github.com/hahwul/dalfox)代码分析

本文将从代码层面深入分析dalfox项目。dalfox是hahwul开发的一个功能强大的开源 XSS 扫描工具和参数分析器，可加快检测和验证 XSS 缺陷的过程。它配备了一个强大的测试引擎，许多适合酷黑客的小众功能！

本文创建于2022年11月18日，最近的一次更新时间为2022年11月18日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  dalfox.go
│          
├─cmd
│      file.go
│      payload.go
│      pipe.go
│      root.go
│      server.go
│      sxss.go
│      url.go
│      version.go
│          
├─lib
│      func.go
│      func_test.go
│      interface.go
│      interface_test.go
│      
├─pkg
│  ├─generating
│  │      bulk.go
│  │      
│  ├─model
│  │      options.go
│  │      param.go
│  │      result.go
│  │      
│  ├─optimization
│  │      abstraction.go
│  │      abstraction_test.go
│  │      inspectionParam.go
│  │      inspectionParam_test.go
│  │      optimization.go
│  │      optimization_test.go
│  │      replace.go
│  │      replace_test.go
│  │      
│  ├─printing
│  │      banner.go
│  │      banner_test.go
│  │      logger.go
│  │      logger_test.go
│  │      multispin.go
│  │      multispin_test.go
│  │      util.go
│  │      util_test.go
│  │      version.go
│  │      
│  ├─report
│  │      report.go
│  │      
│  ├─scanning
│  │      bav.go
│  │      codeview.go
│  │      csp.go
│  │      entity.go
│  │      foundaction.go
│  │      grep.go
│  │      headless.go
│  │      ignore.go
│  │      multicast.go
│  │      parameterAnlaysis.go
│  │      payload.go
│  │      poc.go
│  │      queries.go
│  │      ratelimit.go
│  │      scan.go
│  │      sendReq.go
│  │      staticAnlaysis.go
│  │      transport.go
│  │      utils.go
│  │      waf.go
│  │      
│  ├─server
│  │  │  model.go
│  │  │  scan.go
│  │  │  server.go
│  │  │  utils.go
│  │  │  
│  │  └─docs
│  │          docs.go
│  │          swagger.json
│  │          swagger.yaml
│  │          
│  └─verification
│          verify.go
│          verify_test.go
│          
├─samples
│      sample_config.json
│      sample_custompayload.txt
│      sample_found_action.sh
│      sample_grep.json
│      sample_lib.go.txt
│      sample_rawdata.txt
│      sample_target.txt
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/sha256](https://pkg.go.dev/crypto/sha256)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [html](https://pkg.go.dev/html)
- [ ] [html/template](https://pkg.go.dev/html/template)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/fs](https://pkg.go.dev/io/fs)
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/http/httputil](https://pkg.go.dev/net/http/httputil)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/exec](https://pkg.go.dev/os/exec)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [text/javascript](https://pkg.go.dev/text/javascript)
- [ ] [text/plain](https://pkg.go.dev/text/plain)
- [ ] [text/css](https://pkg.go.dev/text/css)
- [ ] [time](https://pkg.go.dev/time)

## 03-第三方库

- [ ] https://github.com/hahwul/volt | 用于快速制作渗透测试工具的 Golang 库
- [ ] https://github.com/labstack/echo | 高性能、极简的 Go Web 框架
- [ ] https://github.com/spf13/cobra | 命令行操作
- [ ] https://github.com/alecthomas/template | text/template换行省略
- [ ] https://github.com/andybalholm/cascadia | CSS 选择器库
- [ ] https://github.com/antonfisher/nested-logrus-formatter | 日志程序
- [ ] https://github.com/briandowns/spinner | 90个可配置的终端微调器和进度指示器
- [ ] https://github.com/chromedp/cdproto | Chrome相关
- [ ] https://github.com/chromedp/chromedp | Chrome相关
- [ ] https://github.com/chromedp/sysutil | 跨平台系统实用程序
- [ ] https://github.com/fatih/color | Go（golang）的颜色包
- [ ] https://github.com/gobwas/httphead | HTTP 标头值解析库
- [ ] https://github.com/gobwas/pool | Go Pool 助手
- [ ] https://github.com/gobwas/ws | Go 的微型 WebSocket 库
- [ ] https://github.com/golang-jwt/jwt | 一个签名的 JSON 对象
- [ ] https://github.com/go-openapi/jsonpointer | 支持结构的 gojsonpointer 分支
- [ ] https://github.com/go-openapi/jsonreference | 支持结构的 gojsonreference 分支
- [ ] https://github.com/go-openapi/spec | openapi规范对象模型
- [ ] https://github.com/go-openapi/swag | 在 go-openapi 项目中使用
- [ ] https://github.com/inconshreveable/mousetrap | 从 Windows 资源管理器开始检测
- [ ] https://github.com/josharian/intern | 实习生 Go 字符串
- [ ] https://github.com/KyleBanks/depth | 可视化 Go 依赖树
- [ ] https://github.com/labstack/gommon | Go的常用包
- [ ] https://github.com/logrusorgru/aurora | 支持 Printf/Sprintf 方法的 Golang 终极 ANSI 颜色
- [ ] https://github.com/mailru/easyjson | golang 的快速 JSON 序列化程序
- [ ] https://github.com/mattn/go-colorable | 适用于 Windows 的着色器
- [ ] https://github.com/mattn/go-isatty | golang 的 isatty
- [ ] https://github.com/mattn/go-runewidth | golang 的 wcwidth
- [ ] https://github.com/olekukonko/tablewriter | 输出格式化表格
- [ ] https://github.com/PuerkitoBio/goquery | 有点像那个 j-thing，只在 Go 中。
- [ ] https://github.com/PuerkitoBio/purell | 用于标准化 URL 的小型 Go 库
- [ ] https://github.com/PuerkitoBio/urlesc | 根据 RFC3986 转义正确的 URL
- [ ] https://github.com/sirupsen/logrus | 日志库
- [ ] https://github.com/swaggo/echo-swagger | Swagger相关
- [ ] https://github.com/swaggo/files| Swagger相关
- [ ] https://github.com/swaggo/swag| Swagger相关
- [ ] https://github.com/tylerb/graceful | 关闭http.Handler
- [ ] https://github.com/valyala/bytebufferpool | 防内存浪费字节缓冲池
- [ ] https://github.com/valyala/fasttemplate | 简单快速的 Go 模板引擎
- [ ] https://gopkg.in/yaml.v2 | yaml操作

## 04-开发设计

本部分尽可能的列举出dalfox开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型
- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

- UA不是随机生成

## 06-二开计划

- https://github.com/Goqi/Erdalfox