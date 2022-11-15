# [nuclei](https://github.com/projectdiscovery/nuclei)代码分析

本文将从代码层面深入分析nuclei项目。nuclei是projectdiscovery项目发布的一款基于简单 YAML 的 DSL 的快速且可定制化的漏洞扫描器。nuclei 可以基于模板向目标发送请求，从而实现零误报，并提供对大量主机的快速扫描。Nuclei 支持各种协议的扫描，包括 TCP、DNS、HTTP、SSL、文件、Whois、Websocket、Headless 等。

本文创建于2021年3月14日，最近的一次更新时间为2022年11月15日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
├─cmd
│  ├─cve-annotate
│  │      main.go
│  │      
│  ├─docgen
│  │      docgen.go
│  │      
│  ├─functional-test
│  │      main.go
│  │      run.sh
│  │      testcases.txt
│  │      
│  ├─integration-test
│  │      code.go
│  │      custom-dir.go
│  │      dns.go
│  │      file.go
│  │      headless.go
│  │      http.go
│  │      integration-test.go
│  │      loader.go
│  │      network.go
│  │      offline-http.go
│  │      ssl.go
│  │      template-dir.go
│  │      template-path.go
│  │      websocket.go
│  │      whois.go
│  │      workflow.go
│  │      
│  └─nuclei
│          issue-tracker-config.yaml
│          main.go
│          
├─internal
│  ├─colorizer
│  │      colorizer.go
│  │      
│  └─runner
│      │  banner.go
│      │  defaults.go
│      │  doc.go
│      │  enumerate.go
│      │  healthcheck.go
│      │  options.go
│      │  proxy.go
│      │  runner.go
│      │  runner_test.go
│      │  templates.go
│      │  update.go
│      │  update_test.go
│      │  update_unix_test.go
│      │  
│      └─nucleicloud
│              cloud.go
│              types.go
│              
└─pkg
    ├─catalog
    │  │  catalogue.go
    │  │  
    │  ├─config
    │  │      config.go
    │  │      
    │  ├─disk
    │  │      catalog.go
    │  │      find.go
    │  │      path.go
    │  │      
    │  └─loader
    │      │  loader.go
    │      │  loader_test.go
    │      │  remote_loader.go
    │      │  
    │      └─filter
    │              path_filter.go
    │              tag_filter.go
    │              tag_filter_test.go
    │              
    ├─core
    │  │  engine.go
    │  │  engine_test.go
    │  │  execute.go
    │  │  workflow_execute.go
    │  │  workflow_execute_test.go
    │  │  workpool.go
    │  │  
    │  └─inputs
    │      │  inputs.go
    │      │  
    │      └─hybrid
    │              hmap.go
    │              hmap_test.go
    │              
    ├─model
    │  │  model.go
    │  │  model_test.go
    │  │  worflow_loader.go
    │  │  
    │  └─types
    │      ├─severity
    │      │      severities.go
    │      │      severity.go
    │      │      severity_test.go
    │      │      
    │      ├─stringslice
    │      │      stringslice.go
    │      │      
    │      └─userAgent
    │              user_agent.go
    │              
    ├─operators
    │  │  operators.go
    │  │  operators_test.go
    │  │  
    │  ├─common
    │  │  └─dsl
    │  │          dsl.go
    │  │          dsl_test.go
    │  │          
    │  ├─extractors
    │  │      compile.go
    │  │      doc.go
    │  │      extract.go
    │  │      extractors.go
    │  │      extractor_types.go
    │  │      extract_test.go
    │  │      util.go
    │  │      
    │  └─matchers
    │          compile.go
    │          doc.go
    │          match.go
    │          matchers.go
    │          matchers_types.go
    │          match_test.go
    │          validate.go
    │          validate_test.go
    │          
    ├─output
    │      doc.go
    │      file_output_writer.go
    │      format_json.go
    │      format_screen.go
    │      output.go
    │      output_test.go
    │      
    ├─parsers
    │      parser.go
    │      parser_test.go
    │      workflow_loader.go
    │      
    ├─progress
    │      doc.go
    │      progress.go
    │      
    ├─projectfile
    │      httputil.go
    │      project.go
    │      
    ├─protocols
    │  │  protocols.go
    │  │  
    │  ├─common
    │  │  ├─automaticscan
    │  │  │      automaticscan.go
    │  │  │      automaticscan_test.go
    │  │  │      doc.go
    │  │  │      
    │  │  ├─compare
    │  │  │      compare.go
    │  │  │      
    │  │  ├─contextargs
    │  │  │      args.go
    │  │  │      contextargs.go
    │  │  │      doc.go
    │  │  │      
    │  │  ├─executer
    │  │  │      executer.go
    │  │  │      
    │  │  ├─expressions
    │  │  │      expressions.go
    │  │  │      expressions_test.go
    │  │  │      variables.go
    │  │  │      variables_test.go
    │  │  │      
    │  │  ├─generators
    │  │  │      attack_types.go
    │  │  │      env.go
    │  │  │      generators.go
    │  │  │      generators_test.go
    │  │  │      load.go
    │  │  │      maps.go
    │  │  │      maps_test.go
    │  │  │      options.go
    │  │  │      slice.go
    │  │  │      validate.go
    │  │  │      
    │  │  ├─helpers
    │  │  │  ├─deserialization
    │  │  │  │  │  deserialization.go
    │  │  │  │  │  helpers.go
    │  │  │  │  │  java.go
    │  │  │  │  │  
    │  │  │  │  └─testdata
    │  │  │  │          Deserialize.java
    │  │  │  │          README.md
    │  │  │  │          ValueObject.java
    │  │  │  │          
    │  │  │  ├─eventcreator
    │  │  │  │      eventcreator.go
    │  │  │  │      
    │  │  │  ├─responsehighlighter
    │  │  │  │      hexdump.go
    │  │  │  │      response_highlighter.go
    │  │  │  │      response_highlighter_test.go
    │  │  │  │      
    │  │  │  └─writer
    │  │  │          writer.go
    │  │  │          
    │  │  ├─hosterrorscache
    │  │  │      hosterrorscache.go
    │  │  │      hosterrorscache_test.go
    │  │  │      
    │  │  ├─interactsh
    │  │  │      interactsh.go
    │  │  │      
    │  │  ├─marker
    │  │  │      marker.go
    │  │  │      
    │  │  ├─protocolinit
    │  │  │      init.go
    │  │  │      
    │  │  ├─protocolstate
    │  │  │      state.go
    │  │  │      
    │  │  ├─randomip
    │  │  │      randomip.go
    │  │  │      randomip_test.go
    │  │  │      
    │  │  ├─replacer
    │  │  │      replacer.go
    │  │  │      replacer_test.go
    │  │  │      
    │  │  ├─tostring
    │  │  │      tostring.go
    │  │  │      
    │  │  ├─utils
    │  │  │  ├─excludematchers
    │  │  │  │      excludematchers.go
    │  │  │  │      excludematchers_test.go
    │  │  │  │      
    │  │  │  └─vardump
    │  │  │          dump.go
    │  │  │          
    │  │  └─variables
    │  │          variables.go
    │  │          variables_test.go
    │  │          
    │  ├─dns
    │  │  │  dns.go
    │  │  │  dns_test.go
    │  │  │  dns_types.go
    │  │  │  operators.go
    │  │  │  operators_test.go
    │  │  │  request.go
    │  │  │  request_test.go
    │  │  │  
    │  │  └─dnsclientpool
    │  │          clientpool.go
    │  │          
    │  ├─file
    │  │      file.go
    │  │      find.go
    │  │      find_test.go
    │  │      operators.go
    │  │      operators_test.go
    │  │      request.go
    │  │      request_test.go
    │  │      
    │  ├─headless
    │  │  │  headless.go
    │  │  │  operators.go
    │  │  │  operators_test.go
    │  │  │  request.go
    │  │  │  
    │  │  └─engine
    │  │          action.go
    │  │          action_types.go
    │  │          engine.go
    │  │          http_client.go
    │  │          instance.go
    │  │          page.go
    │  │          page_actions.go
    │  │          page_actions_test.go
    │  │          rules.go
    │  │          util.go
    │  │          
    │  ├─http
    │  │  │  build_request.go
    │  │  │  build_request_test.go
    │  │  │  cluster.go
    │  │  │  cluster_test.go
    │  │  │  http.go
    │  │  │  http_method_types.go
    │  │  │  http_test.go
    │  │  │  operators.go
    │  │  │  operators_test.go
    │  │  │  request.go
    │  │  │  request_annotations.go
    │  │  │  request_annotations_test.go
    │  │  │  request_generator.go
    │  │  │  request_generator_test.go
    │  │  │  request_test.go
    │  │  │  signature.go
    │  │  │  utils.go
    │  │  │  validate.go
    │  │  │  
    │  │  ├─httpclientpool
    │  │  │      clientpool.go
    │  │  │      
    │  │  ├─race
    │  │  │      syncedreadcloser.go
    │  │  │      
    │  │  ├─raw
    │  │  │      doc.go
    │  │  │      raw.go
    │  │  │      raw_test.go
    │  │  │      
    │  │  ├─signer
    │  │  │      aws.go
    │  │  │      signer.go
    │  │  │      
    │  │  └─signerpool
    │  │          signerpool.go
    │  │          
    │  ├─network
    │  │  │  network.go
    │  │  │  network_input_types.go
    │  │  │  network_test.go
    │  │  │  operators.go
    │  │  │  operators_test.go
    │  │  │  request.go
    │  │  │  request_test.go
    │  │  │  
    │  │  └─networkclientpool
    │  │          clientpool.go
    │  │          
    │  ├─offlinehttp
    │  │      find.go
    │  │      find_test.go
    │  │      offlinehttp.go
    │  │      operators.go
    │  │      operators_test.go
    │  │      read_response.go
    │  │      read_response_test.go
    │  │      request.go
    │  │      
    │  ├─ssl
    │  │      ssl.go
    │  │      ssl_test.go
    │  │      
    │  ├─utils
    │  │      utils.go
    │  │      
    │  ├─websocket
    │  │      websocket.go
    │  │      
    │  └─whois
    │          whois.go
    │          
    ├─reporting
    │  │  reporting.go
    │  │  
    │  ├─dedupe
    │  │      dedupe.go
    │  │      dedupe_test.go
    │  │      
    │  ├─exporters
    │  │  ├─es
    │  │  │      elasticsearch.go
    │  │  │      
    │  │  ├─markdown
    │  │  │      markdown.go
    │  │  │      
    │  │  └─sarif
    │  │          sarif.go
    │  │          
    │  ├─format
    │  │      format.go
    │  │      format_test.go
    │  │      
    │  └─trackers
    │      ├─github
    │      │      github.go
    │      │      
    │      ├─gitlab
    │      │      gitlab.go
    │      │      
    │      └─jira
    │              jira.go
    │              
    ├─templates
    │  │  cluster.go
    │  │  compile.go
    │  │  doc.go
    │  │  log.go
    │  │  log_test.go
    │  │  preprocessors.go
    │  │  templates.go
    │  │  templates_doc.go
    │  │  templates_doc_examples.go
    │  │  templates_test.go
    │  │  workflows.go
    │  │  
    │  ├─cache
    │  │      cache.go
    │  │      cache_test.go
    │  │      
    │  └─types
    │          types.go
    │          
    ├─testutils
    │  │  integration.go
    │  │  testutils.go
    │  │  
    │  └─testheadless
    │          headless_local.go
    │          headless_runtime.go
    │          
    ├─types
    │      interfaces.go
    │      proxy.go
    │      resume.go
    │      types.go
    │      
    ├─utils
    │  │  insertion_ordered_map.go
    │  │  insertion_ordered_map_test.go
    │  │  template_path.go
    │  │  utils.go
    │  │  utils_test.go
    │  │  
    │  ├─monitor
    │  │      monitor.go
    │  │      monitor_test.go
    │  │      
    │  ├─ratelimit
    │  │      ratelimit.go
    │  │      ratelimit_test.go
    │  │      
    │  ├─stats
    │  │      doc.go
    │  │      stats.go
    │  │      
    │  └─yaml
    │          yaml_decode_wrapper.go
    │          
    └─workflows
            doc.go
            workflows.go
            workflows_test.go
