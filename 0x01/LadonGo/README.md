# [LadonGo](https://github.com/k8gege/LadonGo)代码分析

本文将从代码层面深入分析LadonGo项目。LadonGo是k8gege开发的内网渗透扫描器框架，使用它可轻松一键批量探测C段、B段、A段存活主机、高危漏洞检测MS17010、SmbGhost，远程执行SSH/Winrm，密码爆破，端口扫描服务识别PortScan指纹识别等功能。

本文创建于2022年11月14日，最近的一次更新时间为2022年11月14日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  install.go
│  ip.txt
│  ip24.txt
│  Ladon.go
│  MSSQLSCAN.Log
│  MYSQLSCAN.Log
│  ORACLESCAN.Log
│  pass.txt
│  port.log
│  README.md
│  REDISSCAN.Log
│  ROUTEROSSCAN.Log
│  SMBSCAN.Log
│  SQLPLUSSCAN.Log
│  SSHSCAN.Log
│  update.txt
│  url.txt
│  user.txt
│  userpass.txt
│  WINRMSCAN.Log
│      
├─color
│      color.go
│      
├─dcom
│      oxid.go
│      
├─dic
│      dic.go
│      
├─example
│      mysqltest.go
│      pingtest.go
│      rostest.go
│      smbghost.go
│      smbtest.go
│      sshtest.go
│      
├─exp
│      CVE-2018-14847.go
│      evilarc.go
│      phpstudydoor.go
│      userextract.go
│      winboxexploit.go
│      
├─ftp
│      ftpauth.go
│      
├─goftp
│      ftp.go
│      status.go
│      
├─http
│      basicauth.go
│      gethtml.go
│      httpheader.go
│      httpwork.go
│      
├─icmp
│      icmp.go
│      
├─info
│      info.go
│      
├─lexec
│      lexec.go
│      
├─logger
│      logger.go
│      
├─mongodb
│      mongodb.go
│      
├─mssql
│      mssql.go
│      
├─mysql
│      mysql.go
│      
├─nbt
│      ip.go
│      nbt.go
│      probe_netbios.go
│      utils.go
│      
├─oracle
│      oracle.go
│      oracle.go2
│      sqlplus.go
│      
├─ping
│      ping.go
│      
├─port
│      port.go
│      
├─redis
│      redis_auth.go
│      
├─rexec
│      jspshell.go
│      phpshell.go
│      webshell.go
│      winrmcmd.go
│      
├─routeros
│      routeros.go
│      
├─smb
│      ms17010.go
│      smb.go
│      SmbGhost.go
│      
├─snmp
│      snmp.go
│      
├─ssh
│      ssh.go
│      
├─str
│      str.go
│      
├─t3
│      t3.go
│      
├─tcp
│      tcpBanner.go
│      
├─vul
│      CVE-2021-21972.go
│      CVE_2021_26855.go
│      
├─winrm
│      winrm.go
│      
└─worker
        worker.go
```

## 02-官方包库

- [ ] [archive/tar](https://pkg.go.dev/archive/tar)
- [ ] [archive/zip](https://pkg.go.dev/archive/zip)
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
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [encoding/pem](https://pkg.go.dev/encoding/pem)
- [ ] [encoding/xml](https://pkg.go.dev/encoding/xml)
- [ ] [errors](https://pkg.go.dev/errors)
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
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/http/internal](https://pkg.go.dev/net/http/internal)
- [ ] [net/http/internal/ascii](https://pkg.go.dev/net/http/internal/ascii)
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
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [sync/atomic](https://pkg.go.dev/sync/atomic)
- [ ] [syscall](https://pkg.go.dev/syscall)
- [ ] [text/template](https://pkg.go.dev/text/template)
- [ ] [text/template/parse](https://pkg.go.dev/text/template/parse)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode](https://pkg.go.dev/unicode)
- [ ] [unicode/utf16](https://pkg.go.dev/unicode/utf16)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] https://github.com/alouca/gologger
- [ ] https://github.com/alouca/gosnmp
- [ ] https://github.com/armon/go-socks5
- [ ] https://github.com/Azure/go-ntlmssp
- [ ] https://github.com/ChrisTrenkamp/goxpath
- [ ] https://github.com/denisenkom/go-mssqldb
- [ ] https://github.com/fatih/color
- [ ] https://github.com/godror/godror
- [ ] https://github.com/gofrs/uuid
- [ ] https://github.com/golang-sql/civil
- [ ] https://github.com/go-logfmt/logfmt
- [ ] https://github.com/go-routeros/routeros
- [ ] https://github.com/go-sql-driver/mysql
- [ ] https://github.com/masterzen/simplexml/dom
- [ ] https://github.com/masterzen/winrm
- [ ] https://github.com/mattn/go-colorable
- [ ] https://github.com/mattn/go-isatty
- [ ] https://github.com/monnand/goredis
- [ ] https://github.com/stacktitan/smb


## 04-开发设计

本部分尽可能的列举出LadonGo开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

- 项目结构较为混乱

## 06-二开计划

- https://github.com/Goqi/ErLadonGo