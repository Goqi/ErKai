# [nacs](https://github.com/u21h2/nacs)代码分析

本文将从代码层面深入分析nacs项目。nacs是u21h2开发的事件驱动的渗透测试扫描器。包括探活、服务扫描(常规&非常规端口)、poc探测(xray&nuclei格式)、数据库等弱口令爆破、内网常见漏洞利用等。

本文创建于2022年11月18日，最近的一次更新时间为2022年11月18日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  nacs.go
│  
├─common
│      config.go
│      utils.go
│      
├─discover
│  │  discover.go
│  │  
│  ├─output
│  │      output.go
│  │      
│  ├─parse
│  │      parse_byte_to_string.go
│  │      parse_network.go
│  │      parse_ping.go
│  │      parse_scheme.go
│  │      parse_uri.go
│  │      parse_verbose.go
│  │      
│  ├─protocol
│  │  │  discover.go
│  │  │  identify.go
│  │  │  
│  │  ├─get
│  │  │      tcp.go
│  │  │      tls.go
│  │  │      udp.go
│  │  │      
│  │  └─judge
│  │          tcp_activemq.go
│  │          tcp_dcerpc.go
│  │          tcp_frp.go
│  │          tcp_ftp.go
│  │          tcp_http.go
│  │          tcp_imap.go
│  │          tcp_ldap.go
│  │          tcp_mssql.go
│  │          tcp_mysql.go
│  │          tcp_oracle.go
│  │          tcp_pop3.go
│  │          tcp_rdp.go
│  │          tcp_redis.go
│  │          tcp_rmi.go
│  │          tcp_rtsp.go
│  │          tcp_smb.go
│  │          tcp_smtp.go
│  │          tcp_snmp.go
│  │          tcp_socks.go
│  │          tcp_ssh.go
│  │          tcp_telnet.go
│  │          tcp_vnc.go
│  │          tls_https.go
│  │          tls_rdp.go
│  │          tls_redis_ssl.go
│  │          udp_nbns.go
│  │          
│  ├─proxy
│  │      proxy_http.go
│  │      proxy_tcp.go
│  │      proxy_tls.go
│  │      proxy_udp.go
│  │      
│  └─rule
│          rule.go
│          
├─nonweb
│  │  service.go
│  │  
│  ├─services
│  │      CVE-2020-0796.go
│  │      fcgiscan.go
│  │      findnet.go
│  │      ftp.go
│  │      memcached.go
│  │      mongodb.go
│  │      ms17010.go
│  │      ms17010_exp.go
│  │      mssql.go
│  │      mysql.go
│  │      netbios.go
│  │      oracle.go
│  │      postgres.go
│  │      rdp.go
│  │      redis.go
│  │      smb.go
│  │      ssh.go
│  │      
│  └─utils
│          info_struct.go
│          
├─parse
│      flag.go
│      parse.go
│      parse_credit.go
│      parse_ip.go
│      parse_port.go
│      parse_proxy.go
│      parse_reverse.go
│      parse_url.go
│      
├─probe
│      probe.go
│      
├─utils
│  │  1.png
│  │  2.png
│  │  3.png
│  │  log.go
│  │  proxy.go
│  │  utils.go
│  │  
│  └─logger
│          level.go
│          log.go
│          
└─web
    ├─poc
    │  │  poc.go
    │  │  
    │  ├─errors
    │  │      errors.go
    │  │      stack.go
    │  │      
    │  ├─internal
    │  │  └─common
    │  │      ├─check
    │  │      │      check.go
    │  │      │      nuclei.go
    │  │      │      xray.go
    │  │      │      
    │  │      ├─errors
    │  │      │      errors.go
    │  │      │      
    │  │      ├─load
    │  │      │      load.go
    │  │      │      
    │  │      ├─output
    │  │      │      outut.go
    │  │      │      
    │  │      └─tag
    │  │              tag.go
    │  │              
    │  ├─pkg
    │  │  ├─common
    │  │  │  └─structs
    │  │  │          output.go
    │  │  │          result.go
    │  │  │          reverse.go
    │  │  │          
    │  │  ├─nuclei
    │  │  │  ├─parse
    │  │  │  │      parse.go
    │  │  │  │      
    │  │  │  └─structs
    │  │  │          faketype.go
    │  │  │          poc.go
    │  │  │          task.go
    │  │  │          
    │  │  └─xray
    │  │      ├─cel
    │  │      │      cel.go
    │  │      │      definition.go
    │  │      │      implementation.go
    │  │      │      
    │  │      ├─parse
    │  │      │      parse.go
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
    │  ├─pocs
    │  │  └─nuclei
    │  ├─pocs_dev
    │  │  ├─nucleibak
    │  │  ├─test
    │  │  │  ├─nuclei
    │  │  │  │      
    │  │  │  ├─spon
    │  │  │  │      
    │  │  │  └─xray
    │  │  │          
    │  │  └─xray
    │  ├─pocs_test
    │  │      
    │  ├─poc_single
    │  │      
    │  └─utils
    │          file.go
    │          iconhash.go
    │          log.go
    │          string.go
    │          
    ├─pocv1
    │  │  poc.go
    │  │  
    │  ├─lib
    │  │      check.go
    │  │      client.go
    │  │      eval.go
    │  │      http.pb.go
    │  │      http.proto
    │  │      shiro.go
    │  │      
    │  ├─pocs
    │  ├─pocs_test
    │  │      
    │  └─poc_struct
    │          config.go
    │          
    └─spring
