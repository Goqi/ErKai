# [x-crack](https://github.com/netxfly/x-crack)代码分析

本文将从代码层面深入分析x-crack项目。x-crack是netxfly开发的一款弱口令扫描工具。支持以下服务协议的弱口令扫描：FTP/SSH/SNMP/MSSQL/MYSQL/PostGreSQL/REDIS/ElasticSearch/MONGODB

本文创建于2022年11月26日，最近的一次更新时间为2022年11月26日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  iplist.txt
│  pass.dic
│  user.dic
│  x-crack.go
│  
├─cmd
│      cmd.go
│      
├─logger
│      log.go
│      
├─models
│      cache.go
│      models.go
│      
├─plugins
│      elastic.go
│      elastic_test.go
│      ftp.go
│      ftp_test.go
│      mongodb.go
│      mongodb_test.go
│      mssql.go
│      mssql_test.go
│      mysql.go
│      mysql_test.go
│      oracle.go
│      oracle_test.go
│      plugins.go
│      postgres.go
│      postgres_test.go
│      redis.go
│      redis_test.go
│      smb.go
│      smb_test.go
│      snmp.go
│      snmp_test.go
│      ssh.go
│      ssh_test.go
│      
├─util
│  │  assets.go
│  │  assets_test.go
│  │  file.go
│  │  file_test.go
│  │  task.go
│  │  task_test.go
│  │  util.go
│  │  
│  └─hash
│          hash.go
│          md5.go
│          
└─vars
        vars.go
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
- [ ] [encoding/gob](https://pkg.go.dev/encoding/gob)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [encoding/pem](https://pkg.go.dev/encoding/pem)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
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

- [ ] https://cloud.google.com/go/civil
- [ ] https://github.com/denisenkom/go-mssqldb
- [ ] https://github.com/golang/mock/gomock
- [ ] https://github.com/go-redis/redis
- [ ] https://github.com/jlaffaye/ftp
- [ ] https://github.com/konsorten/go-windows-terminal-sequences
- [ ] https://github.com/lib/pq
- [ ] https://github.com/mattn/go-colorable
- [ ] https://github.com/mattn/go-isatty
- [ ] https://github.com/mgutz/ansi
- [ ] https://github.com/netxfly/mysql
- [ ] https://github.com/patrickmn/go-cache
- [ ] https://github.com/sirupsen/logrus
- [ ] https://github.com/soniah/gosnmp
- [ ] https://github.com/stacktitan/smb/gss
- [ ] https://github.com/stacktitan/smb/ntlmssp
- [ ] https://github.com/stacktitan/smb/smb
- [ ] https://github.com/stacktitan/smb/smb/encoder
- [ ] https://github.com/urfave/cli
- [ ] https://github.com/x-cray/logrus-prefixed-formatter
- [ ] https://gopkg.in/cheggaaa/pb.v2
- [ ] https://gopkg.in/fatih/color.v1
- [ ] https://gopkg.in/mattn/go-colorable.v0
- [ ] https://gopkg.in/mattn/go-isatty.v0
- [ ] https://gopkg.in/mattn/go-runewidth.v0
- [ ] https://gopkg.in/mgo.v2
- [ ] https://gopkg.in/olivere/elastic.v3
- [ ] https://gopkg.in/olivere/elastic.v3/backoff
- [ ] https://gopkg.in/olivere/elastic.v3/uritemplates
- [ ] https://gopkg.in/VividCortex/ewma.v1

## 04-开发设计

本部分尽可能的列举出x-crack开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型
- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Erxcrack