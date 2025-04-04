# [scan4all](https://github.com/hktalent/scan4all)代码分析

本文将从代码层面深入分析scan4all项目。scan4all是hktalent开发的一款由多个安全漏洞扫描工具集成的漏洞扫描工具，包括漏洞扫描，弱密码爆破，Web指纹识别，端口扫描等。

本文创建于2022年11月14日，最近的一次更新时间为2022年11月14日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
├─main.go
├─brute
│  └─dicts
├─config
├─db
├─engine
├─lib
│  ├─api
│  ├─crawlergo
│  ├─goby
│  │  └─goby_pocs
│  ├─Smuggling
│  │  ├─generate
│  │  └─test
│  ├─socket
│  └─util
├─pkg
│  ├─fingerprint
│  ├─httpx
│  ├─hydra
│  ├─jndi
│  ├─kscan
│  ├─ksubdomain
│  └─naabu
│
├─pocs_go
├─pocs_yml
├─projectdiscovery
├─spider
├─static
├─test
├─tools
├─vendor
└─webScan
    ├─config
    └─Functions
```

## 02-官方包库

- [ ] [archive/tar](https://pkg.go.dev/archive/tar)
- [ ] [archive/zip](https://pkg.go.dev/archive/zip)
- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
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
- [ ] [math](https://pkg.go.dev/math)
- [ ] [math/big](https://pkg.go.dev/math/big)
- [ ] [math/bits](https://pkg.go.dev/math/bits)
- [ ] [math/rand](https://pkg.go.dev/math/rand)
- [ ] [mime](https://pkg.go.dev/mime)
- [ ] [mime/multipart](https://pkg.go.dev/mime/multipart)
- [ ] [mime/quotedprintable](https://pkg.go.dev/mime/quotedprintable)
- [ ] [moul.io/http2curl](https://pkg.go.dev/moul.io/http2curl)
- [ ] [net](https://pkg.go.dev/net)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/http/cookiejar](https://pkg.go.dev/net/http/cookiejar)
- [ ] [net/http/httptest](https://pkg.go.dev/net/http/httptest)
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/http/httputil](https://pkg.go.dev/net/http/httputil)
- [ ] [net/http/internal](https://pkg.go.dev/net/http/internal)
- [ ] [net/http/internal/ascii](https://pkg.go.dev/net/http/internal/ascii)
- [ ] [net/http/internal/testcert](https://pkg.go.dev/net/http/internal/testcert)
- [ ] [net/http/pprof](https://pkg.go.dev/net/http/pprof)
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
- [ ] [runtime/cgo](https://pkg.go.dev/runtime/cgo)
- [ ] [runtime/debug](https://pkg.go.dev/runtime/debug)
- [ ] [runtime/internal/atomic](https://pkg.go.dev/runtime/internal/atomic)
- [ ] [runtime/internal/math](https://pkg.go.dev/runtime/internal/math)
- [ ] [runtime/internal/sys](https://pkg.go.dev/runtime/internal/sys)
- [ ] [runtime/pprof](https://pkg.go.dev/runtime/pprof)
- [ ] [runtime/trace](https://pkg.go.dev/runtime/trace)
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
- [ ] https://github.com/akrylysov/pogreb/fs
- [ ] https://github.com/akrylysov/pogreb/internal/errors
- [ ] https://github.com/akrylysov/pogreb/internal/hash
- [ ] https://github.com/alecthomas/jsonschema
- [ ] https://github.com/alecthomas/template
- [ ] https://github.com/alecthomas/template/parse
- [ ] https://github.com/alecthomas/units
- [ ] https://github.com/ammario/ipisp/v2
- [ ] https://github.com/AndreasBriese/bbloom
- [ ] https://github.com/andres-erbsen/clock
- [ ] https://github.com/andybalholm/cascadia
- [ ] https://github.com/andygrunwald/go-jira
- [ ] https://github.com/antchfx/htmlquery
- [ ] https://github.com/antchfx/xmlquery
- [ ] https://github.com/antchfx/xpath
- [ ] https://github.com/antlabs/strsim
- [ ] https://github.com/antlabs/strsim/similarity
- [ ] https://github.com/antlr/antlr4/runtime/Go/antlr
- [ ] https://github.com/apex/log
- [ ] https://github.com/asaskevich/govalidator
- [ ] https://github.com/aws/aws-sdk-go/aws
- [ ] https://github.com/aymerick/douceur/css
- [ ] https://github.com/aymerick/douceur/parser
- [ ] https://github.com/Azure/go-ntlmssp
- [ ] https://github.com/bits-and-blooms/bitset
- [ ] https://github.com/bits-and-blooms/bloom/v3
- [ ] https://github.com/blang/semver
- [ ] https://github.com/bluele/gcache
- [ ] https://github.com/c4milo/unpackit
- [ ] https://github.com/caddyserver/certmagic
- [ ] https://github.com/cespare/xxhash/v2
- [ ] https://github.com/ChrisTrenkamp/goxpath
- [ ] https://github.com/cnf/structhash
- [ ] https://github.com/cockroachdb/errors
- [ ] https://github.com/cockroachdb/logtags
- [ ] https://github.com/cockroachdb/pebble
- [ ] https://github.com/cockroachdb/redact
- [ ] https://github.com/cockroachdb/sentry-go
- [ ] https://github.com/codegangsta/inject
- [ ] https://github.com/corpix/uarand
- [ ] https://github.com/DataDog/zstd
- [ ] https://github.com/denisenkom/go-mssqldb
- [ ] https://github.com/dgraph-io/badger
- [ ] https://github.com/dgraph-io/badger/options
- [ ] https://github.com/dgraph-io/badger/pb
- [ ] https://github.com/dgraph-io/badger/skl
- [ ] https://github.com/dgraph-io/badger/table
- [ ] https://github.com/dgraph-io/badger/trie
- [ ] https://github.com/dgraph-io/badger/y
- [ ] https://github.com/dgraph-io/ristretto/z
- [ ] https://github.com/dgraph-io/ristretto/z/simd
- [ ] https://github.com/dimchansky/utfbom
- [ ] https://github.com/dlclark/regexp2
- [ ] https://github.com/dlclark/regexp2/syntax
- [ ] https://github.com/docker/go-units
- [ ] https://github.com/dsnet/compress
- [ ] https://github.com/dsnet/compress/bzip2
- [ ] https://github.com/dsnet/compress/bzip2/internal/sais
- [ ] https://github.com/dsnet/compress/internal
- [ ] https://github.com/dsnet/compress/internal/errors
- [ ] https://github.com/dsnet/compress/internal/prefix
- [ ] https://github.com/dustin/go-humanize
- [ ] https://github.com/fatih/structs
- [ ] https://github.com/fsnotify/fsnotify
- [ ] https://github.com/goburrow/cache
- [ ] https://github.com/gobwas/httphead
- [ ] https://github.com/gobwas/pool
- [ ] https://github.com/gobwas/pool/internal/pmath
- [ ] https://github.com/gobwas/pool/pbufio
- [ ] https://github.com/gobwas/pool/pbytes
- [ ] https://github.com/gobwas/ws
- [ ] https://github.com/gobwas/ws/wsutil
- [ ] https://github.com/gofrs/uuid
- [ ] https://github.com/gogo/protobuf/proto
- [ ] https://github.com/gogo/protobuf/sortkeys
- [ ] https://github.com/gogo/protobuf/types
- [ ] https://github.com/golang/glog
- [ ] https://github.com/golang/groupcache/lru
- [ ] https://github.com/golang/protobuf/proto
- [ ] https://github.com/golang/snappy
- [ ] https://github.com/golang-jwt/jwt/v4
- [ ] https://github.com/golang-sql/civil
- [ ] https://github.com/golang-sql/sqlexp
- [ ] https://github.com/google/cel-go/cel
- [ ] https://github.com/google/cel-go/checker
- [ ] https://github.com/google/cel-go/checker/decls
- [ ] https://github.com/google/cel-go/common
- [ ] https://github.com/google/cel-go/interpreter
- [ ] https://github.com/google/cel-go/interpreter/functions
- [ ] https://github.com/google/cel-go/parser
- [ ] https://github.com/google/cel-go/parser/gen
- [ ] https://github.com/google/go-github/github
- [ ] https://github.com/google/gopacket
- [ ] https://github.com/google/gopacket/layers
- [ ] https://github.com/google/go-querystring/query
- [ ] https://github.com/google/uuid
- [ ] https://github.com/go-ole/go-ole
- [ ] https://github.com/go-ole/go-ole/oleutil
- [ ] https://github.com/go-playground/locales
- [ ] https://github.com/go-playground/locales/currency
- [ ] https://github.com/go-playground/universal-translator
- [ ] https://github.com/go-playground/validator/v10
- [ ] https://github.com/gorilla/css/scanner
- [ ] https://github.com/gorilla/websocket
- [ ] https://github.com/go-rod/rod
- [ ] https://github.com/go-rod/rod/lib/assets
- [ ] https://github.com/go-rod/rod/lib/cdp
- [ ] https://github.com/go-rod/rod/lib/defaults
- [ ] https://github.com/go-rod/rod/lib/devices
- [ ] https://github.com/go-rod/rod/lib/input
- [ ] https://github.com/go-rod/rod/lib/js
- [ ] https://github.com/go-rod/rod/lib/launcher
- [ ] https://github.com/go-rod/rod/lib/launcher/flags
- [ ] https://github.com/go-rod/rod/lib/proto
- [ ] https://github.com/go-rod/rod/lib/utils
- [ ] https://github.com/go-routeros/routeros
- [ ] https://github.com/go-routeros/routeros/proto
- [ ] https://github.com/gosnmp/gosnmp
- [ ] https://github.com/go-sql-driver/mysql
- [ ] https://github.com/gosuri/uilive
- [ ] https://github.com/gosuri/uiprogress
- [ ] https://github.com/gosuri/uiprogress/util/strutil
- [ ] https://github.com/h2non/filetype
- [ ] https://github.com/h2non/filetype/matchers
- [ ] https://github.com/h2non/filetype/matchers/isobmff
- [ ] https://github.com/h2non/filetype/types
- [ ] https://github.com/hako/durafmt
- [ ] https://github.com/hashicorp/errwrap
- [ ] https://github.com/hashicorp/go-cleanhttp
- [ ] https://github.com/hashicorp/go-multierror
- [ ] https://github.com/hashicorp/go-retryablehttp
- [ ] https://github.com/hashicorp/go-uuid
- [ ] https://github.com/hashicorp/go-version
- [ ] https://github.com/hashicorp/hcl
- [ ] https://github.com/hbakhtiyor/strsim
- [ ] https://github.com/hktalent/jarm-go
- [ ] https://github.com/huin/asn1ber
- [ ] https://github.com/iancoleman/orderedmap
- [ ] https://github.com/icodeface/tls
- [ ] https://github.com/itchyny/gojq
- [ ] https://github.com/itchyny/timefmt-go
- [ ] https://github.com/jcmturner/aescts/v2
- [ ] https://github.com/jcmturner/dnsutils/v2
- [ ] https://github.com/jcmturner/gofork/encoding/asn1
- [ ] https://github.com/jcmturner/gofork/x/crypto/pbkdf2
- [ ] https://github.com/jcmturner/goidentity/v6
- [ ] https://github.com/jcmturner/gokrb5
- [ ] https://github.com/jcmturner/rpc/v2/mstypes
- [ ] https://github.com/jcmturner/rpc/v2/ndr
- [ ] https://github.com/jinzhu/inflection
- [ ] https://github.com/jinzhu/now
- [ ] https://github.com/jlaffaye/ftp
- [ ] https://github.com/jmespath/go-jmespath
- [ ] https://github.com/josharian/intern
- [ ] https://github.com/json-iterator/go
- [ ] https://github.com/karlseguin/ccache
- [ ] https://github.com/klauspost/compress
- [ ] https://github.com/klauspost/cpuid/v2
- [ ] https://github.com/klauspost/pgzip
- [ ] https://github.com/Knetic/govaluate
- [ ] https://github.com/kr/pretty
- [ ] https://github.com/kr/text
- [ ] https://github.com/lcvvvv/gonmap
- [ ] https://github.com/leodido/go-urn
- [ ] https://github.com/lib/pq
- [ ] https://github.com/lib/pq/oid
- [ ] https://github.com/lib/pq/scram
- [ ] https://github.com/libdns/libdns
- [ ] https://github.com/logrusorgru/aurora
- [ ] https://github.com/lor00x/goldap/message
- [ ] https://github.com/lunixbochs/struc
- [ ] https://github.com/magiconair/properties
- [ ] https://github.com/mailru/easyjson
- [ ] https://github.com/mailru/easyjson/buffer
- [ ] https://github.com/mailru/easyjson/jlexer
- [ ] https://github.com/mailru/easyjson/jwriter
- [ ] https://github.com/masterzen/simplexml/dom
- [ ] https://github.com/masterzen/winrm
- [ ] https://github.com/masterzen/winrm/soap
- [ ] https://github.com/mattn/go-isatty
- [ ] https://github.com/mattn/go-runewidth
- [ ] https://github.com/mattn/go-sqlite3
- [ ] https://github.com/mfonda/simhash
- [ ] https://github.com/mholt/acmez
- [ ] https://github.com/mholt/acmez/acme
- [ ] https://github.com/mholt/archiver
- [ ] https://github.com/microcosm-cc/bluemonday
- [ ] https://github.com/microcosm-cc/bluemonday/css
- [ ] https://github.com/miekg/dns
- [ ] https://github.com/mitchellh/go-homedir
- [ ] https://github.com/mitchellh/mapstructure
- [ ] https://github.com/modern-go/concurrent
- [ ] https://github.com/modern-go/reflect2
- [ ] https://github.com/montanaflynn/stats
- [ ] https://github.com/Mzack9999/go-http-digest-auth-client
- [ ] https://github.com/Mzack9999/ldapserver
- [ ] https://github.com/nwaples/rardecode
- [ ] https://github.com/olekukonko/tablewriter
- [ ] https://github.com/olivere/elastic
- [ ] https://github.com/olivere/elastic/config
- [ ] https://github.com/olivere/elastic/uritemplates
- [ ] https://github.com/openrdap/rdap
- [ ] https://github.com/openrdap/rdap/bootstrap
- [ ] https://github.com/openrdap/rdap/bootstrap/cache
- [ ] https://github.com/openrdap/rdap/sandbox
- [ ] https://github.com/owenrumney/go-sarif/v2/sarif
- [ ] https://github.com/patrickmn/go-cache
- [ ] https://github.com/pelletier/go-toml/v2
- [ ] https://github.com/phayes/freeport
- [ ] https://github.com/pierrec/lz4
- [ ] https://github.com/pierrec/lz4/internal/xxh32
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/projectdiscovery/blackrock
- [ ] https://github.com/projectdiscovery/cdncheck
- [ ] https://github.com/projectdiscovery/chaos-client/pkg/chaos
- [ ] https://github.com/projectdiscovery/clistats
- [ ] https://github.com/projectdiscovery/cryptoutil
- [ ] https://github.com/projectdiscovery/dnsx/libs/dnsx
- [ ] https://github.com/projectdiscovery/fastdialer/fastdialer
- [ ] https://github.com/projectdiscovery/fdmax/autofdmax
- [ ] https://github.com/projectdiscovery/filekv
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
- [ ] https://github.com/projectdiscovery/nuclei-updatecheck-api/client
- [ ] https://github.com/projectdiscovery/rawhttp
- [ ] https://github.com/projectdiscovery/rawhttp/client
- [ ] https://github.com/projectdiscovery/rawhttp/clientpipeline
- [ ] https://github.com/projectdiscovery/rawhttp/proxy
- [ ] https://github.com/projectdiscovery/reflectutil
- [ ] https://github.com/projectdiscovery/retryabledns
- [ ] https://github.com/projectdiscovery/retryabledns/doh
- [ ] https://github.com/projectdiscovery/retryabledns/hostsfile
- [ ] https://github.com/projectdiscovery/retryablehttp-go
- [ ] https://github.com/projectdiscovery/sliceutil
- [ ] https://github.com/projectdiscovery/stringsutil
- [ ] https://github.com/projectdiscovery/subfinder
- [ ] https://github.com/projectdiscovery/uncover/uncover
- [ ] https://github.com/projectdiscovery/uncover/uncover/agent/shodanidb
- [ ] https://github.com/projectdiscovery/urlutil
- [ ] https://github.com/projectdiscovery/wappalyzergo
- [ ] https://github.com/projectdiscovery/yamldoc-go/encoder
- [ ] https://github.com/PuerkitoBio/goquery
- [ ] https://github.com/remeh/sizedwaitgroup
- [ ] https://github.com/rivo/uniseg
- [ ] https://github.com/rogpeppe/go-internal/fmtsort
- [ ] https://github.com/rs/xid
- [ ] https://github.com/saintfish/chardet
- [ ] https://github.com/satori/go.uuid
- [ ] https://github.com/segmentio/ksuid
- [ ] https://github.com/shirou/gopsutil/v3/cpu
- [ ] https://github.com/shirou/gopsutil/v3/disk
- [ ] https://github.com/shirou/gopsutil/v3/host
- [ ] https://github.com/shirou/gopsutil/v3/internal/common
- [ ] https://github.com/shirou/gopsutil/v3/mem
- [ ] https://github.com/shirou/gopsutil/v3/net
- [ ] https://github.com/shirou/gopsutil/v3/process
- [ ] https://github.com/sijms/go-ora/v2
- [ ] https://github.com/sijms/go-ora/v2/advanced_nego
- [ ] https://github.com/sijms/go-ora/v2/advanced_nego/ntlmssp
- [ ] https://github.com/sijms/go-ora/v2/converters
- [ ] https://github.com/sijms/go-ora/v2/network
- [ ] https://github.com/sijms/go-ora/v2/network/security
- [ ] https://github.com/sijms/go-ora/v2/network/security/md4
- [ ] https://github.com/sijms/go-ora/v2/trace
- [ ] https://github.com/simonnilsson/ask
- [ ] https://github.com/spaolacci/murmur3
- [ ] https://github.com/spf13/afero
- [ ] https://github.com/spf13/afero/internal/common
- [ ] https://github.com/spf13/afero/mem
- [ ] https://github.com/spf13/cast
- [ ] https://github.com/spf13/jwalterweatherman
- [ ] https://github.com/spf13/pflag
- [ ] https://github.com/spf13/viper
- [ ] https://github.com/spf13/viper/internal/encoding
- [ ] https://github.com/stacktitan/smb/gss
- [ ] https://github.com/stacktitan/smb/ntlmssp
- [ ] https://github.com/stacktitan/smb/smb
- [ ] https://github.com/stacktitan/smb/smb/encoder
- [ ] https://github.com/stoewer/go-strcase
- [ ] https://github.com/subosito/gotenv
- [ ] https://github.com/syndtr/goleveldb/leveldb
- [ ] https://github.com/tj/go-update
- [ ] https://github.com/tj/go-update/progress
- [ ] https://github.com/tj/go-update/stores/github
- [ ] https://github.com/tomnomnom/linkheader
- [ ] https://github.com/trivago/tgo/tcontainer
- [ ] https://github.com/trivago/tgo/treflect
- [ ] https://github.com/txthinking/runnergroup
- [ ] https://github.com/txthinking/socks5
- [ ] https://github.com/txthinking/x
- [ ] https://github.com/ulikunitz/xz
- [ ] https://github.com/ulikunitz/xz/internal/hash
- [ ] https://github.com/ulikunitz/xz/internal/xlog
- [ ] https://github.com/ulikunitz/xz/lzma
- [ ] https://github.com/ulule/deepcopier
- [ ] https://github.com/valyala/bytebufferpool
- [ ] https://github.com/valyala/fasttemplate
- [ ] https://github.com/weppos/publicsuffix-go/publicsuffix
- [ ] https://github.com/xanzy/go-gitlab
- [ ] https://github.com/xdg-go/pbkdf2
- [ ] https://github.com/xdg-go/scram
- [ ] https://github.com/xdg-go/stringprep
- [ ] https://github.com/xi2/xz
- [ ] https://github.com/yl2chen/cidranger
- [ ] https://github.com/yl2chen/cidranger/net
- [ ] https://github.com/youmark/pkcs8
- [ ] https://github.com/ysmood/goob
- [ ] https://github.com/ysmood/gson
- [ ] https://github.com/ysmood/leakless
- [ ] https://github.com/ysmood/leakless/pkg/shared
- [ ] https://github.com/ysmood/leakless/pkg/utils
- [ ] https://github.com/yusufpapurcu/wmi
- [ ] https://github.com/zmap/rc2
- [ ] https://github.com/zmap/zcrypto/dsa
- [ ] https://github.com/zmap/zcrypto/encoding/asn1
- [ ] https://github.com/zmap/zcrypto/internal/randutil
- [ ] https://github.com/zmap/zcrypto/json
- [ ] https://github.com/zmap/zcrypto/tls
- [ ] https://github.com/zmap/zcrypto/util
- [ ] https://github.com/zmap/zcrypto/x509
- [ ] https://github.com/zmap/zcrypto/x509/ct
- [ ] https://github.com/zmap/zcrypto/x509/pkix
- [ ] https://go.etcd.io/bbolt
- [ ] https://go.mongodb.org/mongo-driver/bson
- [ ] https://go.uber.org/atomic
- [ ] https://go.uber.org/multierr
- [ ] https://go.uber.org/ratelimit
- [ ] https://go.uber.org/zap
- [ ] https://go.uber.org/zap/buffer
- [ ] https://go.uber.org/zap/internal
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
- [ ] https://gorm.io/driver/sqlite
- [ ] https://gorm.io/gorm
- [ ] https://gorm.io/gorm/callbacks
- [ ] https://gorm.io/gorm/clause
- [ ] https://gorm.io/gorm/logger
- [ ] https://gorm.io/gorm/migrator
- [ ] https://gorm.io/gorm/schema
- [ ] https://gorm.io/gorm/utils

## 04-开发设计

本部分尽可能的列举出scan4all开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Erscan4all