# [cube](https://github.com/JKme/cube)代码分析

本文将从代码层面深入分析cube项目。cube是JKme开发的一款内网渗透测试工具，弱密码爆破、信息收集和漏洞扫描。方便二次开发，快速增加插件；支持输出结果到excel文档；精简运行参数，方便记忆。

本文创建于2022年11月18日，最近的一次更新时间为2022年11月18日。

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
├─cli
│  └─cmd
│          crack.go
│          probe.go
│          root.go
│          sqlcmd.go
│          version.go
│          
├─config
│      config.go
│      
├─core
│  │  options.go
│  │  report.go
│  │  
│  ├─crackmodule
│  │      check_port.go
│  │      crack_interface.go
│  │      crack_option.go
│  │      crack_report.go
│  │      crack_task.go
│  │      elastic.go
│  │      ftp.go
│  │      httpbasic.go
│  │      interfaces_test.go
│  │      jenkins.go
│  │      mongo.go
│  │      mssql.go
│  │      mysql.go
│  │      oracle.go
│  │      phpmyadmin.go
│  │      postgres.go
│  │      redis.go
│  │      smb.go
│  │      ssh.go
│  │      zabbix.go
│  │      
│  ├─probemodule
│  │      check_port.go
│  │      docker.go
│  │      dubbo.go
│  │      jboss.go
│  │      k8s10250.go
│  │      k8s6443.go
│  │      k8setcd.go
│  │      ms17010.go
│  │      mssql.go
│  │      netbios.go
│  │      oxid.go
│  │      ping.go
│  │      probe_interface.go
│  │      probe_option.go
│  │      probe_task.go
│  │      probe_test.go
│  │      prometheus.go
│  │      rmi.go
│  │      smb.go
│  │      smbghost.go
│  │      winrm.go
│  │      wmi.go
│  │      zookeeper.go
│  │      
│  └─sqlcmdmodule
│          mssql1.go
│          mssql2.go
│          mssql3.go
│          mssql4.go
│          mysql.go
│          sqlcmd_interface.go
│          sqlcmd_option.go
│          sqlcmd_task.go
│          ssh.go
│          
├─gologger
│      gologger.go
│      
├─image
│      crack.gif
│      report.png
│      sqlcmd.png
│      
├─pkg
│      str_util.go
│      
└─report
        cells.go
        csv_test.go
        excel.go
        kv.go
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes) | 操作byte切片
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [database/sql/driver](https://pkg.go.dev/database/sql/driver)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [math/rand](https://pkg.go.dev/math/rand)
- [ ] [net](https://pkg.go.dev/net)
- [ ] [net/http](https://pkg.go.dev/net/http)
- [ ] [net/textproto](https://pkg.go.dev/net/textproto)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/exec](https://pkg.go.dev/os/exec)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [runtime](https://pkg.go.dev/runtime)
- [ ] [sort](https://pkg.go.dev/sort)
- [ ] [strconv](https://pkg.go.dev/strconv)
- [ ] [strings](https://pkg.go.dev/strings)
- [ ] [sync](https://pkg.go.dev/sync)
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode](https://pkg.go.dev/unicode)
- [ ] [unicode/utf16](https://pkg.go.dev/unicode/utf16)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] https://github.com/spf13/cobra
- [ ] https://github.com/golang-sql/civil
- [ ] https://github.com/golang-sql/sqlexp
- [ ] https://gopkg.in/mgo.v2
- [ ] https://github.com/lib/pq
- [ ] https://github.com/jlaffaye/ftp
- [ ] https://github.com/go-sql-driver/mysql
- [ ] https://github.com/denisenkom/go-mssqldb
- [ ] https://github.com/sijms/go-ora
- [ ] https://github.com/stacktitan/smb
- [ ] https://github.com/hashicorp/errwrap
- [ ] https://github.com/hashicorp/go-multierror
- [ ] https://github.com/inconshreveable/mousetrap
- [ ] https://github.com/JKme/gomanuf | 通过mac地址计算设备厂家信息
- [ ] https://github.com/JKme/go-ntlmssp
- [ ] https://github.com/malfunkt/iprange | nmap 格式的 IPv4 地址解析器
- [ ] https://github.com/mattn/go-runewidth
- [ ] https://github.com/mohae/deepcopy
- [ ] https://github.com/olekukonko/tablewriter | 带有颜色的格式化单元表格打印输出
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/richardlehane/mscfb
- [ ] https://github.com/richardlehane/msoleps
- [ ] https://github.com/xuri/excelize | Excel操作

## 04-开发设计

本部分尽可能的列举出cube开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型
- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

- UA不是随机生成

## 06-二开计划

- https://github.com/Goqi/Ercube