# [jaeles](https://github.com/jaeles-project/jaeles)代码分析

本文将从代码层面深入分析jaeles项目。jaeles是j3ssie开发的用于自动化 Web 应用程序测试的瑞士军刀。可以将Jaeles 轻松集成到侦察工作流程中。

本文创建于2022年11月13日，最近的一次更新时间为2022年11月13日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  main.go
│    
├─cmd
│      config.go
│      report.go
│      root.go
│      scan.go
│      server.go
│      server_test.go
│      
├─core
│      analyze.go
│      background.go
│      conclusions.go
│      config.go
│      detector.go
│      dns.go
│      filter.go
│      generator.go
│      generator_test.go
│      middleware.go
│      middleware_test.go
│      output.go
│      parser.go
│      parser_test.go
│      passive.go
│      passive_test.go
│      report.go
│      report_test.go
│      routine.go
│      runner.go
│      runner_test.go
│      sender_test.go
│      sending.go
│      signature.go
│      template.go
│      template_test.go
│      update.go
│      variables.go
│      variables_test.go
│      
├─database
│  │  collaborator.go
│  │  config.go
│  │  connect.go
│  │  dnsbin.go
│  │  record.go
│  │  scan.go
│  │  sign.go
│  │  user.go
│  │  
│  └─models
│          collab.go
│          configuration.go
│          dummy.go
│          model.go
│          oob.go
│          record.go
│          scan.go
│          signature.go
│          user.go
│          
├─dns
│      query.go
│      query_test.go
│      
├─libs
│      banner.go
│      dns.go
│      http.go
│      options.go
│      passive.go
│      signature.go
│      version.go
│      
├─sender
│      checksum.go
│      chrome.go
│      sender.go
│      
├─server
│      api.go
│      controllers.go
│      routers.go
│      
├─test-signatures
│      common-error-header.yaml
│      common-error.yaml
│      local-analyze.yaml
│      query-fuzz.yaml
│      routine-simple.yaml
│      simple-dns.yaml
│      with-check-request.yaml
│      with-origin.yaml
│      with-passive-in-dection.yaml
│      with-passive.yaml
│      with-prefix.yaml
│      
└─utils
        helper.go
        log.go
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/bzip2](https://pkg.go.dev/compress/bzip2)
- [ ] [compress/flate](https://pkg.go.dev/compress/flate)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
- [ ] [compress/zlib](https://pkg.go.dev/compress/zlib)
- [ ] [container/list](https://pkg.go.dev/container/list)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/sha1](https://pkg.go.dev/crypto/sha1)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [database/sql/driver](https://pkg.go.dev/database/sql/driver)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [html/template](https://pkg.go.dev/html/template)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/fs](https://pkg.go.dev/io/fs)
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/exec](https://pkg.go.dev/os/exec)
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [text/template](https://pkg.go.dev/text/template)
- [ ] [time](https://pkg.go.dev/time)

## 03-第三方库

- [ ] https://github.com/andybalholm/cascadia
- [ ] https://github.com/appleboy/gin-jwt/v2
- [ ] https://github.com/chromedp/cdproto
- [ ] https://github.com/chromedp/chromedp
- [ ] https://github.com/chromedp/sysutil
- [ ] https://github.com/dgrijalva/jwt-go
- [ ] https://github.com/emirpasic/gods
- [ ] https://github.com/fatih/color
- [ ] https://github.com/fsnotify/fsnotify
- [ ] https://github.com/gin-contrib/cors
- [ ] https://github.com/gin-contrib/sse
- [ ] https://github.com/gin-contrib/static
- [ ] https://github.com/gin-gonic/gin
- [ ] https://github.com/gobwas/httphead
- [ ] https://github.com/gobwas/pool
- [ ] https://github.com/gobwas/ws
- [ ] https://github.com/gobwas/ws/wsutil
- [ ] https://github.com/golang/protobuf/proto
- [ ] https://github.com/google/uuid
- [ ] https://github.com/go-playground/locales
- [ ] https://github.com/go-playground/universal-translator
- [ ] https://github.com/go-playground/validator/v10
- [ ] https://github.com/go-resty/resty/v2
- [ ] https://github.com/gorilla/websocket
- [ ] https://github.com/hashicorp/hcl
- [ ] https://github.com/huandu/xstrings
- [ ] https://github.com/imdario/mergo
- [ ] https://github.com/inconshreveable/mousetrap
- [ ] https://github.com/jbenet/go-context/io
- [ ] https://github.com/Jeffail/gabs/v2
- [ ] https://github.com/jinzhu/copier
- [ ] https://github.com/jinzhu/gorm
- [ ] https://github.com/jinzhu/gorm/dialects/sqlite
- [ ] https://github.com/jinzhu/inflection
- [ ] https://github.com/josharian/intern
- [ ] https://github.com/json-iterator/go
- [ ] https://github.com/kevinburke/ssh_config
- [ ] https://github.com/leodido/go-urn
- [ ] https://github.com/lixiangzhong/dnsutil
- [ ] https://github.com/logrusorgru/aurora/v3
- [ ] https://github.com/magiconair/properties
- [ ] https://github.com/mailru/easyjson
- [ ] https://github.com/Masterminds/goutils
- [ ] https://github.com/Masterminds/semver/v3
- [ ] https://github.com/Masterminds/sprig/v3
- [ ] https://github.com/mattn/go-colorable
- [ ] https://github.com/mattn/go-isatty
- [ ] https://github.com/mattn/go-sqlite3
- [ ] https://github.com/mgutz/ansi
- [ ] https://github.com/miekg/dns
- [ ] https://github.com/mitchellh/copystructure
- [ ] https://github.com/mitchellh/go-homedir
- [ ] https://github.com/mitchellh/mapstructure
- [ ] https://github.com/mitchellh/reflectwalk
- [ ] https://github.com/modern-go/concurrent
- [ ] https://github.com/modern-go/reflect2
- [ ] https://github.com/panjf2000/ants
- [ ] https://github.com/pelletier/go-toml
- [ ] https://github.com/PuerkitoBio/goquery
- [ ] https://github.com/robertkrimen/otto
- [ ] https://github.com/sergi/go-diff/diffmatchpatch
- [ ] https://github.com/shopspring/decimal
- [ ] https://github.com/sirupsen/logrus
- [ ] https://github.com/spf13/afero
- [ ] https://github.com/spf13/afero/mem
- [ ] https://github.com/spf13/cast
- [ ] https://github.com/spf13/cobra
- [ ] https://github.com/spf13/jwalterweatherman
- [ ] https://github.com/spf13/pflag
- [ ] https://github.com/spf13/viper
- [ ] https://github.com/src-d/gcfg
- [ ] https://github.com/src-d/gcfg/scanner
- [ ] https://github.com/src-d/gcfg/token
- [ ] https://github.com/src-d/gcfg/types
- [ ] https://github.com/subosito/gotenv
- [ ] https://github.com/thoas/go-funk
- [ ] https://github.com/ugorji/go/codec
- [ ] https://github.com/xanzy/ssh-agent
- [ ] https://github.com/x-cray/logrus-prefixed-formatter
- [ ] https://gopkg.in/ini.v1
- [ ] https://gopkg.in/sourcemap.v1
- [ ] https://gopkg.in/src-d/go-billy.v4
- [ ] https://gopkg.in/src-d/go-git.v4
- [ ] https://gopkg.in/warnings.v0
- [ ] https://gopkg.in/yaml.v2

## 04-开发设计

本部分尽可能的列举出jaeles开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Erffuf