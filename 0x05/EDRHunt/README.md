# [EDRHunt](https://github.com/FourCoreLabs/EDRHunt)代码分析

本文将从代码层面深入分析EDRHunt项目。EDRHunt是FourCoreLabs团队开发的一款扫描Windows系统上安装了那些杀毒软件的程序。可以通过Windows 服务、驱动程序、进程、注册表、wmi 查找已安装的杀毒。

本文创建于2022年12月24日，最近的一次更新时间为2022年12月24日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
├─cmd
│  └─EDRHunt
│          main.go
│          
└─pkg
    ├─edrRecon
    │      collect.go
    │      directory.go
    │      drivers_windows.go
    │      edrdata.go
    │      edrRecon_test.go
    │      filechecker_windows.go
    │      privilege.go
    │      process_windows.go
    │      registry.go
    │      services_windows.go
    │      wmi_windows.go
    │      
    ├─resources
    │      edrRecon.go
    │      scan_edr.go
    │      systemdata.go
    │      
    ├─scanners
    │      scanner.go
    │      scan_bitdefender.go
    │      scan_carbonblack.go
    │      scan_checkpoint.go
    │      scan_crowdstrike.go
    │      scan_cybereason.go
    │      scan_cylance.go
    │      scan_cynet.go
    │      scan_deepinstinct.go
    │      scan_elastic.go
    │      scan_eset.go
    │      scan_fireeye.go
    │      scan_fortinet.go
    │      scan_kaspersky.go
    │      scan_limacharlie.go
    │      scan_malwarebytes.go
    │      scan_mcafee.go
    │      scan_qualys.go
    │      scan_sentinelone.go
    │      scan_sophos.go
    │      scan_symantec.go
    │      scan_trendmicro.go
    │      scan_win_defender.go
    │      
    └─util
        │  util.go
        │  
        └─wmi
                wmi.go
```

## 02-官方包库

- [ ] [context](https://pkg.go.dev/context)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/fs](https://pkg.go.dev/io/fs)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [math](https://pkg.go.dev/math)
- [ ] [math/rand](https://pkg.go.dev/math/rand)
- [ ] [net](https://pkg.go.dev/net)
- [ ] [net/netip](https://pkg.go.dev/net/netip)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/exec](https://pkg.go.dev/os/exec)
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
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

- [ ] https://github.com/bi-zone/go-fileversion | 用于查询 Windows 版本信息资源的 Go 包装器。
- [ ] https://github.com/hashicorp/go-multierror | 将错误列表表示为单个错误。
- [ ] https://github.com/spf13/cobra | 命令行参数
- [ ] https://github.com/yusufpapurcu/wmi | Windows WMI


## 04-开发设计

本部分尽可能的列举出EDRHunt开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型
- [ ] 函数方法
- [ ] 并发协程

项目亮点：

## 05-不足之处

- 杀毒软件支持的较少
- 不支持卸载杀毒软件

## 06-二开计划

- https://github.com/Goqi/AvHunt