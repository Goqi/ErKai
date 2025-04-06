# [httpx](https://github.com/projectdiscovery/httpx)代码分析

本文将从代码层面深入分析httpx项目。httpx是projectdiscovery项目开发的一款快速且多用途的 HTTP 工具包，它允许使用 retryablehttp 库运行多个探测。

本文创建于2022年11月15日，最近的一次更新时间为2022年11月15日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
├─cmd
│  ├─functional-test
│  │  │  main.go
│  │  │  run.sh
│  │  │  testcases.txt
│  │  │  
│  │  └─test-data
│  │          raw-request.txt
│  │          request.txt
│  │          
│  ├─httpx
│  │      httpx.go
│  │      
│  └─integration-test
│          http.go
│          integration-test.go
│          library.go
│          
├─common
│  ├─customextract
│  │      customextract.go
│  │      doc.go
│  │      
│  ├─customheader
│  │      customheader.go
│  │      doc.go
│  │      
│  ├─customlist
│  │      customlist.go
│  │      doc.go
│  │      
│  ├─customports
│  │      customport.go
│  │      doc.go
│  │      
│  ├─fileutil
│  │      doc.go
│  │      fileutil.go
│  │      
│  ├─hashes
│  │  │  doc.go
│  │  │  hashes.go
│  │  │  
│  │  └─jarm
│  │          jarmhash.go
│  │          onetimepool.go
│  │          
│  ├─httputilz
│  │      doc.go
│  │      httputilz.go
│  │      
│  ├─httpx
│  │      cdn.go
│  │      csp.go
│  │      doc.go
│  │      encodings.go
│  │      filter.go
│  │      http2.go
│  │      httpx.go
│  │      option.go
│  │      pipeline.go
│  │      response.go
│  │      title.go
│  │      tls.go
│  │      types.go
│  │      virtualhost.go
│  │      
│  ├─slice
│  │      doc.go
│  │      slice.go
│  │      
│  └─stringz
│          doc.go
│          stringz.go
│          
├─integration_tests
│      run.sh
│      
├─internal
│  └─testutils
│          integration.go
│          
├─runner
│  │  banner.go
│  │  doc.go
│  │  filteroperator.go
│  │  healthcheck.go
│  │  options.go
│  │  resume.go
│  │  runner.go
│  │  runner_test.go
│  │  types.go
│  │  
│  └─tests
│          AS14421.txt
│          
├─scripts
│      
└─static
```

## 02-官方包库

- [ ] [archive/zip](https://pkg.go.dev/archive/zip)
- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [container/heap](https://pkg.go.dev/container/heap)
- [ ] [container/list](https://pkg.go.dev/container/list)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/rand](https://pkg.go.dev/crypto/rand)
- [ ] [crypto/rsa](https://pkg.go.dev/crypto/rsa)
- [ ] [crypto/sha256](https://pkg.go.dev/crypto/sha256)
- [ ] [crypto/sha512](https://pkg.go.dev/crypto/sha512)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [crypto/x509](https://pkg.go.dev/crypto/x509)
- [ ] [database/sql/driver](https://pkg.go.dev/database/sql/driver)
- [ ] [debug/dwarf](https://pkg.go.dev/debug/dwarf)
- [ ] [debug/elf](https://pkg.go.dev/debug/elf)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/csv](https://pkg.go.dev/encoding/csv)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [expvar](https://pkg.go.dev/expvar)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/fs](https://pkg.go.dev/io/fs)
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/http/httputil](https://pkg.go.dev/net/http/httputil)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/exec](https://pkg.go.dev/os/exec)
- [ ] [os/signal](https://pkg.go.dev/os/signal)
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [regexp/syntax](https://pkg.go.dev/regexp/syntax)
- [ ] [runtime](https://pkg.go.dev/runtime)
- [ ] [runtime/pprof](https://pkg.go.dev/runtime/pprof)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [syscall](https://pkg.go.dev/syscall)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)

## 03-第三方库

- [ ] https://github.com/akrylysov/pogreb
- [ ] https://github.com/AndreasBriese/bbloom
- [ ] https://github.com/andybalholm/cascadia
- [ ] https://github.com/asaskevich/govalidator
- [ ] https://github.com/aymerick/douceur
- [ ] https://github.com/bluele/gcache
- [ ] https://github.com/bxcodec/faker
- [ ] https://github.com/cespare/xxhash
- [ ] https://github.com/cnf/structhash
- [ ] https://github.com/cockroachdb/errors
- [ ] https://github.com/cockroachdb/logtags
- [ ] https://github.com/cockroachdb/pebble
- [ ] https://github.com/cockroachdb/redact
- [ ] https://github.com/cockroachdb/sentry-go
- [ ] https://github.com/corpix/uarand
- [ ] https://github.com/DataDog/zstd
- [ ] https://github.com/dgraph-io/badger
- [ ] https://github.com/dgraph-io/ristretto
- [ ] https://github.com/dimchansky/utfbom
- [ ] https://github.com/dustin/go-humanize
- [ ] https://github.com/gogo/protobuf
- [ ] https://github.com/golang/snappy
- [ ] https://github.com/gorilla/css/scanner
- [ ] https://github.com/hbakhtiyor/strsim
- [ ] https://github.com/hdm/jarm-go
- [ ] https://github.com/json-iterator/go
- [ ] https://github.com/Knetic/govaluate
- [ ] https://github.com/kr/pretty
- [ ] https://github.com/kr/text
- [ ] https://github.com/logrusorgru/aurora
- [ ] https://github.com/mfonda/simhash
- [ ] https://github.com/microcosm-cc/bluemonday
- [ ] https://github.com/miekg/dns
- [ ] https://github.com/mitchellh/mapstructure
- [ ] https://github.com/modern-go/concurrent
- [ ] https://github.com/modern-go/reflect2
- [ ] https://github.com/Mzack9999/go-http-digest-auth-client
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/projectdiscovery/asnmap/libs
- [ ] https://github.com/projectdiscovery/blackrock
- [ ] https://github.com/projectdiscovery/cdncheck
- [ ] https://github.com/projectdiscovery/clistats
- [ ] https://github.com/projectdiscovery/cryptoutil
- [ ] https://github.com/projectdiscovery/dsl
- [ ] https://github.com/projectdiscovery/fastdialer
- [ ] https://github.com/projectdiscovery/fileutil
- [ ] https://github.com/projectdiscovery/goconfig
- [ ] https://github.com/projectdiscovery/goflags
- [ ] https://github.com/projectdiscovery/gologger
- [ ] https://github.com/projectdiscovery/hmap
- [ ] https://github.com/projectdiscovery/httputil
- [ ] https://github.com/projectdiscovery/iputil
- [ ] https://github.com/projectdiscovery/mapcidr
- [ ] https://github.com/projectdiscovery/mapcidr/asn
- [ ] https://github.com/projectdiscovery/mapsutil
- [ ] https://github.com/projectdiscovery/networkpolicy
- [ ] https://github.com/projectdiscovery/ratelimit
- [ ] https://github.com/projectdiscovery/rawhttp
- [ ] https://github.com/projectdiscovery/reflectutil
- [ ] https://github.com/projectdiscovery/retryabledns
- [ ] https://github.com/projectdiscovery/retryablehttp-go
- [ ] https://github.com/projectdiscovery/sliceutil
- [ ] https://github.com/projectdiscovery/stringsutil
- [ ] https://github.com/projectdiscovery/tlsx/pkg/tlsx/clients
- [ ] https://github.com/projectdiscovery/urlutil
- [ ] https://github.com/projectdiscovery/wappalyzergo
- [ ] https://github.com/PuerkitoBio/goquery
- [ ] https://github.com/remeh/sizedwaitgroup
- [ ] https://github.com/rogpeppe/go-internal/fmtsort
- [ ] https://github.com/rs/xid
- [ ] https://github.com/saintfish/chardet
- [ ] https://github.com/spaolacci/murmur3
- [ ] https://github.com/syndtr/goleveldb/leveldb
- [ ] https://github.com/ulule/deepcopier
- [ ] https://github.com/weppos/publicsuffix-go/publicsuffix
- [ ] https://github.com/yl2chen/cidranger
- [ ] https://github.com/yl2chen/cidranger/net
- [ ] https://github.com/zmap/rc2
- [ ] https://github.com/zmap/zcertificate
- [ ] https://github.com/zmap/zcrypto
- [ ] https://go.etcd.io/bbolt
- [ ] https://go.uber.org/atomic
- [ ] https://go.uber.org/multierr
- [ ] https://gopkg.in/ini.v1
- [ ] https://gopkg.in/yaml.v3

## 04-开发设计

本部分尽可能的列举出httpx开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Erhttpx