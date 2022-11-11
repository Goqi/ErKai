# [kscan](https://github.com/lcvvvv/kscan)代码分析

本文将从代码层面深入分析kscan项目。kscan是lcvvvv开发的一款全方位扫描器，具备端口扫描、协议检测、指纹识别，暴力破解等功能。支持协议1200+，协议指纹10000+，应用指纹20000+，暴力破解协议10余种。

本文创建于2022年11月11日，最近的一次更新时间为2022年11月11日。

- [01-项目结构]()
- [02-官方包库]()
- [02-第三方库]()
- [03-代码分析]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  kscan.go
│      
├─app
│      type-args.go
│      type-config.go
│      
├─assets
│      
├─core
│  ├─cdn
│  │      cdn.go
│  │      
│  ├─fofa
│  │      fofa.go
│  │      fofa_test.go
│  │      
│  ├─hydra
│  │  │  cracker.go
│  │  │  default_ftp_authlist.go
│  │  │  default_mssql_authlist.go
│  │  │  default_mysql_authlist.go
│  │  │  default_oracle_authlist.go
│  │  │  default_postgresql_authlist.go
│  │  │  default_rdp_authlist.go
│  │  │  default_redis_authlist.go
│  │  │  default_smb_authlist.go
│  │  │  default_ssh_authlist.go
│  │  │  default_telnet_authlist.go
│  │  │  defuault_mongodb_authlist.go
│  │  │  hydra.go
│  │  │  type-auth.go
│  │  │  type-authinfo.go
│  │  │  type-authlist.go
│  │  │  
│  │  ├─ftp
│  │  │      ftp.go
│  │  │      
│  │  ├─mongodb
│  │  │      mongodb.go
│  │  │      
│  │  ├─mssql
│  │  │      mssql.go
│  │  │      
│  │  ├─mysql
│  │  │      mysql.go
│  │  │      
│  │  ├─oracle
│  │  │      oracle.go
│  │  │      oracle_test.go
│  │  │      
│  │  ├─postgresql
│  │  │      postgresql.go
│  │  │      
│  │  ├─rdp
│  │  │      grdp.go
│  │  │      
│  │  ├─redis
│  │  │      redis.go
│  │  │      
│  │  ├─smb
│  │  │      smb.go
│  │  │      smb_test.go
│  │  │      
│  │  ├─ssh
│  │  │      ssh.go
│  │  │      
│  │  └─telnet
│  │          telnet.go
│  │          
│  ├─scanner
│  │      type-client-domain.go
│  │      type-client-hydra.go
│  │      type-client-ip.go
│  │      type-client-port.go
│  │      type-client.go
│  │      type-clinet-url.go
│  │      type-config.go
│  │      type-response.go
│  │      
│  ├─slog
│  │      slog.go
│  │      
│  ├─spy
│  │      spy.go
│  │      
│  └─tips
│          tips.go
│          
├─lib
│  ├─appfinger
│  │  │  appfinger.go
│  │  │  appfinger_test.go
│  │  │  database.go
│  │  │  go.mod
│  │  │  go.sum
│  │  │  type-banner.go
│  │  │  type-fingerprint.go
│  │  │  
│  │  ├─gorpc
│  │  │      rpc.go
│  │  │      rpc_test.go
│  │  │      
│  │  ├─httpfinger
│  │  │      http.go
│  │  │      
│  │  └─iconhash
│  │          iconhash.go
│  │          
│  ├─color
│  │      color.go
│  │      color_test.go
│  │      
│  ├─dns
│  │      dns.go
│  │      dns_test.go
│  │      
│  ├─fofa
│  │      fofa.go
│  │      type-result.go
│  │      
│  ├─gosmb
│  │      gosmb.go
│  │      gosmb_test.go
│  │      
│  ├─gotelnet
│  │      telnet.go
│  │      telnet_test.go
│  │      
│  ├─grdp
│  │  │  grdp.go
│  │  │  grdp_test.go
│  │  │  LICENSE
│  │  │  README.md
│  │  │  
│  │  ├─core
│  │  │      io.go
│  │  │      io_test.go
│  │  │      rle.go
│  │  │      rle_test.go
│  │  │      socket.go
│  │  │      types.go
│  │  │      util.go
│  │  │      
│  │  ├─emission
│  │  │      emitter.go
│  │  │      
│  │  ├─glog
│  │  │      log.go
│  │  │      
│  │  └─protocol
│  │      ├─lic
│  │      │      lic.go
│  │      │      
│  │      ├─nla
│  │      │      cssp.go
│  │      │      encode.go
│  │      │      encode_test.go
│  │      │      ntlm.go
│  │      │      ntlm_test.go
│  │      │      
│  │      ├─pdu
│  │      │      caps.go
│  │      │      cliprdr.go
│  │      │      data.go
│  │      │      pdu.go
│  │      │      
│  │      ├─rfb
│  │      │      rfb.go
│  │      │      
│  │      ├─sec
│  │      │      sec.go
│  │      │      
│  │      ├─t125
│  │      │  │  mcs.go
│  │      │  │  
│  │      │  ├─ber
│  │      │  │      ber.go
│  │      │  │      
│  │      │  ├─gcc
│  │      │  │      gcc.go
│  │      │  │      
│  │      │  └─per
│  │      │          per.go
│  │      │          
│  │      ├─tpkt
│  │      │      tpkt.go
│  │      │      
│  │      └─x224
│  │              x224.go
│  │              
│  ├─misc
│  │      misc.go
│  │      misc_test.go
│  │      
│  ├─osping
│  │      osping.go
│  │      
│  ├─pool
│  │      go.mod
│  │      pool.go
│  │      pool_test.go
│  │      
│  ├─qqwry
│  │      qqwry.go
│  │      tyoe-ipdb.go
│  │      update.go
│  │      
│  ├─sflag
│  │      sflag.go
│  │      
│  ├─simplehttp
│  │      go.mod
│  │      simplehttp.go
│  │      simplehttp_test.go
│  │      
│  ├─stdio
│  │  │  go.mod
│  │  │  go.sum
│  │  │  stdio.go
│  │  │  
│  │  └─chinese
│  │          chinese.go
│  │          chinese_test.go
│  │          
│  ├─tcpping
│  │      tcpping.go
│  │      
│  └─uri
│          uri.go
│          
├─run
│      run.go
│      
└─static
        fingerprint.txt
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto](https://pkg.go.dev/crypto)
- [ ] [crypto/aes](https://pkg.go.dev/crypto/aes)
- [ ] [crypto/des](https://pkg.go.dev/crypto/des)
- [ ] [crypto/dsa](https://pkg.go.dev/crypto/dsa)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/rand](https://pkg.go.dev/crypto/rand)
- [ ] [crypto/rc4](https://pkg.go.dev/crypto/rc4)
- [ ] [crypto/rsa](https://pkg.go.dev/crypto/rsa)
- [ ] [crypto/sha1](https://pkg.go.dev/crypto/sha1)
- [ ] [database/sql](https://pkg.go.dev/database/sql)
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding](https://pkg.go.dev/encoding)
- [ ] [encoding/asn1](https://pkg.go.dev/encoding/asn1)
- [ ] [encoding/base32](https://pkg.go.dev/encoding/base32)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/csv](https://pkg.go.dev/encoding/csv)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [encoding/pem](https://pkg.go.dev/encoding/pem)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [hash](https://pkg.go.dev/hash)
- [ ] [hash/adler32](https://pkg.go.dev/hash/adler32)
- [ ] [hash/crc32](https://pkg.go.dev/hash/crc32)
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
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode](https://pkg.go.dev/unicode)
- [ ] [unicode/utf16](https://pkg.go.dev/unicode/utf16)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] https://github.com/andybalholm/cascadia
- [ ] https://github.com/atotto/clipboard
- [ ] https://github.com/denisenkom/go-mssqldb
- [ ] https://github.com/golang/snappy
- [ ] https://github.com/golang-sql/civil
- [ ] https://github.com/go-sql-driver/mysql
- [ ] https://github.com/go-stack/stack
- [ ] https://github.com/hashicorp/errwrap
- [ ] https://github.com/hashicorp/go-multierror
- [ ] https://github.com/huin/asn1ber
- [ ] https://github.com/icodeface/tls
- [ ] https://github.com/jlaffaye/ftp
- [ ] https://github.com/klauspost/compress/fse
- [ ] https://github.com/klauspost/compress/huff0
- [ ] https://github.com/klauspost/compress/snappy
- [ ] https://github.com/klauspost/compress/zstd
- [ ] https://github.com/klauspost/compress/zstd/internal/xxhash
- [ ] https://github.com/lcvvvv/appfinger
- [ ] https://github.com/lcvvvv/gonmap
- [ ] https://github.com/lcvvvv/simplehttp
- [ ] https://github.com/lcvvvv/stdio
- [ ] https://github.com/lib/pq
- [ ] https://github.com/lunixbochs/struc
- [ ] https://github.com/miekg/dns
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/PuerkitoBio/goquery
- [ ] https://github.com/sijms/go-ora
- [ ] https://github.com/stacktitan/smb
- [ ] https://github.com/twmb/murmur3
- [ ] https://github.com/xdg-go/pbkdf2
- [ ] https://github.com/xdg-go/scram
- [ ] https://github.com/xdg-go/stringprep
- [ ] https://github.com/youmark/pkcs8
- [ ] https://pkg.go.dev/go.mongodb.org/mongo-driver

## 04-开发设计

本部分尽可能的列举出kscan开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

- 命令行混乱-优化命令行参数输入
- poc支持较少-优化大量poc

## 06-二开计划

- https://github.com/Goqi/Erkscan