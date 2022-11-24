# [vscan](https://github.com/veo/vscan)代码分析

本文将从代码层面深入分析vscan项目。vscan是安恒星火实验室开发的一款开源、轻量、快速、跨平台的网站漏洞扫描工具。功能包括端口扫描(port scan) 指纹识别(fingerprint) 漏洞检测(nday check) 智能爆破 (admin brute) 敏感文件扫描(file fuzz)等。

本文创建于2022年11月25日，最近的一次更新时间为2022年11月25日。

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
├─brute
│  │  admin_brute.go
│  │  basic_brute.go
│  │  check_loginpage.go
│  │  dicts.go
│  │  filefuzz.go
│  │  fuzzfingerprints.go
│  │  jboss_brute.go
│  │  tomcat_brute.go
│  │  weblogic_brute.go
│  │  
│  └─dicts
│          
├─pkg
│  │  log.go
│  │  util.go
│  │  
│  ├─fingerprint
│  │  │  eHoleFingerData.go
│  │  │  fingerScan.go
│  │  │  getFavicon.go
│  │  │  loadFinger.go
│  │  │  localFingerData.go
│  │  │  matchfinger.go
│  │  │  
│  │  └─dicts
│  │          eHoleFinger.json
│  │          localFinger.json
│  │          
│  ├─httpx
│  │  ├─common
│  │  │  ├─customheader
│  │  │  │      customheader.go
│  │  │  │      doc.go
│  │  │  │      
│  │  │  ├─customlist
│  │  │  │      customlist.go
│  │  │  │      doc.go
│  │  │  │      
│  │  │  ├─customports
│  │  │  │      customport.go
│  │  │  │      doc.go
│  │  │  │      
│  │  │  ├─fileutil
│  │  │  │      doc.go
│  │  │  │      fileutil.go
│  │  │  │      
│  │  │  ├─hashes
│  │  │  │      doc.go
│  │  │  │      hashes.go
│  │  │  │      jarmhash.go
│  │  │  │      
│  │  │  ├─httputilz
│  │  │  │      doc.go
│  │  │  │      httputilz.go
│  │  │  │      
│  │  │  ├─httpx
│  │  │  │      cdn.go
│  │  │  │      csp.go
│  │  │  │      doc.go
│  │  │  │      encodings.go
│  │  │  │      filter.go
│  │  │  │      http2.go
│  │  │  │      httpx.go
│  │  │  │      option.go
│  │  │  │      pipeline.go
│  │  │  │      response.go
│  │  │  │      title.go
│  │  │  │      tls.go
│  │  │  │      virtualhost.go
│  │  │  │      
│  │  │  ├─regexhelper
│  │  │  │      regex.go
│  │  │  │      
│  │  │  ├─slice
│  │  │  │      doc.go
│  │  │  │      slice.go
│  │  │  │      
│  │  │  └─stringz
│  │  │          doc.go
│  │  │          stringz.go
│  │  │          
│  │  ├─internal
│  │  │  └─testutils
│  │  │          integration.go
│  │  │          
│  │  └─runner
│  │          banner.go
│  │          doc.go
│  │          options.go
│  │          resume.go
│  │          runner.go
│  │          
│  ├─jndi
│  │      jndilog.go
│  │      server.go
│  │      
│  └─naabu
│                      
├─pocs_go
│      go_poc_check.go
│      
├─pocs_yml
│  │  yml_poc_check.go
│  │  
│  ├─check
│  │      nucleiCheck.go
│  │      xrayCheck.go
│  │      
│  ├─nucleiFiles
│  ├─pkg
│  │  ├─common
│  │  │  └─structs
│  │  │          commonStructs.go
│  │  │          
│  │  ├─nuclei
│  │  │  ├─catalog
│  │  │  │      catalogue.go
│  │  │  │      find.go
│  │  │  │      
│  │  │  ├─parse
│  │  │  │      parse.go
│  │  │  │      parser.go
│  │  │  │      
│  │  │  ├─structs
│  │  │  │      faketype.go
│  │  │  │      
│  │  │  └─templates
│  │  │          cluster.go
│  │  │          compile.go
│  │  │          preprocessors.go
│  │  │          templates.go
│  │  │          workflows.go
│  │  │          
│  │  └─xray
│  │      ├─cel
│  │      │      cel.go
│  │      │      definition.go
│  │      │      implementation.go
│  │      │      
│  │      ├─requests
│  │      │      cache.go
│  │      │      requests.go
│  │      │      
│  │      └─structs
│  │              cache.go
│  │              poc.go
│  │              requests.pb.go
│  │              requests.proto
│  │              tasks.go
│  │              
│  ├─utils
│  │      load.go
│  │      utils.go
│  │      
│  └─xrayFiles
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
- [ ] [container/list](https://pkg.go.dev/container/list)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/aes](https://pkg.go.dev/crypto/aes)
- [ ] [crypto/cipher](https://pkg.go.dev/crypto/cipher)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/rand](https://pkg.go.dev/crypto/rand)
- [ ] [crypto/rsa](https://pkg.go.dev/crypto/rsa)
- [ ] [crypto/sha1](https://pkg.go.dev/crypto/sha1)
- [ ] [crypto/sha256](https://pkg.go.dev/crypto/sha256)
- [ ] [crypto/sha512](https://pkg.go.dev/crypto/sha512)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [database/sql/driver](https://pkg.go.dev/database/sql/driver)
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [expvar](https://pkg.go.dev/expvar)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [hash](https://pkg.go.dev/hash)
- [ ] [hash/adler32](https://pkg.go.dev/hash/adler32)
- [ ] [hash/crc32](https://pkg.go.dev/hash/crc32)
- [ ] [hash/crc64](https://pkg.go.dev/hash/crc64)
- [ ] [hash/fnv](https://pkg.go.dev/hash/fnv)
- [ ] [html](https://pkg.go.dev/html)
- [ ] [html/template](https://pkg.go.dev/html/template)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/fs](https://pkg.go.dev/io/fs)
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [math/rand](https://pkg.go.dev/math/rand)
- [ ] [mime/multipart](https://pkg.go.dev/mime/multipart)
- [ ] [net](https://pkg.go.dev/net)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/http/cookiejar](https://pkg.go.dev/net/http/cookiejar)
- [ ] [net/http/httptest](https://pkg.go.dev/net/http/httptest)
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/http/httputil](https://pkg.go.dev/net/http/httputil)
- [ ] [net/textproto](https://pkg.go.dev/net/textproto)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/exec](https://pkg.go.dev/os/exec)
- [ ] [os/user](https://pkg.go.dev/os/user)
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [regexp/syntax](https://pkg.go.dev/regexp/syntax)
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

## 03-第三方库

- [ ] [https://moul.io/http2curl](https://pkg.go.dev/moul.io/http2curl)
- [ ] https://git.mills.io/prologic/smtpd
- [ ] https://github.com/akrylysov/pogreb
- [ ] https://github.com/alecthomas/jsonschema
- [ ] https://github.com/alecthomas/template
- [ ] https://github.com/alecthomas/template/parse
- [ ] https://github.com/alecthomas/units
- [ ] https://github.com/ammario/ipisp/v2
- [ ] https://github.com/andres-erbsen/clock
- [ ] https://github.com/andybalholm/cascadia
- [ ] https://github.com/andygrunwald/go-jira
- [ ] https://github.com/antchfx/htmlquery
- [ ] https://github.com/antchfx/xpath
- [ ] https://github.com/antlr/antlr4/runtime/Go/antlr
- [ ] https://github.com/asaskevich/govalidator
- [ ] https://github.com/aws/aws-sdk-go/aws
- [ ] https://github.com/aymerick/douceur/css
- [ ] https://github.com/aymerick/douceur/parser
- [ ] https://github.com/bluele/gcache
- [ ] https://github.com/caddyserver/certmagic
- [ ] https://github.com/cnf/structhash
- [ ] https://github.com/corpix/uarand
- [ ] https://github.com/davecgh/go-spew/spew
- [ ] https://github.com/dimchansky/utfbom
- [ ] https://github.com/dlclark/regexp2
- [ ] https://github.com/dlclark/regexp2/syntax
- [ ] https://github.com/docker/go-units
- [ ] https://github.com/dsnet/compress
- [ ] https://github.com/fatih/structs
- [ ] https://github.com/goburrow/cache
- [ ] https://github.com/gobwas/httphead
- [ ] https://github.com/gobwas/pool
- [ ] https://github.com/gobwas/ws
- [ ] https://github.com/golang/groupcache/lru
- [ ] https://github.com/golang/snappy
- [ ] https://github.com/golang-jwt/jwt/v4
- [ ] https://github.com/google/cel-go
- [ ] https://github.com/google/go-github/github
- [ ] https://github.com/google/gopacket
- [ ] https://github.com/google/go-querystring/query
- [ ] https://github.com/google/uuid
- [ ] https://github.com/go-ole/go-ole
- [ ] https://github.com/go-playground/locales
- [ ] https://github.com/go-playground/universal-translator
- [ ] https://github.com/go-playground/validator/v10
- [ ] https://github.com/gorilla/css/scanner
- [ ] https://github.com/go-rod/rod
- [ ] https://github.com/h2non/filetype
- [ ] https://github.com/hashicorp/go-cleanhttp
- [ ] https://github.com/hashicorp/go-retryablehttp
- [ ] https://github.com/hashicorp/go-version
- [ ] https://github.com/hbakhtiyor/strsim
- [ ] https://github.com/iancoleman/orderedmap
- [ ] https://github.com/itchyny/gojq
- [ ] https://github.com/itchyny/timefmt-go
- [ ] https://github.com/jmespath/go-jmespath
- [ ] https://github.com/json-iterator/go
- [ ] https://github.com/karlseguin/ccache
- [ ] https://github.com/klauspost/compress/flate
- [ ] https://github.com/klauspost/compress/zlib
- [ ] https://github.com/klauspost/cpuid/v2
- [ ] https://github.com/Knetic/govaluate
- [ ] https://github.com/leodido/go-urn
- [ ] https://github.com/libdns/libdns
- [ ] https://github.com/logrusorgru/aurora
- [ ] https://github.com/lor00x/goldap/message
- [ ] https://github.com/mfonda/simhash
- [ ] https://github.com/mholt/acmez
- [ ] https://github.com/mholt/acmez/acme
- [ ] https://github.com/mholt/archiver
- [ ] https://github.com/microcosm-cc/bluemonday
- [ ] https://github.com/microcosm-cc/bluemonday/css
- [ ] https://github.com/miekg/dns
- [ ] https://github.com/mitchellh/go-homedir
- [ ] https://github.com/modern-go/concurrent
- [ ] https://github.com/modern-go/reflect2
- [ ] https://github.com/Mzack9999/go-http-digest-auth-client
- [ ] https://github.com/Mzack9999/ldapserver
- [ ] https://github.com/nwaples/rardecode
- [ ] https://github.com/openrdap/rdap
- [ ] https://github.com/owenrumney/go-sarif/v2/sarif
- [ ] https://github.com/phayes/freeport
- [ ] https://github.com/pierrec/lz4
- [ ] https://github.com/pierrec/lz4/internal/xxh32
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/projectdiscovery/blackrock
- [ ] https://github.com/projectdiscovery/cdncheck
- [ ] https://github.com/projectdiscovery/clistats
- [ ] https://github.com/projectdiscovery/cryptoutil
- [ ] https://github.com/projectdiscovery/dnsx/libs/dnsx
- [ ] https://github.com/projectdiscovery/fastdialer/fastdialer
- [ ] https://github.com/projectdiscovery/fdmax/autofdmax
- [ ] https://github.com/projectdiscovery/fileutil
- [ ] https://github.com/projectdiscovery/folderutil
- [ ] https://github.com/projectdiscovery/goconfig
- [ ] https://github.com/projectdiscovery/goflags
- [ ] https://github.com/projectdiscovery/gologger
- [ ] https://github.com/projectdiscovery/hmap
- [ ] https://github.com/projectdiscovery/httputil
- [ ] https://github.com/projectdiscovery/interactsh
- [ ] https://github.com/projectdiscovery/ipranger
- [ ] https://github.com/projectdiscovery/iputil
- [ ] https://github.com/projectdiscovery/mapcidr
- [ ] https://github.com/projectdiscovery/networkpolicy
- [ ] https://github.com/projectdiscovery/nuclei
- [ ] https://github.com/projectdiscovery/rawhttp
- [ ] https://github.com/projectdiscovery/reflectutil
- [ ] https://github.com/projectdiscovery/retryabledns
- [ ] https://github.com/projectdiscovery/retryablehttp-go
- [ ] https://github.com/projectdiscovery/sliceutil
- [ ] https://github.com/projectdiscovery/stringsutil
- [ ] https://github.com/projectdiscovery/uncover/uncover
- [ ] https://github.com/projectdiscovery/uncover/uncover/agent/shodanidb
- [ ] https://github.com/projectdiscovery/urlutil
- [ ] https://github.com/projectdiscovery/wappalyzergo
- [ ] https://github.com/projectdiscovery/yamldoc-go/encoder
- [ ] https://github.com/PuerkitoBio/goquery
- [ ] https://github.com/remeh/sizedwaitgroup
- [ ] https://github.com/rs/xid
- [ ] https://github.com/RumbleDiscovery/jarm-go
- [ ] https://github.com/saintfish/chardet
- [ ] https://github.com/segmentio/ksuid
- [ ] https://github.com/shirou/gopsutil
- [ ] https://github.com/spaolacci/murmur3
- [ ] https://github.com/spf13/cast
- [ ] https://github.com/stoewer/go-strcase
- [ ] https://github.com/syndtr/goleveldb
- [ ] https://github.com/trivago/tgo/tcontainer
- [ ] https://github.com/trivago/tgo/treflect
- [ ] https://github.com/ulikunitz/xz
- [ ] https://github.com/ulikunitz/xz/internal/hash
- [ ] https://github.com/ulikunitz/xz/internal/xlog
- [ ] https://github.com/ulikunitz/xz/lzma
- [ ] https://github.com/ulule/deepcopier
- [ ] https://github.com/valyala/bytebufferpool
- [ ] https://github.com/valyala/fasttemplate
- [ ] https://github.com/weppos/publicsuffix-go
- [ ] https://github.com/xanzy/go-gitlab
- [ ] https://github.com/xi2/xz
- [ ] https://github.com/yl2chen/cidranger
- [ ] https://github.com/yl2chen/cidranger/net
- [ ] https://github.com/ysmood/goob
- [ ] https://github.com/ysmood/gson
- [ ] https://github.com/ysmood/leakless
- [ ] https://github.com/ysmood/leakless/pkg/shared
- [ ] https://github.com/ysmood/leakless/pkg/utils
- [ ] https://github.com/yusufpapurcu/wmi
- [ ] https://github.com/zmap/rc2
- [ ] https://github.com/zmap/zcrypto
- [ ] https://go.etcd.io/bbolt
- [ ] https://go.uber.org/atomic
- [ ] https://go.uber.org/multierr
- [ ] https://go.uber.org/ratelimit
- [ ] https://go.uber.org/zap
- [ ] https://go.uber.org/zap/buffer
- [ ] https://go.uber.org/zap/internal/bufferpool
- [ ] https://go.uber.org/zap/internal/color
- [ ] https://go.uber.org/zap/internal/exit
- [ ] https://go.uber.org/zap/zapcore
- [ ] https://goftp.io/server/v2
- [ ] https://goftp.io/server/v2/driver/file
- [ ] https://goftp.io/server/v2/ratelimit
- [ ] https://gopkg.in/alecthomas/kingpin.v2
- [ ] https://gopkg.in/corvus-ch/zbase32.v1
- [ ] https://gopkg.in/ini.v1
- [ ] https://gopkg.in/yaml.v2
- [ ] https://gopkg.in/yaml.v3

## 04-开发设计

本部分尽可能的列举出vscan开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型
- [ ] 函数方法
- [ ] 并发协程
- [ ] 将多个静态文件打包到了程序中。

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Ervscan