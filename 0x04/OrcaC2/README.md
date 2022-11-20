#  [OrcaC2](https://github.com/Ptkatz/OrcaC2)代码分析

本文将从代码层面深入分析OrcaC2项目。OrcaC2是一款基于Websocket加密通信的多功能C&C框架，使用Golang实现。由三部分组成：Orca_Server(服务端)、Orca_Master(控制端)、Orca_Puppet(被控端)。

本文会持续更新，创建于2022年11月7日，最近的一次更新时间为2022年11月7日。

- [01-第三方库]()
- [02-开发设计]()
- [03-代码分析]()
- [04-不足之处]()
- [05-二开计划]()

## 01-第三方库

- [ ] https://github.com/Goqi/ErOrcaC2/blob/main/Orca_Master/go.mod
- [ ] https://github.com/Goqi/ErOrcaC2/blob/main/Orca_Puppet/go.mod
- [ ] https://github.com/Goqi/ErOrcaC2/blob/main/Orca_Server/go.mod
- [ ] https://github.com/gorilla/websocket | Go 的快速、经过良好测试和广泛使用的 WebSocket 实现
- [ ] https://github.com/Binject/debug | pkg/debug 的分支，增加了一些额外的功能
- [ ] https://github.com/desertbit/closer | 一个简单的、线程安全的关闭器
- [ ] https://github.com/desertbit/columnize | golang 的简单列格式输出
- [ ] https://github.com/desertbit/go-shlex | 制作像 Unix shell 这样的词法分析器
- [ ] https://github.com/desertbit/readline | Readline 类库的纯 go(golang) 实现
- [ ] https://github.com/faiface/glhf | 一个让 OpenGL 的生活变得愉快的 Go 包
- [ ] https://github.com/faiface/mainthread | 在主操作系统线程上运行代码
- [ ] https://github.com/go-gl/gl | OpenGL 的 Go 绑定
- [ ] https://github.com/go-gl/glfw | GLFW 3 的 Go 绑定
- [ ] https://github.com/go-gl/mathgl | 一个纯 Go 3D 数学库
- [ ] https://github.com/go-ole/go-ole | golang 的 win32 ole 实现
- [ ] https://github.com/google/uuid | 基于 RFC 4122 和 DCE 1.1 的 UUID 的 Go 包：身份验证和安全服务
- [ ] https://github.com/hashicorp/errwrap | 用于包装和查询错误的 Go (golang) 库
- [ ] https://github.com/hashicorp/go-multierror | 用于包装和查询错误的 Go (golang) 库
- [ ] https://github.com/jinzhu/inflection | 英语名词的复数和单数
- [ ] https://github.com/jinzhu/now | golang的时间工具包
- [ ] https://github.com/jpillora/backoff | Go中的简单退避算法
- [ ] https://github.com/json-iterator/go | 一个高性能 100% 兼容的“encoding/json”替代品
- [ ] https://github.com/lufia/plan9stats | 用于检索计划 9 统计数据的模块
- [ ] https://github.com/lxn/win| Go 编程语言的 Windows API 包装程序包
- [ ] https://github.com/mattn/go-colorable | Windows 的可着色作家
- [ ] https://github.com/mattn/go-isatty | golang 的 isatty
- [ ] https://github.com/mattn/go-runewidth | golang 的 wcwidth
- [ ] https://github.com/mitchellh/colorstring | Go (golang) 库，用于为终端输出着色字符串。
- [ ] https://github.com/modern-go/concurrent | 并发实用程序
- [ ] https://github.com/modern-go/reflect2
- [ ] https://github.com/otiai10/gosseract
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/power-devops/perfstat
- [ ] https://github.com/rivo/uniseg
- [ ] https://github.com/robotn/xgb
- [ ] https://github.com/robotn/xgbutil
- [ ] https://github.com/shiena/ansicolor
- [ ] https://github.com/shirou/gopsutil/v3
- [ ] https://github.com/tklauser/go-sysconf
- [ ] https://github.com/tklauser/numcpus
- [ ] https://github.com/vcaesar/gops
- [ ] https://github.com/vcaesar/imgo
- [ ] https://github.com/vcaesar/keycode
- [ ] https://github.com/vcaesar/tt
- [ ] https://github.com/yusufpapurcu/wmi
- [ ] https://golang.org/x/crypto
- [ ] https://golang.org/x/net
- [ ] https://golang.org/x/sys
- [ ] https://golang.org/x/term
- [ ] https://golang.org/x/text
- [ ] https://github.com/4dogs-cn/TXPortMap
- [ ] https://github.com/Binject/go-donut
- [ ] https://github.com/desertbit/grumble
- [ ] https://github.com/faiface/pixel
- [ ] https://github.com/fatih/color
- [ ] https://github.com/go-vgo/robotgo
- [ ] https://github.com/olekukonko/tablewriter
- [ ] https://github.com/satori/go.uuid
- [ ] https://github.com/schollz/progressbar/v3
- [ ] https://github.com/tj/go-spin
- [ ] https://github.com/togettoyou/wsc
- [ ] https://golang.org/x/image
- [ ] https://gopkg.in/yaml.v2
- [ ] https://gorm.io/gorm
- [ ] https://github.com/C-Sto/BananaPhone
- [ ] https://github.com/C-Sto/goWMIExec
- [ ] https://github.com/Ne0nd0g/go-clr
- [ ] https://github.com/OneOfOne/xxhash
- [ ] https://github.com/PuerkitoBio/goquery
- [ ] https://github.com/axgle/mahonia
- [ ] https://github.com/cheggaaa/pb/v3
- [ ] https://github.com/creack/pty
- [ ] https://github.com/go-sql-driver/mysql
- [ ] https://github.com/golang/protobuf
- [ ] https://github.com/gonutz/ide
- [ ] https://github.com/lucas-clemente/quic-go
- [ ] https://github.com/mattn/go-sqlite3
- [ ] https://github.com/niudaii/crack
- [ ] https://github.com/oschwald/geoip2-golang
- [ ] https://github.com/pkg/sftp
- [ ] https://github.com/robotn/gohook
- [ ] https://github.com/shirou/gopsutil
- [ ] https://github.com/shiyanhui/dht
- [ ] https://github.com/vova616/screenshot
- [ ] https://github.com/xtaci/kcp-go
- [ ] https://github.com/xtaci/smux
- [ ] https://go.uber.org/ratelimit
- [ ] https://go.uber.org/zap
- [ ] https://google.golang.org/protobuf
- [ ] https://github.com/kr/text
- [ ] https://gopkg.in/check.v1
- [ ] https://github.com/BurntSushi/xgb
- [ ] https://github.com/VividCortex/ewma
- [ ] https://github.com/andres-erbsen/clock
- [ ] https://github.com/andybalholm/cascadia
- [ ] https://github.com/awgh/rawreader
- [ ] https://github.com/denisenkom/go-mssqldb
- [ ] https://github.com/fsnotify/fsnotify
- [ ] https://github.com/go-redis/redis
- [ ] https://github.com/go-task/slim-sprig
- [ ] https://github.com/golang-sql/civil
- [ ] https://github.com/golang-sql/sqlexp
- [ ] https://github.com/golang/mock
- [ ] https://github.com/huin/asn1ber
- [ ] https://github.com/icodeface/tls
- [ ] https://github.com/jlaffaye/ftp
- [ ] https://github.com/klauspost/cpuid/v2
- [ ] https://github.com/klauspost/reedsolomon
- [ ] https://github.com/kr/fs
- [ ] https://github.com/lib/pq
- [ ] https://github.com/lunixbochs/struc
- [ ] https://github.com/marten-seemann/qtls-go1-18
- [ ] https://github.com/marten-seemann/qtls-go1-19
- [ ] https://github.com/nxadm/tail
- [ ] https://github.com/onsi/ginkgo
- [ ] https://github.com/oschwald/maxminddb-golang
- [ ] https://github.com/sijms/go-ora/v2
- [ ] https://github.com/soniah/gosnmp
- [ ] https://github.com/stacktitan/smb
- [ ] https://github.com/templexxx/cpufeat
- [ ] https://github.com/templexxx/xor
- [ ] https://github.com/tjfoc/gmsm
- [ ] https://github.com/tomatome/grdp
- [ ] https://github.com/xtaci/lossyconn
- [ ] https://go.uber.org/atomic
- [ ] https://go.uber.org/multierr
- [ ] https://golang.org/x/exp
- [ ] https://golang.org/x/mod
- [ ] https://golang.org/x/tools
- [ ] https://golang.org/x/xerrors
- [ ] https://gopkg.in/mgo.v2
- [ ] https://gopkg.in/tomb.v1
- [ ] https://github.com/go-ini/ini
- [ ] https://github.com/go-playground/locales
- [ ] https://github.com/go-playground/universal-translator
- [ ] https://github.com/lestrrat-go/file-rotatelogs
- [ ] https://github.com/rifflock/lfshook
- [ ] https://github.com/sirupsen/logrus
- [ ] https://gopkg.in/go-playground/validator.v9
- [ ] https://gorm.io/driver/sqlite
- [ ] https://github.com/jonboulle/clockwork
- [ ] https://github.com/kr/pretty
- [ ] https://github.com/leodido/go-urn
- [ ] https://github.com/lestrrat-go/strftime
- [ ] https://gopkg.in/go-playground/assert.v1

## 02-开发设计

本部分尽可能的列举出OrcaC2开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 基本类型
- [ ] 函数方法
- [ ] 并发协程

## 03-代码分析

## 04-不足之处

## 05-二开计划

- https://github.com/Goqi/ErOrcaC2