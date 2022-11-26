# [Taichi](https://github.com/sulab999/Taichi)代码分析

本文将从代码层面深入分析Taichi项目。Taichi是sulab999开发的一款高级交互式渗透测试框架。包括服务弱口令扫描、敏感路径扫描、子域名扫描、漏洞扫描等功能。

本文创建于2022年11月26日，最近的一次更新时间为2022年11月26日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  main.go
│  pass.txt
│  res.txt
│  url.txt
│  user.txt
│  
├─core
│  ├─cli
│  │      completer.go
│  │      executor.go
│  │      
│  ├─enum
│  │  │  brute.go
│  │  │  ftp.go
│  │  │  javadebug.go
│  │  │  mongodb.go
│  │  │  mssql.go
│  │  │  mysql.go
│  │  │  plugins.go
│  │  │  postgres.go
│  │  │  rdp.go
│  │  │  redis.go
│  │  │  smb.go
│  │  │  snmp.go
│  │  │  ssh.go
│  │  │  
│  │  └─goftp
│  │          ftp.go
│  │          status.go
│  │          
│  ├─model
│  │      model.go
│  │      model_scans.go
│  │      
│  ├─scan
│  │  │  port.go
│  │  │  rewg.go
│  │  │  vscan.go
│  │  │  
│  │  └─proberbyte
│  │          proberbyte.go
│  │          
│  └─utils
│          dic.go
│          logger.go
│          net.go
│          utils.go
│          var.go
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/flate](https://pkg.go.dev/compress/flate)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
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
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding](https://pkg.go.dev/encoding)
- [ ] [encoding/asn1](https://pkg.go.dev/encoding/asn1)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/csv](https://pkg.go.dev/encoding/csv)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [encoding/pem](https://pkg.go.dev/encoding/pem)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [github.com/c-bata/go-prompt](https://pkg.go.dev/github.com/c-bata/go-prompt)
- [ ] [github.com/fatih/color](https://pkg.go.dev/github.com/fatih/color)
- [ ] [hash](https://pkg.go.dev/hash)
- [ ] [hash/crc32](https://pkg.go.dev/hash/crc32)
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
- [ ] [runtime/trace](https://pkg.go.dev/runtime/trace)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sulab/core/cli](https://pkg.go.dev/sulab/core/cli)
- [ ] [sulab/core/enum](https://pkg.go.dev/sulab/core/enum)
- [ ] [sulab/core/model](https://pkg.go.dev/sulab/core/model)
- [ ] [sulab/core/scan](https://pkg.go.dev/sulab/core/scan)
- [ ] [sulab/core/scan/proberbyte](https://pkg.go.dev/sulab/core/scan/proberbyte)
- [ ] [sulab/core/utils](https://pkg.go.dev/sulab/core/utils)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [sync/atomic](https://pkg.go.dev/sync/atomic)
- [ ] [syscall](https://pkg.go.dev/syscall)
- [ ] [testing](https://pkg.go.dev/testing)
- [ ] [text/template](https://pkg.go.dev/text/template)
- [ ] [text/template/parse](https://pkg.go.dev/text/template/parse)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode](https://pkg.go.dev/unicode)
- [ ] [unicode/utf16](https://pkg.go.dev/unicode/utf16)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] https://github.com/axgle/mahonia
- [ ] https://github.com/c-bata/go-prompt
- [ ] https://github.com/cespare/xxhash/v2
- [ ] https://github.com/cheggaaa/pb/v3
- [ ] https://github.com/denisenkom/go-mssqldb
- [ ] https://github.com/dgryski/go-rendezvous
- [ ] https://github.com/fatih/color
- [ ] https://github.com/golang-sql/civil
- [ ] https://github.com/golang-sql/sqlexp
- [ ] https://github.com/go-redis/redis/v8
- [ ] https://github.com/gosnmp/gosnmp
- [ ] https://github.com/hashicorp/errwrap
- [ ] https://github.com/hashicorp/go-multierror
- [ ] https://github.com/icodeface/grdp
- [ ] https://github.com/icodeface/tls
- [ ] https://github.com/jinzhu/gorm
- [ ] https://github.com/jinzhu/inflection
- [ ] https://github.com/jlaffaye/ftp
- [ ] https://github.com/lib/pq
- [ ] https://github.com/lib/pq/oid
- [ ] https://github.com/lib/pq/scram
- [ ] https://github.com/lunixbochs/struc
- [ ] https://github.com/mattn/go-colorable
- [ ] https://github.com/mattn/go-isatty
- [ ] https://github.com/mattn/go-runewidth
- [ ] https://github.com/mattn/go-sqlite3
- [ ] https://github.com/mattn/go-tty
- [ ] https://github.com/olekukonko/tablewriter
- [ ] https://github.com/rivo/uniseg
- [ ] https://github.com/stacktitan/smb
- [ ] https://github.com/VividCortex/ewma
- [ ] https://gopkg.in/mgo.v2
- [ ] https://gopkg.in/mgo.v2/bson
- [ ] https://gopkg.in/mgo.v2/internal/json
- [ ] https://gopkg.in/mgo.v2/internal/scram

## 04-开发设计

本部分尽可能的列举出Taichi开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型
- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/ErTaichi