```

## 02-官方包库

- [ ] [archive/tar](https://pkg.go.dev/archive/tar)
- [ ] [archive/zip](https://pkg.go.dev/archive/zip)
- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
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
- [ ] [crypto/x509/pkix](https://pkg.go.dev/crypto/x509/pkix)
- [ ] [database/sql/driver](https://pkg.go.dev/database/sql/driver)
- [ ] [debug/dwarf](https://pkg.go.dev/debug/dwarf)
- [ ] [debug/elf](https://pkg.go.dev/debug/elf)
- [ ] [embed](https://pkg.go.dev/embed)
- [ ] [encoding](https://pkg.go.dev/encoding)
- [ ] [encoding/asn1](https://pkg.go.dev/encoding/asn1)
- [ ] [encoding/base32](https://pkg.go.dev/encoding/base32)
- [ ] [encoding/base64](https://pkg.go.dev/encoding/base64)
- [ ] [encoding/binary](https://pkg.go.dev/encoding/binary)
- [ ] [encoding/csv](https://pkg.go.dev/encoding/csv)
- [ ] [encoding/gob](https://pkg.go.dev/encoding/gob)
- [ ] [encoding/hex](https://pkg.go.dev/encoding/hex)
- [ ] [encoding/json](https://pkg.go.dev/encoding/json)
- [ ] [encoding/pem](https://pkg.go.dev/encoding/pem)
- [ ] [encoding/xml](https://pkg.go.dev/encoding/xml)
- [ ] [errors](https://pkg.go.dev/errors)
- [ ] [expvar](https://pkg.go.dev/expvar)
- [ ] [flag](https://pkg.go.dev/flag)
- [ ] [fmt](https://pkg.go.dev/fmt)
- [ ] [hash](https://pkg.go.dev/hash)
- [ ] [hash/adler32](https://pkg.go.dev/hash/adler32)
- [ ] [hash/crc32](https://pkg.go.dev/hash/crc32)
- [ ] [hash/crc64](https://pkg.go.dev/hash/crc64)
- [ ] [hash/fnv](https://pkg.go.dev/hash/fnv)
- [ ] [html](https://pkg.go.dev/html)
- [ ] [html/template](https://pkg.go.dev/html/template)
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
- [ ] [net/http/httptest](https://pkg.go.dev/net/http/httptest)
- [ ] [net/http/httptrace](https://pkg.go.dev/net/http/httptrace)
- [ ] [net/http/httputil](https://pkg.go.dev/net/http/httputil)
- [ ] [net/http/internal](https://pkg.go.dev/net/http/internal)
- [ ] [net/http/internal/ascii](https://pkg.go.dev/net/http/internal/ascii)
- [ ] [net/http/internal/testcert](https://pkg.go.dev/net/http/internal/testcert)
- [ ] [net/http/pprof](https://pkg.go.dev/net/http/pprof)
- [ ] [net/netip](https://pkg.go.dev/net/netip)
- [ ] [net/textproto](https://pkg.go.dev/net/textproto)
- [ ] [net/url](https://pkg.go.dev/net/url)
- [ ] [os](https://pkg.go.dev/os)
- [ ] [os/exec](https://pkg.go.dev/os/exec)
- [ ] [os/signal](https://pkg.go.dev/os/signal)
- [ ] [os/user](https://pkg.go.dev/os/user)
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [regexp/syntax](https://pkg.go.dev/regexp/syntax)
- [ ] [runtime](https://pkg.go.dev/runtime)
- [ ] [runtime/cgo](https://pkg.go.dev/runtime/cgo)
- [ ] [runtime/debug](https://pkg.go.dev/runtime/debug)
- [ ] [runtime/internal/atomic](https://pkg.go.dev/runtime/internal/atomic)
- [ ] [runtime/internal/math](https://pkg.go.dev/runtime/internal/math)
- [ ] [runtime/internal/sys](https://pkg.go.dev/runtime/internal/sys)
- [ ] [runtime/pprof](https://pkg.go.dev/runtime/pprof)
- [ ] [runtime/trace](https://pkg.go.dev/runtime/trace)
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
- [ ] [time](https://pkg.go.dev/time)
- [ ] [unicode](https://pkg.go.dev/unicode)
- [ ] [unicode/utf16](https://pkg.go.dev/unicode/utf16)
- [ ] [unicode/utf8](https://pkg.go.dev/unicode/utf8)
- [ ] [unsafe](https://pkg.go.dev/unsafe)

## 03-第三方库

- [ ] https://github.com/alecthomas/jsonschema | 从 Go 类型生成 JSON 模式
- [ ] https://github.com/andygrunwald/go-jira
- [ ] https://github.com/antchfx/htmlquery
- [ ] https://github.com/antchfx/xmlquery
- [ ] https://github.com/antchfx/xpath
- [ ] https://github.com/apex/log
- [ ] https://github.com/asaskevich/govalidator
- [ ] https://github.com/aws/aws-sdk-go/aws
- [ ] https://github.com/aymerick/douceur/css
- [ ] https://github.com/aymerick/douceur/parser
- [ ] https://github.com/bits-and-blooms/bitset
- [ ] https://github.com/bits-and-blooms/bloom/v3
- [ ] https://github.com/blang/semver
- [ ] https://github.com/bluele/gcache
- [ ] https://github.com/c4milo/unpackit
- [ ] https://github.com/caddyserver/certmagic
- [ ] https://github.com/cespare/xxhash
- [ ] https://github.com/cespare/xxhash/v2
- [ ] https://github.com/cnf/structhash
- [ ] https://github.com/cockroachdb/logtags
- [ ] https://github.com/cockroachdb/redact
- [ ] https://github.com/cockroachdb/redact/internal
- [ ] https://github.com/cockroachdb/redact/internal/fmtsort
- [ ] https://github.com/cockroachdb/sentry-go
- [ ] https://github.com/corpix/uarand | 随机生成user-agent
- [ ] https://github.com/DataDog/gostackparse
- [ ] https://github.com/DataDog/zstd
- [ ] https://github.com/dimchansky/utfbom
- [ ] https://github.com/docker/go-units
- [ ] https://github.com/dsnet/compress
- [ ] https://github.com/dsnet/compress/bzip2
- [ ] https://github.com/dsnet/compress/bzip2/internal/sais
- [ ] https://github.com/dsnet/compress/internal
- [ ] https://github.com/dsnet/compress/internal/errors
- [ ] https://github.com/dsnet/compress/internal/prefix
- [ ] https://github.com/dustin/go-humanize
- [ ] https://github.com/fatih/structs
- [ ] https://github.com/goburrow/cache
- [ ] https://github.com/gobwas/httphead
- [ ] https://github.com/gobwas/pool
- [ ] https://github.com/gobwas/pool/internal/pmath
- [ ] https://github.com/gobwas/pool/pbufio
- [ ] https://github.com/gobwas/pool/pbytes
- [ ] https://github.com/gobwas/ws
- [ ] https://github.com/gobwas/ws/wsutil
- [ ] https://github.com/gogo/protobuf/proto
- [ ] https://github.com/gogo/protobuf/sortkeys
- [ ] https://github.com/gogo/protobuf/types
- [ ] https://github.com/golang/groupcache/lru
- [ ] https://github.com/golang/protobuf/proto
- [ ] https://github.com/golang/snappy
- [ ] https://github.com/golang-jwt/jwt/v4
- [ ] https://github.com/google/go-github/github
- [ ] https://github.com/google/go-querystring/query
- [ ] https://github.com/google/uuid
- [ ] https://github.com/go-ole/go-ole
- [ ] https://github.com/go-ole/go-ole/oleutil
- [ ] https://github.com/go-playground/locales
- [ ] https://github.com/go-playground/locales/currency
- [ ] https://github.com/go-playground/universal-translator
- [ ] https://github.com/go-playground/validator/v10
- [ ] https://github.com/gorilla/css/scanner
- [ ] https://github.com/gosuri/uilive
- [ ] https://github.com/gosuri/uiprogress
- [ ] https://github.com/gosuri/uiprogress/util/strutil
- [ ] https://github.com/h2non/filetype
- [ ] https://github.com/hashicorp/go-cleanhttp
- [ ] https://github.com/hashicorp/go-retryablehttp
- [ ] https://github.com/hashicorp/go-version
- [ ] https://github.com/hdm/jarm-go
- [ ] https://github.com/iancoleman/orderedmap
- [ ] https://github.com/itchyny/gojq
- [ ] https://github.com/itchyny/timefmt-go
- [ ] https://github.com/jmespath/go-jmespath
- [ ] https://github.com/json-iterator/go  | json处理库
- [ ] https://github.com/karlseguin/ccache
- [ ] https://github.com/klauspost/compress/flate
- [ ] https://github.com/klauspost/cpuid/v2
- [ ] https://github.com/klauspost/pgzip
- [ ] https://github.com/Knetic/govaluate | Golang的任意表达式求值
- [ ] https://github.com/kr/pretty
- [ ] https://github.com/kr/text
- [ ] https://github.com/leodido/go-urn
- [ ] https://github.com/libdns/libdns
- [ ] https://github.com/logrusorgru/aurora | 命令行颜色处理
- [ ] https://github.com/lor00x/goldap/message
- [ ] https://github.com/mattn/go-isatty
- [ ] https://github.com/mattn/go-runewidth
- [ ] https://github.com/mholt/acmez
- [ ] https://github.com/mholt/acmez/acme
- [ ] https://github.com/mholt/archiver
- [ ] https://github.com/microcosm-cc/bluemonday
- [ ] https://github.com/microcosm-cc/bluemonday/css
- [ ] https://github.com/miekg/dns
- [ ] https://github.com/mitchellh/go-homedir
- [ ] https://github.com/modern-go/concurrent
- [ ] https://github.com/modern-go/reflect2
- [ ] https://github.com/Mzack9999/go-http-digest-auth-client
- [ ] https://github.com/Mzack9999/ldapserver
- [ ] https://github.com/nwaples/rardecode
- [ ] https://github.com/olekukonko/tablewriter
- [ ] https://github.com/openrdap/rdap
- [ ] https://github.com/openrdap/rdap/bootstrap
- [ ] https://github.com/openrdap/rdap/bootstrap/cache
- [ ] https://github.com/openrdap/rdap/sandbox
- [ ] https://github.com/owenrumney/go-sarif/v2/sarif
- [ ] https://github.com/pierrec/lz4
- [ ] https://github.com/pierrec/lz4/internal/xxh32
- [ ] https://github.com/pkg/errors
- [ ] https://github.com/projectdiscovery/blackrock
- [ ] https://github.com/projectdiscovery/clistats
- [ ] https://github.com/projectdiscovery/cryptoutil
- [ ] https://github.com/projectdiscovery/fastdialer/fastdialer
- [ ] https://github.com/projectdiscovery/filekv
- [ ] https://github.com/projectdiscovery/fileutil
- [ ] https://github.com/projectdiscovery/folderutil
- [ ] https://github.com/projectdiscovery/goflags
- [ ] https://github.com/projectdiscovery/goflags
- [ ] https://github.com/projectdiscovery/gologger
- [ ] https://github.com/projectdiscovery/hmap
- [ ] https://github.com/projectdiscovery/interactsh
- [ ] https://github.com/projectdiscovery/iputil
- [ ] https://github.com/projectdiscovery/mapcidr
- [ ] https://github.com/projectdiscovery/networkpolicy
- [ ] https://github.com/projectdiscovery/nuclei-updatecheck-api/client
- [ ] https://github.com/projectdiscovery/rawhttp
- [ ] https://github.com/projectdiscovery/rawhttp/client
- [ ] https://github.com/projectdiscovery/rawhttp/clientpipeline
- [ ] https://github.com/projectdiscovery/rawhttp/proxy
- [ ] https://github.com/projectdiscovery/retryabledns
- [ ] https://github.com/projectdiscovery/retryabledns/doh
- [ ] https://github.com/projectdiscovery/retryabledns/hostsfile
- [ ] https://github.com/projectdiscovery/retryablehttp-go
- [ ] https://github.com/projectdiscovery/sliceutil
- [ ] https://github.com/projectdiscovery/stringsutil
- [ ] https://github.com/projectdiscovery/tlsx/pkg/connpool
- [ ] https://github.com/projectdiscovery/tlsx/pkg/output/stats
- [ ] https://github.com/projectdiscovery/tlsx/pkg/tlsx
- [ ] https://github.com/projectdiscovery/tlsx/pkg/tlsx/auto
- [ ] https://github.com/projectdiscovery/tlsx/pkg/tlsx/clients
- [ ] https://github.com/projectdiscovery/tlsx/pkg/tlsx/jarm
- [ ] https://github.com/projectdiscovery/tlsx/pkg/tlsx/openssl
- [ ] https://github.com/projectdiscovery/tlsx/pkg/tlsx/tls
- [ ] https://github.com/projectdiscovery/tlsx/pkg/tlsx/ztls
- [ ] https://github.com/projectdiscovery/tlsx/pkg/tlsx/ztls/ja3
- [ ] https://github.com/projectdiscovery/urlutil
- [ ] https://github.com/projectdiscovery/wappalyzergo
- [ ] https://github.com/projectdiscovery/yamldoc-go/encoder
- [ ] https://github.com/remeh/sizedwaitgroup
- [ ] https://github.com/rivo/uniseg
- [ ] https://github.com/rogpeppe/go-internal/fmtsort
- [ ] https://github.com/rs/xid | 全球唯一ID生成器
- [ ] https://github.com/saintfish/chardet
- [ ] https://github.com/segmentio/ksuid
- [ ] https://github.com/shirou/gopsutil/v3/cpu
- [ ] https://github.com/shirou/gopsutil/v3/internal/common
- [ ] https://github.com/shirou/gopsutil/v3/mem
- [ ] https://github.com/shirou/gopsutil/v3/net
- [ ] https://github.com/shirou/gopsutil/v3/process
- [ ] https://github.com/spaolacci/murmur3
- [ ] https://github.com/spf13/cast
- [ ] https://github.com/syndtr/goleveldb/leveldb
- [ ] https://github.com/tj/go-update
- [ ] https://github.com/trivago/tgo/tcontainer
- [ ] https://github.com/trivago/tgo/treflect
- [ ] https://github.com/ulule/deepcopier
- [ ] https://github.com/valyala/bytebufferpool
- [ ] https://github.com/valyala/fasttemplate
- [ ] https://github.com/weppos/publicsuffix-go/publicsuffix
- [ ] https://github.com/xanzy/go-gitlab
- [ ] https://github.com/xi2/xz
- [ ] https://github.com/yl2chen/cidranger
- [ ] https://github.com/yl2chen/cidranger/net
- [ ] https://github.com/ysmood/goob
- [ ] https://github.com/ysmood/gson
- [ ] https://github.com/ysmood/leakless
- [ ] https://github.com/ysmood/leakless/pkg/shared
- [ ] https://github.com/ysmood/leakless/pkg/utils
- [ ] https://github.com/yusufpapurcu/wmi
- [ ] https://github.com/zmap/rc2
- [ ] https://github.com/zmap/zcrypto/dsa
- [ ] https://github.com/zmap/zcrypto/encoding/asn1
- [ ] https://github.com/zmap/zcrypto/internal/randutil
- [ ] https://github.com/zmap/zcrypto/json
- [ ] https://github.com/zmap/zcrypto/tls
- [ ] https://github.com/zmap/zcrypto/util
- [ ] https://github.com/zmap/zcrypto/x509
- [ ] https://github.com/zmap/zcrypto/x509/ct
- [ ] https://github.com/zmap/zcrypto/x509/pkix
- [ ] https://go.etcd.io/bbolt
- [ ] https://go.uber.org/atomic
- [ ] https://go.uber.org/multierr
- [ ] https://go.uber.org/zap
- [ ] https://go.uber.org/zap/buffer
- [ ] https://go.uber.org/zap/internal
- [ ] https://go.uber.org/zap/internal/bufferpool
- [ ] https://go.uber.org/zap/internal/color
- [ ] https://go.uber.org/zap/internal/exit
- [ ] https://go.uber.org/zap/zapcore
- [ ] https://goftp.io/server/v2
- [ ] https://goftp.io/server/v2/driver/file
- [ ] https://goftp.io/server/v2/ratelimit
- [ ] https://gopkg.in/alecthomas/kingpin.v2
- [ ] https://gopkg.in/corvus-ch/zbase32.v1
- [ ] https://gopkg.in/yaml.v2 |  YAML配置文件处理
- [ ] https://gopkg.in/yaml.v3 |  YAML配置文件处理
- [ ] [moul.io/http2curl](https://pkg.go.dev/moul.io/http2curl)
- [ ] github.com/andygrunwald/go-jira | jira客户端
- [ ] github.com/blang/semver | 语义版本控制库
- [ ] github.com/fatih/structs | 包含各种实用程序结构体
- [ ] github.com/golang/protobuf | 协议缓冲区的Go绑定
- [ ] github.com/google/go-github | 用于访问GitHub API的Go库
- [ ] github.com/mattn/go-runewidth | 获取固定宽度的字符或字符串
- [ ] github.com/miekg/dns | Go中的DNS库
- [ ] github.com/mitchellh/go-ps | Go的流程列表库
- [ ] github.com/olekukonko/tablewriter | Golang中的ASCII表
- [ ] github.com/pkg/errors | 错误处理库
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
- [ ] github.com/segmentio/ksuid | 稳定的全局唯一ID
- [ ] github.com/spaolacci/murmur3 | 本机MurmurHash3 Go实现
- [ ] github.com/spf13/cast | 安全的进行类型转换
- [ ] github.com/stretchr/testify | Go语言测试库
- [ ] github.com/syndtr/goleveldb | Go中的LevelDB键/值数据库
- [ ] github.com/valyala/fasttemplate | Go的简单快速模板引擎
- [ ] github.com/xanzy/go-gitlab | 一个GitLab API客户端

## 04-开发设计

本部分尽可能的列举出nuclei开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 基本类型
- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

- Poc未打包到可执行程序中
- UA存在规则特征
  - 代码较多

## 06-二开计划

- https://github.com/Goqi/Ernuclei