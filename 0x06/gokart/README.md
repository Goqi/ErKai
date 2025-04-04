# [gokart](https://github.com/praetorian-inc/gokart)代码分析

本文将从代码层面深入分析gokart项目。gokart是praetorian-inc开发的Go语言代码安全扫描工具。使用 Go 源代码的 SSA（单一静态分配）形式发现漏洞。能够跟踪变量和函数参数的来源以确定输入源是否安全，与其他 Go 安全扫描程序相比，这减少了误报的数量。

本文创建于2022年11月14日，最近的一次更新时间为2022年11月14日。

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
├─analyzers
│      cmdi.go
│      cmdi_test.go
│      generic.go
│      rsa.go
│      rsa_test.go
│      scan.go
│      sqli.go
│      sqli_test.go
│      ssrf.go
│      ssrf_test.go
│      traversal.go
│      traversal_test.go
│      
├─cmd
│      root.go
│      scan.go
│      scan_test.go
│      version.go
│      
├─docs
│  │  EXTENDING_ANALYZERS.md
│  │  
│  └─img
│          logo.png
│          
├─run
│      run.go
│      run_test.go
│      
├─test
│  ├─testdata
│  │  └─vulnerablemodule
│  │      │  go.mod
│  │      │  
│  │      ├─combined
│  │      │      serverTest.go
│  │      │      
│  │      ├─command_injection
│  │      │      commandContext.go
│  │      │      command_context_injection_safe.go
│  │      │      command_injection.go
│  │      │      
│  │      ├─old_tests
│  │      │      gosecFalsePositive2.go
│  │      │      rsaBin.go
│  │      │      rsaBinPhi.go
│  │      │      
│  │      ├─path_traversal
│  │      │      gosecFalsePositive1.go
│  │      │      path1.go
│  │      │      path2.go
│  │      │      path3.go
│  │      │      path4.go
│  │      │      pathBin.go
│  │      │      pathBinPhi.go
│  │      │      pathMultParams.go
│  │      │      pathPhi.go
│  │      │      pathReg.go
│  │      │      twitterTest.go
│  │      │      unop_test.go
│  │      │      
│  │      ├─rsa_keylength
│  │      │      rsa_direct_bad.go
│  │      │      rsa_direct_good.go
│  │      │      rsa_indirect_bad.go
│  │      │      rsa_indirect_good.go
│  │      │      rsa_multiple_issues.go
│  │      │      rsa_multiple_params_bad.go
│  │      │      rsa_multiple_params_good.go
│  │      │      rsa_phi_bad.go
│  │      │      rsa_phi_good.go
│  │      │      
│  │      ├─sql_injection
│  │      │      sql1.go
│  │      │      sql2.go
│  │      │      sql3.go
│  │      │      sql4_formatstr.go
│  │      │      SQLfp.go
│  │      │      
│  │      └─ssrf
│  │              ssrf.go
│  │              ssrf_alloc_taint.go
│  │              ssrf_client.go
│  │              ssrf_client_control.go
│  │              ssrf_client_good.go
│  │              ssrf_good.go
│  │              ssrf_struct.go
│  │              
│  └─testutil
│          run.go
│          
└─util
        analyzers.yml
        config.go
        finding.go
        module.go
        sarif.go
        taint.go
        util.go
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/bzip2](https://pkg.go.dev/compress/bzip2)
- [ ] [compress/flate](https://pkg.go.dev/compress/flate)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
- [ ] [compress/zlib](https://pkg.go.dev/compress/zlib)
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
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding](https://pkg.go.dev/encoding)
- [ ] [encoding/asn1](https://pkg.go.dev/encoding/asn1)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/csv](https://pkg.go.dev/encoding/csv)
- [ ] [encoding/gob](https://pkg.go.dev/encoding/gob)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [encoding/pem](https://pkg.go.dev/encoding/pem)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [hash](https://pkg.go.dev/hash)
- [ ] [hash/adler32](https://pkg.go.dev/hash/adler32)
- [ ] [hash/crc32](https://pkg.go.dev/hash/crc32)
- [ ] [hash/crc64](https://pkg.go.dev/hash/crc64)
- [ ] [html](https://pkg.go.dev/html)
- [ ] [image](https://pkg.go.dev/image)
- [ ] [image/color](https://pkg.go.dev/image/color)
- [ ] [image/internal/imageutil](https://pkg.go.dev/image/internal/imageutil)
- [ ] [image/jpeg](https://pkg.go.dev/image/jpeg)
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
- [ ] [runtime/internal/atomic](https://pkg.go.dev/runtime/internal/atomic)
- [ ] [runtime/internal/math](https://pkg.go.dev/runtime/internal/math)
- [ ] [runtime/internal/sys](https://pkg.go.dev/runtime/internal/sys)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [sync/atomic](https://pkg.go.dev/sync/atomic)
- [ ] [syscall](https://pkg.go.dev/syscall)
- [ ] [text/scanner](https://pkg.go.dev/text/scanner)
- [ ] [text/template](https://pkg.go.dev/text/template)
- [ ] [text/template/parse](https://pkg.go.dev/text/template/parse)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode](https://pkg.go.dev/unicode)
- [ ] [unicode/utf16](https://pkg.go.dev/unicode/utf16)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] https://github.com/Microsoft/go-winio
- [ ] https://github.com/ProtonMail/go-crypto
- [ ] https://github.com/emirpasic/gods
- [ ] https://github.com/fatih/color
- [ ] https://github.com/go-git/gcfg
- [ ] https://github.com/go-git/go-billy
- [ ] https://github.com/go-git/go-git
- [ ] https://github.com/imdario/mergo
- [ ] https://github.com/inconshreveable/mousetrap
- [ ] https://github.com/jbenet/go-context/io
- [ ] https://github.com/kevinburke/ssh_config
- [ ] https://github.com/lithammer/dedent | 从多行字符串中删除任何常见的前导空格
- [ ] https://github.com/mattn/go-colorable
- [ ] https://github.com/mattn/go-isatty
- [ ] https://github.com/mitchellh/go-homedir
- [ ] https://github.com/owenrumney/go-sarif/sarif
- [ ] https://github.com/segmentio/fasthash/fnv1a
- [ ] https://github.com/sergi/go-diff/diffmatchpatch
- [ ] https://github.com/spf13/cobra
- [ ] https://github.com/spf13/pflag
- [ ] https://github.com/xanzy/ssh-agent
- [ ] https://github.com/zclconf/go-cty
- [ ] https://gopkg.in/warnings.v0
- [ ] https://gopkg.in/yaml.v3

## 04-开发设计

本部分尽可能的列举出gokart开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

- 使用效果不佳，存在很多严重的问题。

## 06-二开计划

- https://github.com/Goqi/Ergokart