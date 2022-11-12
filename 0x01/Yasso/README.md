# [Yasso](https://github.com/sairson/Yasso)代码分析

本文将从代码层面深入分析Yasso项目。Yasso是sairson开发的一款强大的内网渗透辅助工具集。支持rdp，ssh，redis，postgres，mongodb，mssql，mysql，winrm等服务爆破，快速的端口扫描，web指纹识别，各种内置服务的一键利用，包括ssh完全交互式登陆，mssql提权，redis一键利用，mysql数据库查询，winrm横向利用，多种服务利用支持socks5代理执行。

本文创建于2022年11月12日，最近的一次更新时间为2022年11月12日。

- [01-项目结构]()
- [02-官方包库]()
- [02-第三方库]()
- [03-代码分析]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  example.txt
│  main.go
│  result.txt
│    
├─cmd
│      cmd.go
│      
├─config
│  │  config.go
│  │  rules.go
│  │  
│  └─banner
│          banner.go
│          
├─core
│  ├─brute
│  │      brute.go
│  │      
│  ├─flag
│  │      flag.go
│  │      
│  ├─logger
│  │      logger.go
│  │      
│  ├─parse
│  │      parse_ip.go
│  │      parse_port.go
│  │      
│  ├─plugin
│  │      all.go
│  │      brute.go
│  │      dcerpc.go
│  │      eternalblue.go
│  │      ftp.go
│  │      grdp.go
│  │      icmp.go
│  │      memcache.go
│  │      mongo.go
│  │      mssql.go
│  │      mysql.go
│  │      nbnsscan.go
│  │      nbns_test.go
│  │      oxidscan.go
│  │      postgres.go
│  │      ps.go
│  │      redis.go
│  │      rmi.go
│  │      smb.go
│  │      smbghost.go
│  │      smbscan.go
│  │      ssh.go
│  │      winrm.go
│  │      zookeeper.go
│  │      
│  └─utils
│          utils.go
│          
├─example
│      tcp_smb_test.go
│      tcp_ssh_test.go
│      
└─pkg
    ├─exploit
    │  │  exploit.go
    │  │  
    │  ├─config
    │  │      config.go
    │  │      tools.go
    │  │      
    │  ├─ldap
    │  │  │  ldap.go
    │  │  │  
    │  │  └─core
    │  │      ├─filter
    │  │      │      filter.go
    │  │      │      
    │  │      └─query
    │  │              flags.go
    │  │              object.go
    │  │              query.go
    │  │              resolver.go
    │  │              
    │  ├─mssql
    │  │  │  mssql.go
    │  │  │  
    │  │  └─static
    │  │          SharpSQLKit.txt
    │  │          
    │  ├─redis
    │  │  │  redis.go
    │  │  │  
    │  │  └─static
    │  │          exp.so
    │  │          
    │  ├─ssh
    │  │      ssh.go
    │  │      
    │  ├─sunlogin
    │  │      sunlogin.go
    │  │      sunlogin_test.go
    │  │      
    │  └─winrm
    │          winrm.go
    │          
    ├─grdp
    │  │  grdp.go
    │  │  grdp_test.go
    │  │  LICENSE
    │  │  README.md
    │  │  
    │  ├─core
    │  │      io.go
    │  │      io_test.go
    │  │      rle.go
    │  │      rle_test.go
    │  │      socket.go
    │  │      types.go
    │  │      util.go
    │  │      
    │  ├─emission
    │  │      emitter.go
    │  │      
    │  ├─glog
    │  │      log.go
    │  │      
    │  └─protocol
    │      ├─lic
    │      │      lic.go
    │      │      
    │      ├─nla
    │      │      cssp.go
    │      │      encode.go
    │      │      encode_test.go
    │      │      ntlm.go
    │      │      ntlm_test.go
    │      │      
    │      ├─pdu
    │      │      caps.go
    │      │      cliprdr.go
    │      │      data.go
    │      │      pdu.go
    │      │      
    │      ├─rfb
    │      │      rfb.go
    │      │      
    │      ├─sec
    │      │      sec.go
    │      │      
    │      ├─t125
    │      │  │  mcs.go
    │      │  │  
    │      │  ├─ber
    │      │  │      ber.go
    │      │  │      
    │      │  ├─gcc
    │      │  │      gcc.go
    │      │  │      
    │      │  └─per
    │      │          per.go
    │      │          
    │      ├─tpkt
    │      │      tpkt.go
    │      │      
    │      └─x224
    │              x224.go
    │              
    ├─netspy
    │  ├─example
    │  │      ip_test.go
    │  │      spy_test.go
    │  │      
    │  ├─icmp
    │  │      icmp.go
    │  │      
    │  └─spy
    │          spy.go
    │          
    ├─report
    │      report.go
    │      
    └─webscan
            dismap.go
            http.go
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/aes](https://pkg.go.dev/crypto/aes)
- [ ] [crypto/hmac](https://pkg.go.dev/crypto/hmac)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/rand](https://pkg.go.dev/crypto/rand)
- [ ] [crypto/rc4](https://pkg.go.dev/crypto/rc4)
- [ ] [crypto/rsa](https://pkg.go.dev/crypto/rsa)
- [ ] [crypto/sha1](https://pkg.go.dev/crypto/sha1)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [database/sql](https://pkg.go.dev/database/sql)
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
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
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/exec](https://pkg.go.dev/os/exec)
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
- [ ] [text/template](https://pkg.go.dev/text/template)
- [ ] [text/template/parse](https://pkg.go.dev/text/template/parse)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode/utf16](https://pkg.go.dev/unicode/utf16)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] https://github.com/audibleblink/bamflags
- [ ] https://github.com/Azure/go-ntlmssp
- [ ] https://github.com/bwmarrin/go-objectsid
- [ ] https://github.com/cespare/xxhash/v2
- [ ] https://github.com/ChrisTrenkamp/goxpath
- [ ] https://github.com/denisenkom/go-mssqldb
- [ ] https://github.com/dgryski/go-rendezvous
- [ ] https://github.com/gofrs/uuid
- [ ] https://github.com/golang-sql/civil
- [ ] https://github.com/golang-sql/sqlexp
- [ ] https://github.com/gookit/color
- [ ] https://github.com/go-redis/redis/v8
- [ ] https://github.com/go-sql-driver/mysql
- [ ] https://github.com/hashicorp/errwrap
- [ ] https://github.com/hashicorp/go-multierror
- [ ] https://github.com/hashicorp/go-uuid
- [ ] https://github.com/huin/asn1ber
- [ ] https://github.com/icodeface/tls
- [ ] https://github.com/inconshreveable/mousetrap
- [ ] https://github.com/jedib0t/go-pretty/v6/table
- [ ] https://github.com/jedib0t/go-pretty/v6/text
- [ ] https://github.com/jlaffaye/ftp
- [ ] https://github.com/json-iterator/go
- [ ] https://github.com/lunixbochs/struc
- [ ] https://github.com/masterzen/simplexml/dom
- [ ] https://github.com/masterzen/winrm
- [ ] https://github.com/masterzen/winrm/soap
- [ ] https://github.com/mattn/go-runewidth
- [ ] https://github.com/modern-go/concurrent
- [ ] https://github.com/modern-go/reflect2
- [ ] https://github.com/projectdiscovery/cdncheck
- [ ] https://github.com/rivo/uniseg
- [ ] https://github.com/spf13/cobra
- [ ] https://github.com/spf13/pflag
- [ ] https://github.com/stacktitan/smb/gss
- [ ] https://github.com/stacktitan/smb/ntlmssp
- [ ] https://github.com/stacktitan/smb/smb
- [ ] https://github.com/stacktitan/smb/smb/encoder
- [ ] https://github.com/xo/terminfo
- [ ] https://github.com/yl2chen/cidranger
- [ ] https://github.com/yl2chen/cidranger/net
- [ ] https://gopkg.in/asn1-ber.v1
- [ ] https://gopkg.in/ldap.v2
- [ ] https://gopkg.in/mgo.v2
- [ ] https://gopkg.in/mgo.v2/bson
- [ ] https://gopkg.in/mgo.v2/internal/json
- [ ] https://gopkg.in/mgo.v2/internal/scram

## 04-开发设计

本部分尽可能的列举出Yasso开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/ErYasso