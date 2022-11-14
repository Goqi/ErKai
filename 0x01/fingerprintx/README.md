# [fingerprintx](https://github.com/praetorian-inc/fingerprintx)代码分析

本文将从代码层面深入分析fingerprintx项目。fingerprintx是praetorian-inc项目开发的一款指纹识别工具，支持 RDP、SSH、MySQL、PostgreSQL、Kafka 等指纹识别服务。可以与[Naabu](https://github.com/projectdiscovery/naabu)等端口扫描仪一起使用，对端口扫描期间识别的一组端口进行指纹识别。

本文创建于2022年11月14日，最近的一次更新时间为2022年11月14日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
├─cmd
│  └─fingerprintx
│          main.go
│          
├─docs
│      plugin.md
│      usage_library.md
│      usage_scan.md
│      usage_target.md
│      
├─pkg
│  ├─plugins
│  │  │  plugins.go
│  │  │  types.go
│  │  │  
│  │  ├─pluginutils
│  │  │      error.go
│  │  │      requests.go
│  │  │      
│  │  └─services
│  │      ├─dhcp
│  │      │      dhcp.go
│  │      │      dhcpd.conf
│  │      │      dhcp_test.go
│  │      │      
│  │      ├─dns
│  │      │      dns.go
│  │      │      dns_test.go
│  │      │      
│  │      ├─ftp
│  │      │      ftp.go
│  │      │      ftp_test.go
│  │      │      
│  │      ├─http
│  │      │      http.go
│  │      │      https_test.go
│  │      │      http_test.go
│  │      │      
│  │      ├─imap
│  │      │      imap.go
│  │      │      imap_test.go
│  │      │      
│  │      ├─ipsec
│  │      │      ipsec.go
│  │      │      ipsec_test.go
│  │      │      
│  │      ├─kafka
│  │      │  ├─kafkaNew
│  │      │  │      kafkaNew.go
│  │      │  │      kafkaNew_test.go
│  │      │  │      
│  │      │  └─kafkaOld
│  │      │          kafkaOld.go
│  │      │          kafkaOld_test.go
│  │      │          
│  │      ├─ldap
│  │      │      ldap.go
│  │      │      ldap_test.go
│  │      │      
│  │      ├─linuxrpc
│  │      │      linuxrpc.go
│  │      │      linuxrpc_test.go
│  │      │      
│  │      ├─modbus
│  │      │      modbus.go
│  │      │      modbus_test.go
│  │      │      
│  │      ├─mqtt
│  │      │  ├─mqtt3
│  │      │  │      mqtt3.go
│  │      │  │      mqtt3_test.go
│  │      │  │      
│  │      │  └─mqtt5
│  │      │          mqtt5.go
│  │      │          mqtt5_test.go
│  │      │          
│  │      ├─mssql
│  │      │      mssql.go
│  │      │      mssql_test.go
│  │      │      
│  │      ├─mysql
│  │      │      mysql.go
│  │      │      mysql_test.go
│  │      │      
│  │      ├─netbios
│  │      │      netbios_ns.go
│  │      │      netbios_ns_test.go
│  │      │      
│  │      ├─ntp
│  │      │      ntp.go
│  │      │      ntp_test.go
│  │      │      
│  │      ├─openvpn
│  │      │      openvpn.go
│  │      │      openvpn_test.go
│  │      │      
│  │      ├─oracledb
│  │      │      oracle.go
│  │      │      oracle_test.go
│  │      │      
│  │      ├─pop3
│  │      │      pop3.go
│  │      │      pop3_test.go
│  │      │      
│  │      ├─postgresql
│  │      │      postgresql.go
│  │      │      postgresql_test.go
│  │      │      
│  │      ├─rdp
│  │      │      rdp.go
│  │      │      rdp_test.go
│  │      │      
│  │      ├─redis
│  │      │      redis.go
│  │      │      redis_test.go
│  │      │      
│  │      ├─rsync
│  │      │      rsync.go
│  │      │      rsync_test.go
│  │      │      
│  │      ├─rtsp
│  │      │      rtsp.go
│  │      │      rtsp_test.go
│  │      │      
│  │      ├─smb
│  │      │      smb.go
│  │      │      smb_test.go
│  │      │      
│  │      ├─smtp
│  │      │      smtp.go
│  │      │      smtp_test.go
│  │      │      
│  │      ├─snmp
│  │      │      snmp.go
│  │      │      snmp_test.go
│  │      │      
│  │      ├─ssh
│  │      │      ssh.go
│  │      │      ssh_test.go
│  │      │      
│  │      ├─stun
│  │      │      stun.go
│  │      │      stun_test.go
│  │      │      
│  │      ├─telnet
│  │      │      telnet.go
│  │      │      telnet_test.go
│  │      │      
│  │      └─vnc
│  │              vnc.go
│  │              vnc_test.go
│  │              
│  ├─runner
│  │      report.go
│  │      root.go
│  │      target.go
│  │      types.go
│  │      utils.go
│  │      
│  ├─scan
│  │      config.go
│  │      plugin_list.go
│  │      scan_api.go
│  │      simple_scan.go
│  │      types.go
│  │      
│  └─test
│          testutil.go
│          
├─static
│      fingerprintx-chain.png
│      fingerprintx-logo.png
│      fingerprintx-smb.png
│      fingerprintx-usage.png
│      
└─third_party
    └─cryptolib
        │  .gitattributes
        │  .gitignore
        │  codereview.cfg
        │  CONTRIBUTING.md
        │  LICENSE
        │  PATENTS
        │  README.md
        │  
        └─ssh
            │  benchmark_test.go
            │  buffer.go
            │  buffer_test.go
            │  certs.go
            │  certs_test.go
            │  channel.go
            │  cipher.go
            │  cipher_test.go
            │  client.go
            │  client_auth.go
            │  client_auth_test.go
            │  client_test.go
            │  common.go
            │  common_test.go
            │  connection.go
            │  doc.go
            │  example_test.go
            │  export.go
            │  handshake.go
            │  handshake_test.go
            │  kex.go
            │  kex_test.go
            │  keys.go
            │  keys_test.go
            │  mac.go
            │  mempipe_test.go
            │  messages.go
            │  messages_test.go
            │  mux.go
            │  mux_test.go
            │  server.go
            │  session.go
            │  session_test.go
            │  ssh_gss.go
            │  ssh_gss_test.go
            │  streamlocal.go
            │  tcpip.go
            │  tcpip_test.go
            │  testdata_test.go
            │  transport.go
            │  transport_test.go
            │  
            ├─agent
            │      client.go
            │      client_test.go
            │      example_test.go
            │      forward.go
            │      keyring.go
            │      keyring_test.go
            │      server.go
            │      server_test.go
            │      testdata_test.go
            │      
            ├─internal
            │  └─bcrypt_pbkdf
            │          bcrypt_pbkdf.go
            │          bcrypt_pbkdf_test.go
            │          
            ├─knownhosts
            │      knownhosts.go
            │      knownhosts_test.go
            │      
            ├─terminal
            │      terminal.go
            │      
            ├─test
            │      agent_unix_test.go
            │      banner_test.go
            │      cert_test.go
            │      dial_unix_test.go
            │      doc.go
            │      forward_unix_test.go
            │      multi_auth_test.go
            │      session_test.go
            │      sshd_test_pw.c
            │      testdata_test.go
            │      test_unix_test.go
            │      
            └─testdata
                    doc.go
                    keys.go
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
- [ ] [os/user](https://pkg.go.dev/os/user)
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [regexp/syntax](https://pkg.go.dev/regexp/syntax)
- [ ] [runtime](https://pkg.go.dev/runtime)
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

- [ ] https://github.com/inconshreveable/mousetrap
- [ ] https://github.com/spf13/cobra
- [ ] https://github.com/spf13/pflag

## 04-开发设计

本部分尽可能的列举出fingerprintx开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Erfingerprintx
- https://x.hacking8.com/post-446.html