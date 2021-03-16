# 代码赏析之nuclei

本文将从代码层面深入分析理解nuclei项目。nuclei是一款基于模板的漏洞扫描工具，用户可以方便快速的进行漏洞扫描。也可以使用YAML文件快速的编写漏洞扫描规则。是一款优秀的漏洞扫描工具。

本文会持续更新，创建于2021年3月14日，最近的一次更新时间为2021年3月16日。

- [01-第三方库](https://github.com/0e0w/GolangCode/tree/main/04-nuclei#01-%E7%AC%AC%E4%B8%89%E6%96%B9%E5%BA%93)
- [02-开发设计]()
- [03-代码赏析]()

## 01-第三方库

- [ ] github.com/json-iterator/go | json处理库
- [ ] github.com/julienschmidt/httprouter | 轻量级高性能HTTP请求路由器
- [ ] github.com/hashicorp/go-retryablehttp | Go中可重试的HTTP客户端
- [ ] github.com/karrick/godirwalk | 快速的进行目录遍历
- [ ] github.com/Knetic/govaluate | Golang的任意表达式求值
- [ ] github.com/andygrunwald/go-jira | jira客户端
- [ ] github.com/blang/semver | 语义版本控制库
- [ ] github.com/corpix/uarand | 随机生成user-agent
- [ ] github.com/fatih/structs | 包含各种实用程序结构体
- [ ] github.com/go-rod/rod | web自动化工具和爬虫
- [ ] github.com/golang/protobuf | 协议缓冲区的Go绑定
- [ ] github.com/google/go-github | 用于访问GitHub API的Go库
- [ ] github.com/logrusorgru/aurora | 命令行颜色处理
- [ ] github.com/mattn/go-runewidth | 获取固定宽度的字符或字符串
- [ ] github.com/miekg/dns | Go中的DNS库
- [ ] github.com/mitchellh/go-ps | Go的流程列表库
- [ ] github.com/olekukonko/tablewriter | Golang中的ASCII表
- [ ] github.com/pkg/errors | 错误处理
- [ ] github.com/projectdiscovery/clistats | 命令行统计信息显示库
- [ ] github.com/projectdiscovery/collaborator | BurpSuite标准/私人协作者库
- [ ] github.com/projectdiscovery/fastdialer | 具有dns缓存+拨号历史记录的拨号程序
- [ ] github.com/projectdiscovery/goflags | Go官方flag库的包装器
- [ ] github.com/projectdiscovery/gologger | 非常简单的日志记录程序包
- [ ] github.com/projectdiscovery/hmap | 混合内存/磁盘映射
- [ ] github.com/projectdiscovery/rawhttp | 原始的HTTP客户端
- [ ] github.com/projectdiscovery/retryabledns | 可重试的DNS客户端
- [ ] github.com/projectdiscovery/retryablehttp-go | 可重试的HTTP客户端
- [ ] github.com/remeh/sizedwaitgroup | 协程任务处理
- [ ] github.com/rivo/uniseg | 处理Unicode文本
- [ ] github.com/rs/xid | 全球唯一ID生成器
- [ ] github.com/segmentio/ksuid | 稳定的全局唯一ID
- [ ] github.com/spaolacci/murmur3 | 本机MurmurHash3 Go实现
- [ ] github.com/spf13/cast | 安全的进行类型转换
- [ ] github.com/stretchr/testify | Go语言测试库
- [ ] github.com/syndtr/goleveldb | Go中的LevelDB键/值数据库
- [ ] github.com/valyala/fasttemplate | Go的简单快速模板引擎
- [ ] github.com/xanzy/go-gitlab | 一个GitLab API客户端
- [ ] github.com/hashicorp/go-cleanhttp
- [ ] github.com/trivago/tgo
- [ ] go.uber.org/atomic
- [ ] go.uber.org/multierr
- [ ] go.uber.org/ratelimit
- [ ] golang.org/x/crypto
- [ ] golang.org/x/net
- [ ] golang.org/x/oauth2
- [ ] golang.org/x/sync
- [ ] golang.org/x/sys
- [ ] golang.org/x/text
- [ ] golang.org/x/time
- [ ] google.golang.org/appengine
- [ ] gopkg.in/yaml.v2

## 02-开发设计

本部分尽可能的列举出nuclei开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 基本类型
- [ ] 函数方法
- [ ] 并发协程

## 03-代码赏析