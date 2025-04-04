# [fscan](https://github.com/shadow1ng/fscan)代码分析

本文将从代码层面深入分析fscan项目。fscan是shadow1ng开发的一款内网综合扫描工具，方便一键自动化、全方位的漏洞扫描工具。支持主机存活探测、端口扫描、常见服务的爆破、ms17010、redis批量写公钥、计划任务反弹shell、读取win网卡信息、web指纹识别、web漏洞扫描、netbios探测、域控识别等功能。

本文创建于2022年11月2日，最近的一次更新时间为2022年11月11日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
│  main.go  //程序入口主函数
│
├─common
│      config.go
│      flag.go
│      log.go
│      Parse.go
│      ParseIP.go
│      ParsePort.go
│      proxy.go
│      
├─image
│      
├─Plugins
│      base.go
│      CVE-2020-0796.go
│      fcgiscan.go
│      findnet.go
│      ftp.go
│      icmp.go
│      memcached.go
│      mongodb.go
│      ms17010-exp.go
│      ms17010.go
│      mssql.go
│      mysql.go
│      NetBIOS.go
│      oracle.go
│      portscan.go
│      postgres.go
│      rdp.go
│      redis.go
│      scanner.go
│      smb.go
│      ssh.go
│      webtitle.go
│      
└─WebScan
    │  InfoScan.go
    │  WebScan.go
    │  
    ├─info
    │      rules.go
    │      
    ├─lib
    │      check.go
    │      client.go
    │      eval.go
    │      http.pb.go
    │      http.proto
    │      shiro.go
    │      
    └─pocs  // yml pocs格式
            74cms-sqli-1.yml
            74cms-sqli-2.yml
```

## 02-官方包库

- [x] [**embed**](https://pkg.go.dev/embed) | 将静态资源打包至二进制文件中
- [ ] [**context**](https://pkg.go.dev/context) | 上下文-设置截止日期/同步信号/传递请求相关值的结构体
- [ ] [runtime](https://pkg.go.dev/runtime) | 调度器
- [ ] [bufio](https://pkg.go.dev/bufio) | io操作，通过缓冲来提高效率
- [ ] [bytes](https://pkg.go.dev/bytes) | 提供了对字节切片进行读写操作的一系列函数
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip) | gzip 格式压缩文件的读写
- [ ] [crypto](https://pkg.go.dev/crypto) | 密码加减密
- [ ] [crypto/aes](https://pkg.go.dev/crypto/aes) | aes加减密
- [ ] [crypto/cipher](https://pkg.go.dev/crypto/cipher) | 实现了标准的分组密码
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5) | md5加减密
- [ ] [crypto/rand](https://pkg.go.dev/crypto/rand) | 强随机数生成器
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls) | HTTPS
- [ ] [database/sql](https://pkg.go.dev/database/sql) | SQL数据库的通用接口
- [ ] [encoding](https://pkg.go.dev/encoding) | 编码
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64) | Base编码解码
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary) | 读写二进制文件
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex) | 十六进制
- [ ] [errors](https://pkg.go.dev/errors) | 错误处理
- [ ] [flag](https://pkg.go.dev/flag) | 命令行标志解析
- [ ] [fmt](https://pkg.go.dev/fmt) | 格式化输出
- [ ] [io](https://pkg.go.dev/io) | 数据读写
- [ ] [io/fs](https://pkg.go.dev/io/fs) | 文件系统的基本接口
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil) | 文件读写实用程序功能
- [ ] [log](https://pkg.go.dev/log) | 日志
- [ ] [math/rand](https://pkg.go.dev/math/rand) | 伪随机数生成器
- [ ] [net](https://pkg.go.dev/net) | 网络
- [ ] [net/http](https://pkg.go.dev/net/http) | HTTP
- [ ] [net/url](https://pkg.go.dev/net/url) | 解析URL
- [ ] [os](https://pkg.go.dev/os) | 操作系统相关库
- [ ] [os/exec](https://pkg.go.dev/os/exec) | 执行命令
- [ ] [path/filepath](https://pkg.go.dev/path/filepath) | 文件路径
- [ ] [reflect](https://pkg.go.dev/reflect)| 反射
- [ ] [regexp](https://pkg.go.dev/regexp) | 正则表达式
- [ ] [sort](https://pkg.go.dev/sort) | 排序
- [ ] [strconv](https://pkg.go.dev/strconv) | 类型转换
- [ ] [strings](https://pkg.go.dev/strings) | 字符串操作
- [ ] [sync](https://pkg.go.dev/sync) | 同步
- [ ] [sync/atomic](https://pkg.go.dev/sync/atomic) | 原子操作
- [ ] [syscall](https://pkg.go.dev/syscall) | 操作系统接口
- [ ] [time](https://pkg.go.dev/time) | 时间日期
- [ ] [unicode](https://pkg.go.dev/unicode) | Unicode操作
- [ ] [unsafe](https://pkg.go.dev/unsafe) | 不安全类型操作

## 03-第三方库

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
- [ ] https://golang.org/x/net/proxy | 支持多种协议来代理网络数据

## 04-开发设计

本部分尽可能的列举出fscan开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

```
var Userdict = map[string][]string{}
var Passwords = []string{}
var PORTList = map[string]int{}
type HostInfo struct {}
type PocInfo struct {}
var (Path string)
var PluginList = map[string]interface{}{}
```

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

- 命令行混乱-优化命令行参数输入
- poc支持较少-优化大量poc

## 06-二开计划

- https://github.com/Goqi/Erfscan