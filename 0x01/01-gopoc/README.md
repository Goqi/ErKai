# 代码赏析之gopoc

本文将从代码层面深入分析理解gopoc项目。gopoc是一款基于模板的漏洞扫描工具，用户可以方便快速的进行漏洞扫描。也可以使用YAML文件快速的编写漏洞扫描规则。是一款优秀的漏洞扫描工具。

本文会持续更新，创建于2021年3月16日，最近的一次更新时间为2021年3月16日。

- [01-第三方库](https://github.com/0e0w/GolangCode/tree/main/04-nuclei#01-%E7%AC%AC%E4%B8%89%E6%96%B9%E5%BA%93)
- [02-开发设计]()
- [03-代码赏析]()

## 01-第三方库

- [ ] github.com/antlr/antlr4 | 功能强大的解析器生成器
- [ ] github.com/cpuguy83/go-md2man | 将markdown转换为roff
- [ ] github.com/fatih/color | 命令行彩色包装
- [ ] github.com/gogo/protobuf | Gadgets的协议缓冲区
- [ ] github.com/golang/protobuf | 支持Google的协议缓冲区
- [ ] github.com/mattn/go-colorable | Windows的彩色书写器
- [ ] github.com/mgutz/ansi | 用于创建ANSI彩色的字符串和代码
- [ ] github.com/onsi/ginkgo | 测试框架
- [ ] github.com/onsi/gomega | 测试框架
- [ ] github.com/sirupsen/logrus | 日志处理
- [ ] github.com/thoas/go-funk | 基于反射的现代Go库
- [ ] github.com/urfave/cli | 命令控制
- [ ] github.com/x-cray/logrus-prefixed-formatter | Logrus前缀日志格式化程序
- [ ] github.com/google/cel-go
- [ ] golang.org/x/crypto
- [ ] golang.org/x/net
- [ ] golang.org/x/sys
- [ ] google.golang.org/genproto
- [ ] google.golang.org/grpc
- [ ] gopkg.in/yaml.v2

## 02-开发设计

本部分尽可能的列举出nuclei开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 基本类型
- [ ] 函数方法
- [ ] 并发协程

## 03-代码赏析