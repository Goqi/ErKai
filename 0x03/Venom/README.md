# Venom代码分析

本文将从代码层面深入分析Venom项目。Venom是一款为渗透测试人员设计的使用Go开发的多级代理流量转发工具。支持流量加密、执行操作系统命令等。可将多个节点进行连接，然后以节点为跳板，构建多级代理。渗透测试人员可以使用Venom轻松地将网络流量代理到多层内网，并轻松地管理代理节点。

本文会持续更新，创建于2022年11月02日，最近的一次更新时间为2022年11月02日。

- [01-第三方库]()
- [02-开发设计]()
- [03-代码分析]()
- [04-不足之处]()
- [05-二开计划]()

## 01-第三方库

- https://github.com/Goqi/ErVenom/blob/main/go.mod
- https://github.com/Goqi/ErVenom/blob/main/go.sum
- [ ] https://github.com/cheggaaa/pb | 程序运行进度条
- [ ] https://github.com/davecgh/go-spew | 对Go代码的数据结构打印调试
- [ ] https://github.com/fatih/color | 程序运行终端颜色打印
- [ ] http://github.com/libp2p/go-reuseport | 重用 tcp/udp 端口
- [ ] https://github.com/mattn/go-colorable | 程序运行终端颜色打印
- [ ] http://github.com/stretchr/objx | 处理Go内置数据类型
- [ ] http://github.com/stretchr/testify | 测试测试代码
- [ ] http://golang.org/x/crypto
- [ ] http://github.com/pkg/errors
- [ ] https://github.com/mattn/go-isatty
- [ ] http://github.com/mattn/go-runewidth
- [ ] https://github.com/pmezard/go-difflib

## 02-开发设计

本部分尽可能的列举出Venom开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 基本类型
- [ ] 函数方法
- [ ] 并发协程

## 03-代码分析

## 04-不足之处

## 05-二开计划

- https://github.com/Goqi/ErVenom