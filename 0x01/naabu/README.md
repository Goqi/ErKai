# [naabu](https://github.com/projectdiscovery/naabu)代码分析

本文将从代码层面深入分析naabu项目。naabu是projectdiscovery项目开发的快速端口扫描器，重点是可靠性和简单性。旨在与其他工具结合使用，以发现漏洞赏金和渗透测试中的攻击面。

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
│  │          request.txt
│  │          
│  ├─integration-test
│  │      integration-test.go
│  │      library.go
│  │      
│  └─naabu
│          main.go
│          
├─internal
│  └─testutils
│          integration.go
│          
└─pkg
    ├─israce
    │      norace.go
    │      race.go
    │      
    ├─privileges
    │      privileges.go
    │      privileges_darwin.go
    │      privileges_linux.go
    │      privileges_win.go
    │      
    ├─result
    │      results.go
    │      results_test.go
    │      
    ├─routing
    │      router.go
    │      router_darwin.go
    │      router_linux.go
    │      router_windows.go
    │      
    ├─runner
    │      banners.go
    │      banners_test.go
    │      default.go
    │      healthcheck.go
    │      ips.go
    │      ips_test.go
    │      nmap.go
    │      nmap_test.go
    │      options.go
    │      output.go
    │      output_test.go
    │      ports.go
    │      ports_test.go
    │      resume.go
    │      runner.go
    │      targets.go
    │      targets_test.go
    │      util.go
    │      util_test.go
    │      validate.go
    │      validate_test.go
    │      
    ├─scan
    │      arp.go
    │      cdn.go
    │      cdn_test.go
    │      connect.go
    │      connect_test.go
    │      externalip.go
    │      externalip_test.go
    │      icmp.go
    │      ndp.go
    │      option.go
    │      ping.go
    │      ping_test.go
    │      scan.go
    │      scan_unix.go
    │      tcpsequencer.go
    │      tcpsequencer_test.go
    │      
    └─utils
            util.go
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [container/heap](https://pkg.go.dev/container/heap)
- [ ] [container/list](https://pkg.go.dev/container/list)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [debug/dwarf](https://pkg.go.dev/debug/dwarf)
- [ ] [debug/elf](https://pkg.go.dev/debug/elf)
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding](https://pkg.go.dev/encoding)
- [ ] [encoding/asn1](https://pkg.go.dev/encoding/asn1)
- [ ] [encoding/base32](https://pkg.go.dev/encoding/base32)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/csv](https://pkg.go.dev/encoding/csv)
- [ ] [encoding/gob](https://pkg.go.dev/encoding/gob)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [encoding/pem](https://pkg.go.dev/encoding/pem)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [expvar](https://pkg.go.dev/expvar)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [hash](https://pkg.go.dev/hash)
- [ ] [hash/adler32](https://pkg.go.dev/hash/adler32)
- [ ] [hash/crc32](https://pkg.go.dev/hash/crc32)
- [ ] [hash/fnv](https://pkg.go.dev/hash/fnv)
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
- [ ] [mime](https://pkg.go.dev/mime)
- [ ] [mime/multipart](https://pkg.go.dev/mime/multipart)
- [ ] [mime/quotedprintable](https://pkg.go.dev/mime/quotedprintable)
- [ ] [net](https://pkg.go.dev/net)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/http/internal](https://pkg.go.dev/net/http/internal)
- [ ] [net/http/internal/ascii](https://pkg.go.dev/net/http/internal/ascii)
- [ ] [net/netip](https://pkg.go.dev/net/netip)
- [ ] [net/textproto](https://pkg.go.dev/net/textproto)
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
- [ ] [runtime/cgo](https://pkg.go.dev/runtime/cgo)
- [ ] [runtime/debug](https://pkg.go.dev/runtime/debug)
- [ ] [runtime/internal/atomic](https://pkg.go.dev/runtime/internal/atomic)
- [ ] [runtime/internal/math](https://pkg.go.dev/runtime/internal/math)
- [ ] [runtime/internal/sys](https://pkg.go.dev/runtime/internal/sys)
- [ ] [runtime/pprof](https://pkg.go.dev/runtime/pprof)
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

- [ ] https://github.com/akrylysov/pogreb
- [ ] https://github.com/akrylysov/pogreb/fs
- [ ] https://github.com/akrylysov/pogreb/internal/errors
- [ ] https://github.com/akrylysov/pogreb/internal/hash
- [ ] https://github.com/AndreasBriese/bbloom
- [ ] https://github.com/asaskevich/govalidator
- [ ] https://github.com/aymerick/douceur/css
- [ ] https://github.com/aymerick/douceur/parser
- [ ] https://github.com/cespare/xxhash
- [ ] https://github.com/cespare/xxhash/v2
- [ ] https://github.com/cnf/structhash
- [ ] https://github.com/cockroachdb/errors
- [ ] https://github.com/cockroachdb/logtags
- [ ] https://github.com/cockroachdb/pebble
- [ ] https://github.com/cockroachdb/redact
- [ ] https://github.com/cockroachdb/sentry-go
- [ ] https://github.com/DataDog/zstd
- [ ] https://github.com/dgraph-io/badger
- [ ] https://github.com/dgraph-io/ristretto/z
- [ ] https://github.com/dustin/go-humanize
- [ ] https://github.com/gogo/protobuf
- [ ] https://github.com/golang/snappy
- [ ] https://github.com/google/gopacket
- [ ] https://github.com/gorilla/css/scanner
- [ ] https://github.com/json-iterator/go
- [ ] https://github.com/kr/pretty
- [ ] https://github.com/kr/text
- [ ] https://github.com/logrusorgru/aurora
- [ ] https://github.com/microcosm-cc/bluemonday
- [ ] https://github.com/microcosm-cc/bluemonday/css
- [ ] https://github.com/miekg/dns
- [ ] https://github.com/modern-go/concurrent
- [ ] https://github.com/modern-go/reflect2
- [ ] https://github.com/phayes/freeport
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/projectdiscovery/asnmap/libs
- [ ] https://github.com/projectdiscovery/blackrock
- [ ] https://github.com/projectdiscovery/cdncheck
- [ ] https://github.com/projectdiscovery/clistats
- [ ] https://github.com/projectdiscovery/dnsx/libs/dnsx
- [ ] https://github.com/projectdiscovery/fdmax
- [ ] https://github.com/projectdiscovery/fileutil
- [ ] https://github.com/projectdiscovery/goflags
- [ ] https://github.com/projectdiscovery/gologger
- [ ] https://github.com/projectdiscovery/hmap
- [ ] https://github.com/projectdiscovery/ipranger
- [ ] https://github.com/projectdiscovery/iputil
- [ ] https://github.com/projectdiscovery/mapcidr
- [ ] https://github.com/projectdiscovery/networkpolicy
- [ ] https://github.com/projectdiscovery/ratelimit
- [ ] https://github.com/projectdiscovery/retryabledns
- [ ] https://github.com/projectdiscovery/retryablehttp-go
- [ ] https://github.com/projectdiscovery/sliceutil
- [ ] https://github.com/projectdiscovery/stringsutil
- [ ] https://github.com/projectdiscovery/uncover
- [ ] https://github.com/remeh/sizedwaitgroup
- [ ] https://github.com/saintfish/chardet
- [ ] https://github.com/syndtr/goleveldb/leveldb
- [ ] https://github.com/yl2chen/cidranger
- [ ] https://github.com/yl2chen/cidranger/net
- [ ] https://go.etcd.io/bbolt
- [ ] https://go.uber.org/atomic
- [ ] https://go.uber.org/multierr
- [ ] https://gopkg.in/yaml.v3

## 04-开发设计

本部分尽可能的列举出naabu开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Ernaabu