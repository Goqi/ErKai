# [zgrab2](https://github.com/zmap/zgrab2)代码分析

本文将从代码层面深入分析理解zgrab2项目。zgrab2是zmap开发的一款快速、模块化的应用层网络扫描仪。专为完成大型 Internet 范围的调查而设计。ZGrab 是为与 ZMap 一起工作而构建的（ZMap 识别 L4 响应主机，ZGrab 执行深入的后续 L7 握手）。与许多其他网络扫描仪不同，ZGrab 输出网络握手的详细记录（例如，在 TLS 握手中交换的所有消息）以供离线分析。

本文会持续更新，创建于2021年3月16日，最近的一次更新时间为2022年11月14日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  config.go
│  conn.go
│  conn_bytelimit_test.go
│  conn_timeout_test.go
│  errors.go
│  fake_resolver.go
│  input.go
│  input_test.go
│  module.go
│  module_set.go
│  monitor.go
│  multiple.go
│  output.go
│  output_test.go
│  processing.go
│  scanner.go
│  status.go
│  tls.go
│  utility.go
│      
├─bin
│      bin.go
│      default_modules.go
│      doc.go
│      summary.go
│      
├─cmd
│  └─zgrab2
│          main.go
│          mult.ini
│              
├─lib
│  ├─http
│  │  │  chunked.go
│  │  │  chunked_test.go
│  │  │  client.go
│  │  │  clientserver_test.go
│  │  │  client_test.go
│  │  │  cookie.go
│  │  │  cookie_test.go
│  │  │  doc.go
│  │  │  example_test.go
│  │  │  export_test.go
│  │  │  filetransport.go
│  │  │  filetransport_test.go
│  │  │  fs.go
│  │  │  fs_test.go
│  │  │  h2_bundle.go
│  │  │  header.go
│  │  │  header_test.go
│  │  │  http.go
│  │  │  jar.go
│  │  │  lex.go
│  │  │  lex_test.go
│  │  │  main_test.go
│  │  │  method.go
│  │  │  npn_test.go
│  │  │  protocol.go
│  │  │  proxy_test.go
│  │  │  race.go
│  │  │  range_test.go
│  │  │  readrequest_test.go
│  │  │  request.go
│  │  │  requestwrite_test.go
│  │  │  request_test.go
│  │  │  response.go
│  │  │  responsewrite_test.go
│  │  │  response_test.go
│  │  │  server.go
│  │  │  serve_test.go
│  │  │  sniff.go
│  │  │  sniff_test.go
│  │  │  status.go
│  │  │  testcert.go
│  │  │  transfer.go
│  │  │  transfer_test.go
│  │  │  transport.go
│  │  │  transport_internal_test.go
│  │  │  transport_test.go
│  │  │  triv.go
│  │  │  
│  │  ├─cookiejar
│  │  │      dummy_publicsuffix_test.go
│  │  │      example_test.go
│  │  │      jar.go
│  │  │      jar_test.go
│  │  │      punycode.go
│  │  │      punycode_test.go
│  │  │      
│  │  ├─httptest
│  │  │      example_test.go
│  │  │      httptest.go
│  │  │      httptest_test.go
│  │  │      recorder.go
│  │  │      recorder_test.go
│  │  │      server.go
│  │  │      server_test.go
│  │  │      
│  │  ├─httptrace
│  │  │      example_test.go
│  │  │      trace.go
│  │  │      trace_test.go
│  │  │      
│  │  ├─httputil
│  │  │      dump.go
│  │  │      dump_test.go
│  │  │      example_test.go
│  │  │      non
│  │  │      non.go
│  │  │      non1.go
│  │  │      persist.go
│  │  │      
│  │  ├─nettrace
│  │  │      nettrace.go
│  │  │      
│  │  └─testdata
│  │          file
│  │          index.html
│  │          style.css
│  │          
│  ├─mysql
│  │      errors.go
│  │      mysql.go
│  │      
│  ├─output
│  │  │  process.go
│  │  │  
│  │  └─test
│  │          process_test.go
│  │          
│  ├─smb
│  │  │  .gitignore
│  │  │  about.txt
│  │  │  LICENSE
│  │  │  README.md
│  │  │  
│  │  ├─gss
│  │  │      gss.go
│  │  │      oid.go
│  │  │      
│  │  ├─ntlmssp
│  │  │      crypto.go
│  │  │      crypto_test.go
│  │  │      ntlmssp.go
│  │  │      ntlmssp_test.go
│  │  │      
│  │  └─smb
│  │      │  session.go
│  │      │  smb.go
│  │      │  zgrab.go
│  │      │  
│  │      └─encoder
│  │              encoder.go
│  │              encoder_test.go
│  │              unicode.go
│  │              
│  └─ssh
│      │  benchmark_test.go
│      │  buffer.go
│      │  buffer_test.go
│      │  certs.go
│      │  certs_test.go
│      │  channel.go
│      │  cipher.go
│      │  cipher_test.go
│      │  client.go
│      │  client_auth.go
│      │  client_auth_test.go
│      │  client_test.go
│      │  common.go
│      │  config.go
│      │  connection.go
│      │  doc.go
│      │  example_test.go
│      │  handshake.go
│      │  handshake_test.go
│      │  kex.go
│      │  kex_gex.go
│      │  kex_test.go
│      │  keys.go
│      │  keys_test.go
│      │  log.go
│      │  mac.go
│      │  mempipe_test.go
│      │  messages.go
│      │  messages_test.go
│      │  mux.go
│      │  mux_test.go
│      │  server.go
│      │  session.go
│      │  session_test.go
│      │  tcpip.go
│      │  tcpip_test.go
│      │  testdata_test.go
│      │  transport.go
│      │  transport_test.go
│      │  
│      ├─agent
│      │      client.go
│      │      client_test.go
│      │      example_test.go
│      │      forward.go
│      │      keyring.go
│      │      keyring_test.go
│      │      server.go
│      │      server_test.go
│      │      testdata_test.go
│      │      
│      ├─terminal
│      │      terminal.go
│      │      terminal_test.go
│      │      util.go
│      │      util_bsd.go
│      │      util_linux.go
│      │      util_plan9.go
│      │      util_solaris.go
│      │      util_windows.go
│      │      
│      ├─test
│      │      agent_unix_test.go
│      │      cert_test.go
│      │      doc.go
│      │      forward_unix_test.go
│      │      session_test.go
│      │      tcpip_test.go
│      │      testdata_test.go
│      │      test_unix_test.go
│      │      
│      └─testdata
│              doc.go
│              keys.go
│              
├─modules
│  │  bacnet.go
│  │  banner.go
│  │  dnp3.go
│  │  fox.go
│  │  ftp.go
│  │  http.go
│  │  imap.go
│  │  ipp.go
│  │  modbus.go
│  │  mongodb.go
│  │  mssql.go
│  │  mysql.go
│  │  ntp.go
│  │  oracle.go
│  │  pop3.go
│  │  postgres.go
│  │  redis.go
│  │  siemens.go
│  │  smb.go
│  │  smtp.go
│  │  ssh.go
│  │  telnet.go
│  │  tls.go
│  │  
│  ├─bacnet
│  │      common.go
│  │      log.go
│  │      messages.go
│  │      messages_test.go
│  │      objects.go
│  │      objects_test.go
│  │      query.go
│  │      scanner.go
│  │      
│  ├─banner
│  │      scanner.go
│  │      
│  ├─dnp3
│  │      crc16.go
│  │      dnp3.go
│  │      log.go
│  │      scanner.go
│  │      
│  ├─fox
│  │      fox.go
│  │      log.go
│  │      scanner.go
│  │      
│  ├─ftp
│  │      scanner.go
│  │      
│  ├─http
│  │      http_readlimit_test.go
│  │      scanner.go
│  │      
│  ├─imap
│  │      imap.go
│  │      scanner.go
│  │      
│  ├─ipp
│  │      ipp.go
│  │      ipp_test.go
│  │      scanner.go
│  │      scanner_test.go
│  │      
│  ├─modbus
│  │      modbus.go
│  │      scanner.go
│  │      
│  ├─mongodb
│  │      scanner.go
│  │      types.go
│  │      
│  ├─mssql
│  │      connection.go
│  │      scanner.go
│  │      
│  ├─mysql
│  │      scanner.go
│  │      
│  ├─ntp
│  │      scanner.go
│  │      
│  ├─oracle
│  │      connection.go
│  │      scanner.go
│  │      types.go
│  │      types_test.go
│  │      
│  ├─pop3
│  │      pop3.go
│  │      scanner.go
│  │      
│  ├─postgres
│  │      connection.go
│  │      scanner.go
│  │      
│  ├─redis
│  │      scanner.go
│  │      types.go
│  │      types_test.go
│  │      
│  ├─siemens
│  │      common.go
│  │      log.go
│  │      messages.go
│  │      s7.go
│  │      scanner.go
│  │      
│  ├─smb
│  │      scanner.go
│  │      
│  ├─smtp
│  │      scanner.go
│  │      smtp.go
│  │      
│  └─telnet
│          log.go
│          names.go
│          scanner.go
│          telnet.go
│          
├─tools
│  └─keys
│      │  dhe.go
│      │  dhe_test.go
│      │  ecdhe.go
│      │  ecdhe_test.go
│      │  names.go
│      │  rsa.go
│      │  rsa_test.go
│      │  
│      └─testdata
│              test1024dh.pem
│              test4096.pem
│              
└─zgrab2_schemas
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
- [ ] [expvar](https://pkg.go.dev/expvar)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [hash](https://pkg.go.dev/hash)
- [ ] [hash/crc32](https://pkg.go.dev/hash/crc32)
- [ ] [input_test.go](https://pkg.go.dev/input_test.go)
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
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/netip](https://pkg.go.dev/net/netip)
- [ ] [net/textproto](https://pkg.go.dev/net/textproto)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [output_test.go"](https://pkg.go.dev/output_test.go")
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
- [ ] [testing](https://pkg.go.dev/testing)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [time"](https://pkg.go.dev/time")
- [ ] [unicode](https://pkg.go.dev/unicode)
- [ ] [unicode/utf16](https://pkg.go.dev/unicode/utf16)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] https://github.com/beorn7/perks
- [ ] https://github.com/golang/protobuf | protobuf 库
- [ ] https://github.com/konsorten/go-windows-terminal-sequences | 启用对 Windows 终端颜色的支持
- [ ] https://github.com/matttproud/golang_protobuf_extensions | 流式传输 Protocol Buffer 消息
- [ ] https://github.com/prometheus/client_golang
- [ ] https://github.com/prometheus/client_model/go
- [ ] https://github.com/prometheus/common/expfmt
- [ ] https://github.com/prometheus/common | 在 Prometheus 组件和库之间共享的 Go 库
- [ ] https://github.com/prometheus/common/model
- [ ] https://github.com/sirupsen/logrus
- [ ] https://github.com/weppos/publicsuffix-go/publicsuffix
- [ ] https://github.com/zmap/rc2
- [ ] https://github.com/zmap/zcrypto
- [ ] https://github.com/zmap/zflags
- [ ] https://golang.org/x/net/dns/dnsmessage


## 04-开发设计

本部分尽可能的列举出ffuf开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Erzgrab2