```

## 02-官方包库

- [ ] [archive/tar](https://pkg.go.dev/archive/tar)
- [ ] [archive/zip](https://pkg.go.dev/archive/zip)
- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/flate](https://pkg.go.dev/compress/flate)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
- [ ] [compress/zlib](https://pkg.go.dev/compress/zlib)
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
- [ ] [debug/dwarf](https://pkg.go.dev/debug/dwarf)
- [ ] [debug/elf](https://pkg.go.dev/debug/elf)
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding](https://pkg.go.dev/encoding)
- [ ] [encoding/asn1](https://pkg.go.dev/encoding/asn1)
- [ ] [encoding/base32](https://pkg.go.dev/encoding/base32)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
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
- [ ] [hash/crc64](https://pkg.go.dev/hash/crc64)
- [ ] [hash/fnv](https://pkg.go.dev/hash/fnv)
- [ ] [html](https://pkg.go.dev/html)
- [ ] [html/template](https://pkg.go.dev/html/template)
- [ ] [image](https://pkg.go.dev/image)
- [ ] [image/color](https://pkg.go.dev/image/color)
- [ ] [image/internal/imageutil](https://pkg.go.dev/image/internal/imageutil)
- [ ] [image/jpeg](https://pkg.go.dev/image/jpeg)
- [ ] [image/png](https://pkg.go.dev/image/png)
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
- [ ] [net/http/cookiejar](https://pkg.go.dev/net/http/cookiejar)
- [ ] [net/http/httptest](https://pkg.go.dev/net/http/httptest)
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/http/httputil](https://pkg.go.dev/net/http/httputil)
- [ ] [net/http/internal](https://pkg.go.dev/net/http/internal)
- [ ] [net/http/internal/ascii](https://pkg.go.dev/net/http/internal/ascii)
- [ ] [net/http/internal/testcert](https://pkg.go.dev/net/http/internal/testcert)
- [ ] [net/netip](https://pkg.go.dev/net/netip)
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
- [ ] [runtime/debug](https://pkg.go.dev/runtime/debug)
- [ ] [runtime/internal/atomic](https://pkg.go.dev/runtime/internal/atomic)
- [ ] [runtime/internal/math](https://pkg.go.dev/runtime/internal/math)
- [ ] [runtime/internal/sys](https://pkg.go.dev/runtime/internal/sys)
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

- [ ] https://git.mills.io/prologic/smtpd
- [ ] https://github.com/akrylysov/pogreb
- [ ] https://github.com/alecthomas/jsonschema
- [ ] https://github.com/alecthomas/template
- [ ] https://github.com/alecthomas/units
- [ ] https://github.com/andres-erbsen/clock
- [ ] https://github.com/andygrunwald/go-jira
- [ ] https://github.com/antchfx/htmlquery
- [ ] https://github.com/antchfx/xpath
- [ ] https://github.com/antlr/antlr4/runtime
- [ ] https://github.com/asaskevich/govalidator
- [ ] https://github.com/aws/aws-sdk-go/aws
- [ ] https://github.com/bluele/gcache
- [ ] https://github.com/caddyserver/certmagic
- [ ] https://github.com/cnf/structhash
- [ ] https://github.com/corpix/uarand
- [ ] https://github.com/denisenkom/go-mssqldb
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
- [ ] https://github.com/golang/protobuf
- [ ] https://github.com/golang/snappy
- [ ] https://github.com/golang-jwt/jwt/v4
- [ ] https://github.com/golang-sql/civil
- [ ] https://github.com/golang-sql/sqlexp
- [ ] https://github.com/google/cel-go
- [ ] https://github.com/google/go-github
- [ ] https://github.com/google/go-querystring/query
- [ ] https://github.com/google/uuid
- [ ] https://github.com/gookit/color
- [ ] https://github.com/go-ole/go-ole
- [ ] https://github.com/go-ole/go-ole/oleutil
- [ ] https://github.com/go-playground/locales
- [ ] https://github.com/go-playground/locales/currency
- [ ] https://github.com/go-playground/universal-translator
- [ ] https://github.com/go-playground/validator/v10
- [ ] https://github.com/go-rod/rod
- [ ] https://github.com/go-sql-driver/mysql
- [ ] https://github.com/h2non/filetype
- [ ] https://github.com/h2non/filetype/matchers
- [ ] https://github.com/h2non/filetype/types
- [ ] https://github.com/hashicorp/go-cleanhttp
- [ ] https://github.com/hashicorp/go-retryablehttp
- [ ] https://github.com/hashicorp/go-version
- [ ] https://github.com/huin/asn1ber
- [ ] https://github.com/iancoleman/orderedmap
- [ ] https://github.com/icodeface/tls
- [ ] https://github.com/itchyny/gojq
- [ ] https://github.com/itchyny/timefmt-go
- [ ] https://github.com/jlaffaye/ftp
- [ ] https://github.com/jmespath/go-jmespath
- [ ] https://github.com/json-iterator/go
- [ ] https://github.com/karlseguin/ccache
- [ ] https://github.com/karrick/godirwalk
- [ ] https://github.com/klauspost/compress
- [ ] https://github.com/klauspost/cpuid/v2
- [ ] https://github.com/Knetic/govaluate
- [ ] https://github.com/leodido/go-urn
- [ ] https://github.com/lib/pq
- [ ] https://github.com/libdns/libdns
- [ ] https://github.com/logrusorgru/aurora
- [ ] https://github.com/lor00x/goldap/message
- [ ] https://github.com/lunixbochs/struc
- [ ] https://github.com/mholt/acmez
- [ ] https://github.com/mholt/acmez/acme
- [ ] https://github.com/mholt/archiver
- [ ] https://github.com/miekg/dns
- [ ] https://github.com/mitchellh/go-homedir
- [ ] https://github.com/modern-go/concurrent
- [ ] https://github.com/modern-go/reflect2
- [ ] https://github.com/Mzack9999/go-http-digest-auth-client
- [ ] https://github.com/Mzack9999/ldapserver
- [ ] https://github.com/nwaples/rardecode
- [ ] https://github.com/openrdap/rdap
- [ ] https://github.com/openrdap/rdap/sandbox
- [ ] https://github.com/owenrumney/go-sarif
- [ ] https://github.com/panjf2000/ants
- [ ] https://github.com/patrickmn/go-cache
- [ ] https://github.com/pierrec/lz4
- [ ] https://github.com/pierrec/lz4/internal/xxh32
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/projectdiscovery/blackrock
- [ ] https://github.com/projectdiscovery/clistats
- [ ] https://github.com/projectdiscovery/cryptoutil
- [ ] https://github.com/projectdiscovery/fastdialer/fastdialer
- [ ] https://github.com/projectdiscovery/fileutil
- [ ] https://github.com/projectdiscovery/folderutil
- [ ] https://github.com/projectdiscovery/goflags
- [ ] https://github.com/projectdiscovery/gologger
- [ ] https://github.com/projectdiscovery/hmap
- [ ] https://github.com/projectdiscovery/interactsh
- [ ] https://github.com/projectdiscovery/iputil
- [ ] https://github.com/projectdiscovery/mapcidr
- [ ] https://github.com/projectdiscovery/networkpolicy
- [ ] https://github.com/projectdiscovery/nuclei
- [ ] https://github.com/projectdiscovery/rawhttp
- [ ] https://github.com/projectdiscovery/retryabledns
- [ ] https://github.com/projectdiscovery/retryablehttp-go
- [ ] https://github.com/projectdiscovery/sliceutil
- [ ] https://github.com/projectdiscovery/stringsutil
- [ ] https://github.com/projectdiscovery/urlutil
- [ ] https://github.com/projectdiscovery/yamldoc-go/encoder
- [ ] https://github.com/remeh/sizedwaitgroup
- [ ] https://github.com/rs/xid
- [ ] https://github.com/satori/go.uuid
- [ ] https://github.com/segmentio/ksuid
- [ ] https://github.com/shirou/gopsutil
- [ ] https://github.com/sijms/go-ora
- [ ] https://github.com/spaolacci/murmur3
- [ ] https://github.com/spf13/cast
- [ ] https://github.com/stacktitan/smb/gss
- [ ] https://github.com/stacktitan/smb/ntlmssp
- [ ] https://github.com/stacktitan/smb/smb
- [ ] https://github.com/syndtr/goleveldb
- [ ] https://github.com/tomatome/grdp
- [ ] https://github.com/trivago/tgo/tcontainer
- [ ] https://github.com/trivago/tgo/treflect
- [ ] https://github.com/txthinking/runnergroup
- [ ] https://github.com/txthinking/socks5
- [ ] https://github.com/txthinking/x
- [ ] https://github.com/ulikunitz/xz
- [ ] https://github.com/ulule/deepcopier
- [ ] https://github.com/valyala/bytebufferpool
- [ ] https://github.com/valyala/fasttemplate
- [ ] https://github.com/weppos/publicsuffix-go/publicsuffix
- [ ] https://github.com/xanzy/go-gitlab
- [ ] https://github.com/xi2/xz
- [ ] https://github.com/xo/terminfo
- [ ] https://github.com/yargevad/filepathx
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
- [ ] https://goftp.io/server/v2
- [ ] https://gopkg.in/alecthomas/kingpin.v2
- [ ] https://gopkg.in/corvus-ch/zbase32.v1
- [ ] https://gopkg.in/yaml.v2
- [ ] https://gopkg.in/yaml.v3

## 04-开发设计

本部分尽可能的列举出nacs开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Ernacs