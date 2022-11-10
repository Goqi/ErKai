# fscan代码分析

本文将从代码层面深入分析fscan项目。fscan是一款内网综合扫描工具，方便一键自动化、全方位的漏洞扫描。

本文创建于2022年11月02日，最近的一次更新时间为2022年11月9日。

- [01-官方包库]()
- [02-第三方库]()
- [03-开发设计]()
- [04-代码分析]()
- [05-不足之处]()
- [06-二开计划]()

## 01-官方包库

- go list -json

- [x] [**embed**](https://pkg.go.dev/embed) | 将静态资源打包至二进制文件中
- [ ] [bufio](https://pkg.go.dev/bufio) | io操作，通过缓冲来提高效率
- [ ] [bytes](https://pkg.go.dev/bytes) | 提供了对字节切片进行读写操作的一系列函数
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
- [ ] [encoding](https://pkg.go.dev/encoding)
- [ ] [encoding/asn1](https://pkg.go.dev/encoding/asn1)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [encoding/pem](https://pkg.go.dev/encoding/pem)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [hash](https://pkg.go.dev/hash)
- [ ] [hash/crc32](https://pkg.go.dev/hash/crc32)
- [ ] [html](https://pkg.go.dev/html)
- [ ] [html/template](https://pkg.go.dev/html/template)
- [ ] [internal/abi](https://pkg.go.dev/internal/abi)
- [ ] [internal/bytealg](https://pkg.go.dev/internal/bytealg)
- [ ] [internal/cpu](https://pkg.go.dev/internal/cpu)
- [ ] [internal/fmtsort](https://pkg.go.dev/internal/fmtsort)
- [ ] [internal/goarch](https://pkg.go.dev/internal/goarch)
- [ ] [internal/godebug](https://pkg.go.dev/internal/godebug)
- [ ] [internal/goexperiment](https://pkg.go.dev/internal/goexperiment)
- [ ] [internal/goos](https://pkg.go.dev/internal/goos)
- [ ] [internal/intern](https://pkg.go.dev/internal/intern)
- [ ] [internal/itoa](https://pkg.go.dev/internal/itoa)
- [ ] [internal/nettrace](https://pkg.go.dev/internal/nettrace)
- [ ] [internal/oserror](https://pkg.go.dev/internal/oserror)
- [ ] [internal/poll](https://pkg.go.dev/internal/poll)
- [ ] [internal/race](https://pkg.go.dev/internal/race)
- [ ] [internal/reflectlite](https://pkg.go.dev/internal/reflectlite)
- [ ] [internal/singleflight](https://pkg.go.dev/internal/singleflight)
- [ ] [internal/syscall/execenv](https://pkg.go.dev/internal/syscall/execenv)
- [ ] [internal/syscall/windows](https://pkg.go.dev/internal/syscall/windows)
- [ ] [internal/syscall/windows/registry](https://pkg.go.dev/internal/syscall/windows/registry)
- [ ] [internal/syscall/windows/sysdll](https://pkg.go.dev/internal/syscall/windows/sysdll)
- [ ] [internal/testlog](https://pkg.go.dev/internal/testlog)
- [ ] [internal/unsafeheader](https://pkg.go.dev/internal/unsafeheader)
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

## 02-第三方库

- https://github.com/Goqi/Erfscan/blob/main/go.mod
- https://github.com/Goqi/Erfscan/blob/main/go.sum

- [ ] https://gopkg.in/yaml.v2 | 对 Go 语言的 YAML 支持
- [ ] https://github.com/antlr/antlr4 | 用于读取和处理结构化文本或二进制文件
- [ ] https://github.com/denisenkom/go-mssqldb |Microsoft SQL驱动程序
- [ ] https://github.com/go-sql-driver/mysql | MySQL驱动程序
- [ ] https://github.com/jlaffaye/ftp | FTP 驱动程序
- [ ] https://github.com/tomatome/grdp | RDP 驱动程序
- [ ] https://github.com/lib/pq | PostgresSQL驱动程序
- [ ] https://github.com/sijms/go-ora | Oracle数据库驱动程序
- [ ] https://github.com/golang-sql/civil | 日期和时间包
- [ ] https://github.com/golang/protobuf | 支持 Google 的协议缓冲区
- [ ] https://github.com/google/cel-go | 快速、便携、非图灵完整的表达式评估
- [ ] https://github.com/lunixbochs/struc | encoding/binary的替代品
- [ ] https://github.com/satori/go.uuid | Go 的 UUID 包
- [ ] https://github.com/stacktitan/smb | Go 中的 SMB 库
- [ ] https://golang.org/x/crypto/blowfish
- [ ] https://golang.org/x/crypto/chacha20
- [ ] https://golang.org/x/crypto/curve25519
- [ ] https://golang.org/x/crypto/ed25519
- [ ] https://golang.org/x/crypto/internal/subtle
- [ ] https://golang.org/x/crypto/md4
- [ ] https://golang.org/x/crypto/poly1305
- [ ] https://golang.org/x/crypto/ssh
- [ ] https://golang.org/x/net/bpf
- [ ] https://golang.org/x/net/http/httpguts
- [ ] https://golang.org/x/net/http2
- [ ] https://golang.org/x/net/icmp
- [ ] https://golang.org/x/net/idna
- [ ] https://golang.org/x/net/internal/iana
- [ ] https://golang.org/x/net/internal/socket
- [ ] https://golang.org/x/net/internal/socks
- [ ] https://golang.org/x/net/internal/timeseries
- [ ] https://golang.org/x/net/ipv4
- [ ] https://golang.org/x/net/ipv6
- [ ] https://golang.org/x/net/proxy
- [ ] https://golang.org/x/net/trace
- [ ] https://golang.org/x/sys/internal/unsafeheader
- [ ] https://golang.org/x/sys/windows
- [ ] https://golang.org/x/text/encoding
- [ ] https://golang.org/x/text/secure/bidirule
- [ ] https://golang.org/x/text/transform
- [ ] https://golang.org/x/text/unicode/bidi
- [ ] https://golang.org/x/text/unicode/norm
- [ ] https://golang.org/x/text/width
- [ ] https://google.golang.org/genproto/googleapis/api/annotations
- [ ] https://google.golang.org/genproto/googleapis/api/expr/v1alpha1
- [ ] https://google.golang.org/genproto/googleapis/rpc/status
- [ ] https://google.golang.org/grpc
- [ ] https://github.com/huin/asn1ber
- [ ] https://github.com/icodeface/tls

## 03-开发设计

本部分尽可能的列举出fscan开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型
- [ ] 函数方法
- [ ] 并发协程

## 04-代码分析

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Erfscan