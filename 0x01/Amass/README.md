# [Amass](https://github.com/OWASP/Amass)代码分析

本文将从代码层面深入分析Amass项目。Amass是OWASP开发的一款域名资产发现工具。项目使用开源信息收集和主动侦察技术执行攻击面的网络映射和外部资产发现。

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
│  └─amass
│          db.go
│          enum.go
│          help.go
│          intel.go
│          io.go
│          main.go
│          track.go
│          viz.go
│          
├─config
│      brute.go
│      brute_test.go
│      config.go
│      config_test.go
│      datasrcs.go
│      datasrcs_test.go
│      graphdb.go
│      graphdb_test.go
│      resolvers.go
│      resolvers_test.go
│      scope.go
│      scope_test.go
│      scripts.go
│      test_wordlist.txt
│      wordlist.go
│      wordlist_test.go
│      
├─datasrcs
│  │  alienvault.go
│  │  cloudflare.go
│  │  dnsdb.go
│  │  networksdb.go
│  │  radb.go
│  │  sources.go
│  │  twitter.go
│  │  umbrella.go
│  │  
│  └─scripting
│          cache.go
│          cache_test.go
│          config.go
│          dns.go
│          dns_test.go
│          http.go
│          new.go
│          new_test.go
│          script.go
│          script_test.go
│          socket.go
│          socket_test.go
│          support.go
│          
├─doc
│      install.md
│      scripting.md
│      tutorial.md
│      user_guide.md
│      
├─enum
│      active.go
│      dns.go
│      enum.go
│      input.go
│      monitor.go
│      names.go
│      srv_records.go
│      store.go
│      zone.go
│      zone_test.go
│      
├─examples
│  │  config.ini
│  │  
│  └─wordlists
│          all.txt
│          
├─format
│      parse.go
│      parse_test.go
│      print.go
│      
├─images
│          
├─intel
│      active.go
│      input.go
│      intel.go
│      
├─limits
│      limits_unix.go
│      limits_unix_test.go
│      limits_windows.go
│      limits_windows_test.go
│      
├─net
│  │  network.go
│  │  network_test.go
│  │  
│  ├─dns
│  │      dns.go
│  │      dns_test.go
│  │      
│  └─http
│      │  http.go
│      │  http_test.go
│      │  
│      └─static
│              
├─requests
│      asncache.go
│      asncache_test.go
│      request.go
│      request_test.go
│      
├─resources
│  │  alterations.txt
│  │  io.go
│  │  io_test.go
│  │  ip2asn-combined.tsv.gz
│  │  namelist.txt
│  │  user_agents.txt
│  │  
│  └─scripts
│      ├─alt
│      │      alterations.ads
│      │      
│      ├─api
│      │      360passivedns.ads
│      │      
│      ├─archive
│      │      archiveit.ads
│      │      arquivo.ads
│      │      haw.ads
│      │      ukwebarchive.ads
│      │      wayback.ads
│      │      
│      ├─brute
│      │      bruteforcing.ads
│      │      
│      ├─cert
│      │      censys.ads
│      │      certcentral.ads
│      │      certspotter.ads
│      │      crtsh.ads
│      │      digitorus.ads
│      │      facebookct.ads
│      │      googlect.ads
│      │      
│      ├─crawl
│      │      commoncrawl.ads
│      │      publicwww.ads
│      │      
│      ├─misc
│      │      shadowserver.ads
│      │      teamcymru.ads
│      │      
│      └─scrape
│              abuseipdb.ads
│              
├─systems
│      local.go
│      local_test.go
│      simple.go
│      system.go
│      
└─viz
        d3.go
        d3_test.go
        dot.go
        dot_test.go
        gexf.go
        gexf_test.go
        graphistry.go
        graphistry_test.go
        maltego.go
        maltego_test.go
        viz.go
        viz_test.go
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/flate](https://pkg.go.dev/compress/flate)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
- [ ] [container/heap](https://pkg.go.dev/container/heap)
- [ ] [container/list](https://pkg.go.dev/container/list)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto](https://pkg.go.dev/crypto)
- [ ] [crypto/aes](https://pkg.go.dev/crypto/aes)
- [ ] [crypto/cipher](https://pkg.go.dev/crypto/cipher)
- [ ] [crypto/des](https://pkg.go.dev/crypto/des)
- [ ] [crypto/dsa](https://pkg.go.dev/crypto/dsa)
- [ ] [crypto/ecdsa](https://pkg.go.dev/crypto/ecdsa)
- [ ] [crypto/ed25519](https://pkg.go.dev/crypto/ed25519)
- [ ] [crypto/elliptic](https://pkg.go.dev/crypto/elliptic)
- [ ] [crypto/hmac](https://pkg.go.dev/crypto/hmac)
- [ ] [crypto/internal/boring](https://pkg.go.dev/crypto/internal/boring)
- [ ] [crypto/internal/boring/bbig](https://pkg.go.dev/crypto/internal/boring/bbig)
- [ ] [crypto/internal/boring/sig](https://pkg.go.dev/crypto/internal/boring/sig)
- [ ] [crypto/internal/edwards25519](https://pkg.go.dev/crypto/internal/edwards25519)
- [ ] [crypto/internal/edwards25519/field](https://pkg.go.dev/crypto/internal/edwards25519/field)
- [ ] [crypto/internal/nistec](https://pkg.go.dev/crypto/internal/nistec)
- [ ] [crypto/internal/nistec/fiat](https://pkg.go.dev/crypto/internal/nistec/fiat)
- [ ] [crypto/internal/randutil](https://pkg.go.dev/crypto/internal/randutil)
- [ ] [crypto/internal/subtle](https://pkg.go.dev/crypto/internal/subtle)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/rand](https://pkg.go.dev/crypto/rand)
- [ ] [crypto/rc4](https://pkg.go.dev/crypto/rc4)
- [ ] [crypto/rsa](https://pkg.go.dev/crypto/rsa)
- [ ] [crypto/sha1](https://pkg.go.dev/crypto/sha1)
- [ ] [crypto/sha256](https://pkg.go.dev/crypto/sha256)
- [ ] [crypto/sha512](https://pkg.go.dev/crypto/sha512)
- [ ] [crypto/subtle](https://pkg.go.dev/crypto/subtle)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [crypto/x509](https://pkg.go.dev/crypto/x509)
- [ ] [crypto/x509/pkix](https://pkg.go.dev/crypto/x509/pkix)
- [ ] [database/sql](https://pkg.go.dev/database/sql)
- [ ] [database/sql/driver](https://pkg.go.dev/database/sql/driver)
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
- [ ] [encoding/xml](https://pkg.go.dev/encoding/xml)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [expvar](https://pkg.go.dev/expvar)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [hash](https://pkg.go.dev/hash)
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
- [ ] [net/http/cookiejar](https://pkg.go.dev/net/http/cookiejar)
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/http/httputil](https://pkg.go.dev/net/http/httputil)
- [ ] [net/http/internal](https://pkg.go.dev/net/http/internal)
- [ ] [net/http/internal/ascii](https://pkg.go.dev/net/http/internal/ascii)
- [ ] [net/netip](https://pkg.go.dev/net/netip)
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
- [ ] [runtime/debug](https://pkg.go.dev/runtime/debug)
- [ ] [runtime/internal/atomic](https://pkg.go.dev/runtime/internal/atomic)
- [ ] [runtime/internal/math](https://pkg.go.dev/runtime/internal/math)
- [ ] [runtime/internal/sys](https://pkg.go.dev/runtime/internal/sys)
- [ ] [runtime/metrics](https://pkg.go.dev/runtime/metrics)
- [ ] [runtime/trace](https://pkg.go.dev/runtime/trace)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [sync/atomic](https://pkg.go.dev/sync/atomic)
- [ ] [syscall](https://pkg.go.dev/syscall)
- [ ] [testing](https://pkg.go.dev/testing)
- [ ] [text/tabwriter](https://pkg.go.dev/text/tabwriter)
- [ ] [text/template](https://pkg.go.dev/text/template)
- [ ] [text/template/parse](https://pkg.go.dev/text/template/parse)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode](https://pkg.go.dev/unicode)
- [ ] [unicode/utf16](https://pkg.go.dev/unicode/utf16)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] [layeh.com/gopher-json](https://pkg.go.dev/layeh.com/gopher-json)
- [ ] https://github.com/AndreasBriese/bbloom
- [ ] https://github.com/andres-erbsen/clock
- [ ] https://github.com/andybalholm/cascadia
- [ ] https://github.com/beorn7/perks/quantile
- [ ] https://github.com/boltdb/bolt
- [ ] https://github.com/caffix/netmap
- [ ] https://github.com/caffix/pipeline
- [ ] https://github.com/caffix/queue
- [ ] https://github.com/caffix/resolve
- [ ] https://github.com/caffix/service
- [ ] https://github.com/caffix/stringset
- [ ] https://github.com/cayleygraph/cayley
- [ ] https://github.com/cayleygraph/quad
- [ ] https://github.com/cenkalti/backoff/v4
- [ ] https://github.com/cespare/xxhash/v2
- [ ] https://github.com/chromedp/cdproto
- [ ] https://github.com/chromedp/chromedp
- [ ] https://github.com/chromedp/sysutil
- [ ] https://github.com/cjoudrey/gluaurl
- [ ] https://github.com/cloudflare/cloudflare-go
- [ ] https://github.com/dennwc/base
- [ ] https://github.com/dghubble/go-twitter/twitter
- [ ] https://github.com/dghubble/sling
- [ ] https://github.com/dgraph-io/badger
- [ ] https://github.com/dgraph-io/ristretto/z
- [ ] https://github.com/dgraph-io/ristretto/z/simd
- [ ] https://github.com/dustin/go-humanize
- [ ] https://github.com/fatih/color
- [ ] https://github.com/geziyor/geziyor
- [ ] https://github.com/gobwas/httphead
- [ ] https://github.com/gobwas/pool
- [ ] https://github.com/gobwas/ws
- [ ] https://github.com/gogo/protobuf
- [ ] https://github.com/go-ini/ini
- [ ] https://github.com/go-kit/kit
- [ ] https://github.com/golang/glog
- [ ] https://github.com/golang/protobuf
- [ ] https://github.com/google/go-querystring/query
- [ ] https://github.com/google/uuid
- [ ] https://github.com/go-sql-driver/mysql
- [ ] https://github.com/hashicorp/errwrap
- [ ] https://github.com/hashicorp/go-cleanhttp
- [ ] https://github.com/hashicorp/go-multierror
- [ ] https://github.com/hashicorp/go-retryablehttp
- [ ] https://github.com/hidal-go/hidalgo/base
- [ ] https://github.com/hidal-go/hidalgo/kv
- [ ] https://github.com/hidal-go/hidalgo/kv/bolt
- [ ] https://github.com/josharian/intern
- [ ] https://github.com/lib/pq
- [ ] https://github.com/lib/pq/oid
- [ ] https://github.com/lib/pq/scram
- [ ] https://github.com/mailru/easyjson
- [ ] https://github.com/mailru/easyjson/buffer
- [ ] https://github.com/mailru/easyjson/jlexer
- [ ] https://github.com/mailru/easyjson/jwriter
- [ ] https://github.com/mattn/go-colorable
- [ ] https://github.com/mattn/go-isatty
- [ ] https://github.com/matttproud/golang_protobuf_extensions
- [ ] https://github.com/miekg/dns
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/prometheus/client_golang
- [ ] https://github.com/prometheus/client_model/go
- [ ] https://github.com/prometheus/common
- [ ] https://github.com/PuerkitoBio/goquery
- [ ] https://github.com/temoto/robotstxt
- [ ] https://github.com/tylertreat/BoomFilters
- [ ] https://github.com/VividCortex/gohistogram
- [ ] https://github.com/yl2chen/cidranger
- [ ] https://github.com/yl2chen/cidranger/net
- [ ] https://github.com/yuin/gopher-lua
- [ ] https://go.uber.org/ratelimit

## 04-开发设计

本部分尽可能的列举出Amass开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/ErAmass