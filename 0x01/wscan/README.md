# [wscan](https://github.com/chushuai/wscan)代码分析

本文将从代码层面深入分析理解wscan项目。wscan是chushuai开发的一款专注于WEB安全的扫描器，用来向Nmap致敬，转眼间Nmap已经开源25年了，Wscan也计划在未来25年内持续更新，持续开源。

本文会持续更新，创建于2022年11月14日，最近的一次更新时间为2022年11月14日。

- [01-项目结构]()
- [02-官方包库]()
- [03-第三方库]()
- [04-开发设计]()
- [05-不足之处]()
- [06-二开计划]()

## 01-项目结构

```
├─core
│  ├─assassin
│  │  │  crawlergo_test.go
│  │  │  main.go
│  │  │  
│  │  ├─collector
│  │  │  │  burp.go
│  │  │  │  config.go
│  │  │  │  dummy.go
│  │  │  │  mitm.go
│  │  │  │  mitm_test.go
│  │  │  │  request.go
│  │  │  │  service-list.go
│  │  │  │  url-list.go
│  │  │  │  
│  │  │  ├─basiccrawler
│  │  │  │      basic_crawler.go
│  │  │  │      
│  │  │  └─mitmhelper
│  │  │          basic_auth.go
│  │  │          encoding.go
│  │  │          httpmirror.go
│  │  │          ip_source.go
│  │  │          webctrl.go
│  │  │          webctrl_data.go
│  │  │          websocket.go
│  │  │          
│  │  ├─ctrl
│  │  │  │  bus.go
│  │  │  │  config.go
│  │  │  │  config_test.go
│  │  │  │  dispatcher.go
│  │  │  │  runner.go
│  │  │  │  
│  │  │  └─event_bus
│  │  │          event_bus.go
│  │  │          event_bus_test.go
│  │  │          
│  │  ├─entry
│  │  │      config.go
│  │  │      config_test.go
│  │  │      convert.go
│  │  │      entry.go
│  │  │      genca.go
│  │  │      reverse.go
│  │  │      service_scan.go
│  │  │      subdomain.go
│  │  │      upgrade.go
│  │  │      utils.go
│  │  │      webscan.go
│  │  │      
│  │  ├─http
│  │  │  │  client.go
│  │  │  │  config.go
│  │  │  │  cookie.go
│  │  │  │  flow.go
│  │  │  │  param.go
│  │  │  │  request.go
│  │  │  │  resource.go
│  │  │  │  response.go
│  │  │  │  statistics.go
│  │  │  │  transport.go
│  │  │  │  utils.go
│  │  │  │  utils_back.go
│  │  │  │  
│  │  │  ├─v1
│  │  │  │  │  client.go
│  │  │  │  │  client_test.go
│  │  │  │  │  clone.go
│  │  │  │  │  header.go
│  │  │  │  │  http.go
│  │  │  │  │  jar.go
│  │  │  │  │  request.go
│  │  │  │  │  request_test.go
│  │  │  │  │  response.go
│  │  │  │  │  response_test.go
│  │  │  │  │  server.go
│  │  │  │  │  transfer.go
│  │  │  │  │  
│  │  │  │  └─internal
│  │  │  │      │  chunked.go
│  │  │  │      │  chunked_test.go
│  │  │  │      │  
│  │  │  │      ├─ascii
│  │  │  │      │      print.go
│  │  │  │      │      print_test.go
│  │  │  │      │      
│  │  │  │      └─testcert
│  │  │  │              testcert.go
│  │  │  │              
│  │  │  ├─v2
│  │  │  │  │  base.go
│  │  │  │  │  base_delete_test.go
│  │  │  │  │  base_get_test.go
│  │  │  │  │  base_head_test.go
│  │  │  │  │  base_options_test.go
│  │  │  │  │  base_patch_test.go
│  │  │  │  │  base_post_test.go
│  │  │  │  │  base_put_test.go
│  │  │  │  │  example_test.go
│  │  │  │  │  file_upload.go
│  │  │  │  │  file_upload_test.go
│  │  │  │  │  go.mod
│  │  │  │  │  go.sum
│  │  │  │  │  LICENSE
│  │  │  │  │  README.md
│  │  │  │  │  request.go
│  │  │  │  │  request_test.go
│  │  │  │  │  response.go
│  │  │  │  │  response_test.go
│  │  │  │  │  session.go
│  │  │  │  │  utils.go
│  │  │  │  │  utils_test.go
│  │  │  │  │  
│  │  │  │  └─testdata
│  │  │  │          herefortheglob
│  │  │  │          mypassword
│  │  │  │          
│  │  │  └─v3
│  │  │      │  allocation_test.go
│  │  │      │  args.go
│  │  │      │  args_test.go
│  │  │      │  args_timing_test.go
│  │  │      │  brotli.go
│  │  │      │  brotli_test.go
│  │  │      │  bytesconv.go
│  │  │      │  bytesconv_32.go
│  │  │      │  bytesconv_32_test.go
│  │  │      │  bytesconv_64.go
│  │  │      │  bytesconv_64_test.go
│  │  │      │  bytesconv_table.go
│  │  │      │  bytesconv_table_gen.go
│  │  │      │  bytesconv_test.go
│  │  │      │  bytesconv_timing_test.go
│  │  │      │  client.go
│  │  │      │  client_example_test.go
│  │  │      │  client_test.go
│  │  │      │  client_timing_test.go
│  │  │      │  client_timing_wait_test.go
│  │  │      │  client_unix_test.go
│  │  │      │  coarseTime.go
│  │  │      │  coarseTime_test.go
│  │  │      │  compress.go
│  │  │      │  compress_test.go
│  │  │      │  cookie.go
│  │  │      │  cookie_test.go
│  │  │      │  cookie_timing_test.go
│  │  │      │  doc.go
│  │  │      │  fs.go
│  │  │      │  fs_example_test.go
│  │  │      │  fs_handler_example_test.go
│  │  │      │  fs_test.go
│  │  │      │  go.mod
│  │  │      │  go.sum
│  │  │      │  header.go
│  │  │      │  headers.go
│  │  │      │  header_regression_test.go
│  │  │      │  header_test.go
│  │  │      │  header_timing_test.go
│  │  │      │  http.go
│  │  │      │  http_test.go
│  │  │      │  lbclient.go
│  │  │      │  lbclient_example_test.go
│  │  │      │  LICENSE
│  │  │      │  methods.go
│  │  │      │  nocopy.go
│  │  │      │  peripconn.go
│  │  │      │  peripconn_test.go
│  │  │      │  README.md
│  │  │      │  requestctx_setbodystreamwriter_example_test.go
│  │  │      │  SECURITY.md
│  │  │      │  server.go
│  │  │      │  server_example_test.go
│  │  │      │  server_test.go
│  │  │      │  server_timing_test.go
│  │  │      │  status.go
│  │  │      │  status_test.go
│  │  │      │  status_timing_test.go
│  │  │      │  stream.go
│  │  │      │  streaming.go
│  │  │      │  streaming_test.go
│  │  │      │  stream_test.go
│  │  │      │  stream_timing_test.go
│  │  │      │  strings.go
│  │  │      │  tcp.go
│  │  │      │  tcpdialer.go
│  │  │      │  tcp_windows.go
│  │  │      │  timer.go
│  │  │      │  tls.go
│  │  │      │  TODO
│  │  │      │  uri.go
│  │  │      │  uri_test.go
│  │  │      │  uri_timing_test.go
│  │  │      │  uri_unix.go
│  │  │      │  uri_windows.go
│  │  │      │  uri_windows_test.go
│  │  │      │  userdata.go
│  │  │      │  userdata_test.go
│  │  │      │  userdata_timing_test.go
│  │  │      │  workerpool.go
│  │  │      │  workerpool_test.go
│  │  │      │  
│  │  │      ├─examples
│  │  │      │  │  README.md
│  │  │      │  │  
│  │  │      │  ├─client
│  │  │      │  │      .gitignore
│  │  │      │  │      client.go
│  │  │      │  │      Makefile
│  │  │      │  │      README.md
│  │  │      │  │      
│  │  │      │  ├─fileserver
│  │  │      │  │      .gitignore
│  │  │      │  │      fileserver.go
│  │  │      │  │      Makefile
│  │  │      │  │      README.md
│  │  │      │  │      ssl-cert-snakeoil.key
│  │  │      │  │      ssl-cert-snakeoil.pem
│  │  │      │  │      
│  │  │      │  ├─helloworldserver
│  │  │      │  │      .gitignore
│  │  │      │  │      helloworldserver.go
│  │  │      │  │      Makefile
│  │  │      │  │      README.md
│  │  │      │  │      
│  │  │      │  ├─host_client
│  │  │      │  │      .gitignore
│  │  │      │  │      hostclient.go
│  │  │      │  │      Makefile
│  │  │      │  │      README.md
│  │  │      │  │      
│  │  │      │  ├─letsencrypt
│  │  │      │  │      letsencryptserver.go
│  │  │      │  │      
│  │  │      │  └─multidomain
│  │  │      │          Makefile
│  │  │      │          multidomain.go
│  │  │      │          README.md
│  │  │      │          
│  │  │      ├─fasthttputil
│  │  │      │      doc.go
│  │  │      │      inmemory_listener.go
│  │  │      │      inmemory_listener_test.go
│  │  │      │      inmemory_listener_timing_test.go
│  │  │      │      pipeconns.go
│  │  │      │      pipeconns_test.go
│  │  │      │      rsa.key
│  │  │      │      rsa.pem
│  │  │      │      
│  │  │      └─fuzzit
│  │  │          ├─cookie
│  │  │          │      cookie_fuzz.go
│  │  │          │      
│  │  │          ├─request
│  │  │          │      request_fuzz.go
│  │  │          │      
│  │  │          ├─response
│  │  │          │      response_fuzz.go
│  │  │          │      
│  │  │          └─url
│  │  │                  url_fuzz.go
│  │  │                  
│  │  ├─model
│  │  │      vuln.go
│  │  │      
│  │  ├─output
│  │  │      htmlfile.go
│  │  │      html_data.go
│  │  │      stdout.go
│  │  │      webhook.go
│  │  │      
│  │  ├─plugins
│  │  │  │  plugins.go
│  │  │  │  
│  │  │  ├─base
│  │  │  │      bifrost.go
│  │  │  │      config.go
│  │  │  │      plugin.go
│  │  │  │      
│  │  │  ├─baseline
│  │  │  │      baseline.go
│  │  │  │      cookie.go
│  │  │  │      cors.go
│  │  │  │      header.go
│  │  │  │      host_injection.go
│  │  │  │      redirect.go
│  │  │  │      sensitive_info.go
│  │  │  │      serialization.go
│  │  │  │      server_error.go
│  │  │  │      ssl.go
│  │  │  │      unsafe_scheme.go
│  │  │  │      
│  │  │  ├─bruteforce
│  │  │  │      basicauth.go
│  │  │  │      bruteforce.go
│  │  │  │      bruteforce_finger_community.go
│  │  │  │      data.go
│  │  │  │      dvwa.go
│  │  │  │      formbrute_community.go
│  │  │  │      
│  │  │  ├─cmd_injection
│  │  │  │      cmd_injection.go
│  │  │  │      expression.go
│  │  │  │      generic.go
│  │  │  │      payload.go
│  │  │  │      phpcode.go
│  │  │  │      template.go
│  │  │  │      
│  │  │  ├─crlf_injection
│  │  │  │      crlf_injection.go
│  │  │  │      
│  │  │  ├─dirscan
│  │  │  │      backup.go
│  │  │  │      compare.go
│  │  │  │      dirscan.go
│  │  │  │      dirscan_data.go
│  │  │  │      sourcemap.go
│  │  │  │      yaml.go
│  │  │  │      
│  │  │  ├─fastjson
│  │  │  │      deserialization.go
│  │  │  │      fastjson.go
│  │  │  │      
│  │  │  ├─helper
│  │  │  │  ├─expr
│  │  │  │  │      CustomInt.go
│  │  │  │  │      element.go
│  │  │  │  │      expr.go
│  │  │  │  │      Origin.go
│  │  │  │  │      OriginNumberToExpr.go
│  │  │  │  │      OriginNumberToHex.go
│  │  │  │  │      RandInt.go
│  │  │  │  │      RandStr.go
│  │  │  │  │      SleepTime.go
│  │  │  │  │      Space.go
│  │  │  │  │      
│  │  │  │  ├─expression
│  │  │  │  │      expression.go
│  │  │  │  │      types.go
│  │  │  │  │      
│  │  │  │  ├─knowledge
│  │  │  │  │      know.go
│  │  │  │  │      
│  │  │  │  └─seeyon
│  │  │  │          encode.go
│  │  │  │          
│  │  │  ├─jsonp
│  │  │  │      jsonp.go
│  │  │  │      parser.go
│  │  │  │      
│  │  │  ├─path_traversal
│  │  │  │      path_traversal.go
│  │  │  │      payloads.go
│  │  │  │      
│  │  │  ├─phantasm
│  │  │  │  │  loader.go
│  │  │  │  │  phantasm.go
│  │  │  │  │  yaml_finger.go
│  │  │  │  │  yaml_poc_data.go
│  │  │  │  │  
│  │  │  │  └─pocs
│  │  │  │      └─gopoc
│  │  │  │              ecology-dbconfig-info-leak.go
│  │  │  │              poc_community.go
│  │  │  │              seeyon-htmlofficeservlet-rce.go
│  │  │  │              tomcat-cve-2020-1938.go
│  │  │  │              tomcat-put.go
│  │  │  │              tongda-arbitrarily-auth.go
│  │  │  │              tongda-lfi-upload-rce.go
│  │  │  │              
│  │  │  ├─redirect
│  │  │  │      redirect.go
│  │  │  │      script.go
│  │  │  │      
│  │  │  ├─shiro
│  │  │  │      default.go
│  │  │  │      deserialization.go
│  │  │  │      shiro.go
│  │  │  │      
│  │  │  ├─sql_injection
│  │  │  │  │  sql_injection.go
│  │  │  │  │  
│  │  │  │  └─sqli_detector
│  │  │  │      │  db_error.go
│  │  │  │      │  detector.go
│  │  │  │      │  
│  │  │  │      └─sqli_payload
│  │  │  │              base.go
│  │  │  │              booleaned_based.go
│  │  │  │              error_based.go
│  │  │  │              time_based.go
│  │  │  │              
│  │  │  ├─ssrf
│  │  │  │      payload.go
│  │  │  │      ssrf.go
│  │  │  │      
│  │  │  ├─struts
│  │  │  │      devmode.go
│  │  │  │      ognl.go
│  │  │  │      s2-005.go
│  │  │  │      s2-007.go
│  │  │  │      s2-009.go
│  │  │  │      s2-013.go
│  │  │  │      s2-015.go
│  │  │  │      s2-016.go
│  │  │  │      s2-032.go
│  │  │  │      s2-037.go
│  │  │  │      s2-045.go
│  │  │  │      s2-046.go
│  │  │  │      s2-052.go
│  │  │  │      s2-057.go
│  │  │  │      struts.go
│  │  │  │      
│  │  │  ├─thinkphp
│  │  │  │      invoke_rce.go
│  │  │  │      method_rce.go
│  │  │  │      preg_rce.go
│  │  │  │      sqli.go
│  │  │  │      thinkphp.go
│  │  │  │      v6_filewrite.go
│  │  │  │      
│  │  │  ├─upload
│  │  │  │      payloads.go
│  │  │  │      upload.go
│  │  │  │      
│  │  │  ├─xss
│  │  │  │  │  element.go
│  │  │  │  │  helper.go
│  │  │  │  │  pos_script.go
│  │  │  │  │  pos_style.go
│  │  │  │  │  pos_tag.go
│  │  │  │  │  pos_text.go
│  │  │  │  │  query_response.go
│  │  │  │  │  request_builder.go
│  │  │  │  │  xss.go
│  │  │  │  │  
│  │  │  │  └─js
│  │  │  │          esprima.go
│  │  │  │          parser.go
│  │  │  │          
│  │  │  └─xxe
│  │  │          blind.go
│  │  │          echo.go
│  │  │          payloads.go
│  │  │          xxe.go
│  │  │          
│  │  ├─resource
│  │  │      service.go
│  │  │      
│  │  ├─reverse
│  │  │  │  api_base.go
│  │  │  │  config.go
│  │  │  │  conn.go
│  │  │  │  db.go
│  │  │  │  dns_server.go
│  │  │  │  fetch.go
│  │  │  │  group.go
│  │  │  │  http_server.go
│  │  │  │  payload_template.go
│  │  │  │  reverse.go
│  │  │  │  rmi_server.go
│  │  │  │  server.go
│  │  │  │  
│  │  │  └─cland
│  │  │          cland_data.go
│  │  │          
│  │  └─utils
│  │      │  case_insensitive.go
│  │      │  cert.go
│  │      │  cert_test.go
│  │      │  file.go
│  │      │  math.go
│  │      │  network.go
│  │      │  print.go
│  │      │  rand.go
│  │      │  string.go
│  │      │  sync.go
│  │      │  tamper.go
│  │      │  test.go
│  │      │  time.go
│  │      │  url.go
│  │      │  
│  │      ├─buildinfo
│  │      │      info.go
│  │      │      oui_data.go
│  │      │      update.go
│  │      │      
│  │      ├─comparer
│  │      │  │  header.go
│  │      │  │  response.go
│  │      │  │  
│  │      │  ├─htmlcompare
│  │      │  │      compare.go
│  │      │  │      
│  │      │  └─strcompare
│  │      │          compare.go
│  │      │          utils.go
│  │      │          
│  │      ├─cusctx
│  │      │      ctx.go
│  │      │      
│  │      ├─guess
│  │      │      guess.go
│  │      │      response.go
│  │      │      value.go
│  │      │      
│  │      ├─rlimit
│  │      │      file.go
│  │      │      
│  │      └─ysoserial
│  │          │  ysoserial.go
│  │          │  
│  │          └─Gadgets
│  │                  CommonsBeanutils1.go
│  │                  CommonsBeanutils2.go
│  │                  CommonsCollectionsK1.go
│  │                  CommonsCollectionsK2.go
│  │                  gadget.go
│  │                  Jdk7u21.go
│  │                  Jdk8u20.go
│  │                  
│  ├─rpc
│  │  └─pb
│  │          gunkit.pb.go
│  │          
│  └─utils
│      │  config.go
│      │  connection.go
│      │  domainutil.go
│      │  host_n_port_utils.go
│      │  http_utils.go
│      │  iconhash.go
│      │  os_utils.go
│      │  rand_utils.go
│      │  str_utils.go
│      │  
│      ├─checker
│      │  │  error.go
│      │  │  request_checker.go
│      │  │  service_checker.go
│      │  │  url_checker.go
│      │  │  util.go
│      │  │  
│      │  ├─filter
│      │  │      badger_filter.go
│      │  │      sync_map_filter.go
│      │  │      
│      │  └─matcher
│      │          glob_matcher.go
│      │          hostname_matcher.go
│      │          interface.go
│      │          key_matcher.go
│      │          port_matcher.go
│      │          regexp_matcher.go
│      │          
│      ├─collections
│      │      queue.go
│      │      
│      ├─log
│      │      config.go
│      │      log.go
│      │      log_back.go
│      │      log_test.go
│      │      
│      ├─network
│      │      network.go
│      │      
│      └─printer
│          │  base.go
│          │  console.go
│          │  json.go
│          │  multi.go
│          │  text.go
│          │  
│          └─nice
│                  color.go
│                  
├─data
│  │  plugins.json
│  │  
│  ├─anti-tamper
│  │      anti-tamper.bin
│  │      
│  ├─background
│  │      background.db
│  │      
│  ├─illegal_feature
│  │      erotic.txt
│  │      gambling.txt
│  │      page_captcha.txt
│  │      routine_loan.txt
│  │      site_feature.json
│  │      
│  ├─keywords
│  │      keywords.json
│  │      keywords.txt
│  │      
│  ├─malurl
│  │      black_url.dat
│  │      whitelist.dat
│  │      
│  ├─pass_dict
│  │      tomcat_pass.dic
│  │      tomcat_user.dic
│  │      
│  ├─rules
│  │      cms_rule.json
│  │      cms_version.json
│  │      component_rule.json
│  │      server_rule.json
│  │      
│  └─sensitive
│          content_parse_rules.xml
│          db_server.xml
│          file_include.xml
│          uri_check_rules.xml
│          
├─doc
│  ├─img
│  │      Wscan.png
│  │      
│  └─plugins
│          blindsql.py
│          blindsql_time.py
│          crlf_injection.py
│          directory_traversal.py
│          xss.py
│          
└─ext
    ├─crawler
    │  │  analysis_page.go
    │  │  basic_task_hander.go
    │  │  body.go
    │  │  browser_task_hander.go
    │  │  check_url.go
    │  │  chrome_util.go
    │  │  client.go
    │  │  config.go
    │  │  crawer.go
    │  │  filter.go
    │  │  util.go
    │  │  
    │  └─js
    │          js.go
    │          
    ├─crawlergo
    │  └─pkg
    │      │  domain_collect.go
    │      │  path_expansion.go
    │      │  task_main.go
    │      │  
    │      ├─config
    │      │      config.go
    │      │      
    │      ├─engine
    │      │      after_dom_tasks.go
    │      │      after_loaded_tasks.go
    │      │      browser.go
    │      │      collect_links.go
    │      │      intercept_request.go
    │      │      tab.go
    │      │      
    │      ├─filter
    │      │      simple_filter.go
    │      │      smart_filter.go
    │      │      
    │      ├─js
    │      │      javascript.go
    │      │      
    │      ├─logger
    │      │      logger.go
    │      │      
    │      ├─model
    │      │      request.go
    │      │      url.go
    │      │      url_test.go
    │      │      
    │      └─tools
    │          │  common.go
    │          │  random.go
    │          │  
    │          └─requests
    │                  requests.go
    │                  response.go
    │                  utils.go
    │                  
    ├─fastdomain
    │  │  fastdomain.go
    │  │  
    │  ├─datasource
    │  │      alienvault.go
    │  │      ask.go
    │  │      baidu.go
    │  │      base.go
    │  │      bing.go
    │  │      brute.go
    │  │      certspotter.go
    │  │      crtsh.go
    │  │      dnsfinder.go
    │  │      fofa.go
    │  │      google.go
    │  │      hacktarget.go
    │  │      httpfinder.go
    │  │      ip138.go
    │  │      myssl.go
    │  │      qianxun.go
    │  │      quake.go
    │  │      rapiddns.go
    │  │      riskiq.go
    │  │      sogou.go
    │  │      sublist3r.go
    │  │      threatminer.go
    │  │      virustotal.go
    │  │      yahoo.go
    │  │      
    │  ├─dns
    │  │      client.go
    │  │      round_robin.go
    │  │      
    │  ├─geodb
    │  │      client.go
    │  │      geodb.go
    │  │      
    │  ├─model
    │  │      dicts.go
    │  │      dict_data.go
    │  │      model.go
    │  │      
    │  └─utils
    │          utils.go
    │          
    ├─pocassist
    │  │  LICENSE
    │  │  main.go
    │  │  README.md
    │  │  
    │  ├─api
    │  │  ├─middleware
    │  │  │  └─jwt
    │  │  │          jwt.go
    │  │  │          
    │  │  ├─msg
    │  │  │      msg.go
    │  │  │      
    │  │  └─routers
    │  │      │  route.go
    │  │      │  ui.go
    │  │      │  
    │  │      └─v1
    │  │          ├─auth
    │  │          │      auth.go
    │  │          │      
    │  │          ├─plugin
    │  │          │      plugin.go
    │  │          │      
    │  │          ├─scan
    │  │          │  ├─result
    │  │          │  │      result.go
    │  │          │  │      
    │  │          │  ├─scan
    │  │          │  │      scan.go
    │  │          │  │      
    │  │          │  └─task
    │  │          │          task.go
    │  │          │          
    │  │          ├─vulnerability
    │  │          │      vulnerability.go
    │  │          │      
    │  │          └─webapp
    │  │                  webapp.go
    │  │                  
    │  ├─cmd
    │  │      pocassist.go
    │  │      
    │  ├─docs
    │  │  │  docs.go
    │  │  │  swagger.json
    │  │  │  swagger.yaml
    │  │  │  
    │  │  └─pic
    │  │          
    │  ├─pkg
    │  │  ├─cel
    │  │  │  │  cel.go
    │  │  │  │  
    │  │  │  ├─proto
    │  │  │  │      http.pb.go
    │  │  │  │      http.proto
    │  │  │  │      
    │  │  │  └─reverse
    │  │  │          reverse.go
    │  │  │          
    │  │  ├─conf
    │  │  │      config.go
    │  │  │      default.go
    │  │  │      
    │  │  ├─db
    │  │  │      auth.go
    │  │  │      conn.go
    │  │  │      plugin.go
    │  │  │      scanResult.go
    │  │  │      scanTask.go
    │  │  │      vulnerability.go
    │  │  │      webapp.go
    │  │  │      
    │  │  ├─file
    │  │  │      file.go
    │  │  │      
    │  │  ├─logging
    │  │  │      log.go
    │  │  │      
    │  │  └─util
    │  │          jwt.go
    │  │          rand.go
    │  │          request.go
    │  │          request_test.go
    │  │          result.go
    │  │          tcp.go
    │  │          util.go
    │  │          version.go
    │  │          
    │  ├─poc
    │  │  ├─rule
    │  │  │      cel.go
    │  │  │      content.go
    │  │  │      controller.go
    │  │  │      handle.go
    │  │  │      parallel.go
    │  │  │      request.go
    │  │  │      rule.go
    │  │  │      run.go
    │  │  │      
    │  │  └─scripts
    │  │          poc-go-CVE-2020-17518.go
    │  │          poc-go-dedecms-bakfile-disclosure.go
    │  │          poc-go-ecshop-anyone-login.go
    │  │          poc-go-elasticsearch-path-traversal.go
    │  │          poc-go-exim-cve-2017-16943-uaf.go
    │  │          poc-go-exim-cve-2019-15846-rce.go
    │  │          poc-go-fastcgi-file-read.go
    │  │          poc-go-ftp-unauth.go
    │  │          poc-go-iis-ms15034.go
    │  │          poc-go-jboss-console-weakpwd.go
    │  │          poc-go-jboss-invoker-servlet-rce.go
    │  │          poc-go-jboss-serialization.go
    │  │          poc-go-joomla-serialization.go
    │  │          poc-go-memcached-unauth.go
    │  │          poc-go-mongo-unauth.go
    │  │          poc-go-openssl-heartbleed.go
    │  │          poc-go-redis-unauth.go
    │  │          poc-go-redis-weakpass.go
    │  │          poc-go-rsync-anonymous.go
    │  │          poc-go-shiro-unserialize-550.go
    │  │          poc-go-smb-cve-2020-0796.go
    │  │          poc-go-smb-ms17-010.go
    │  │          poc-go-tomcat-weak-pass.go
    │  │          poc-go-zookeeper-unauth.go
    │  │          scripts.go
    │  │          
    │  └─web
    │      │  bindata.go
    │      │  
    │      └─build
    │          └─static
    │              ├─css  
    │              ├─js  
    │              └─media
    │                      
    ├─yamlcel
    │  │  load.go
    │  │  
    │  └─client
    │      │  cel.go
    │      │  common.go
    │      │  event.go
    │      │  http.go
    │      │  parse.go
    │      │  parse_test.go
    │      │  reggen.go
    │      │  reverse.go
    │      │  server.go
    │      │  tcp.go
    │      │  udp.go
    │      │  
    │      ├─assets
    │      │  ├─html
    │      │  │      index.gohtml
    │      │  │      
    │      │  └─images
    │      │          core.svg
    │      │          logo.png
    │      │          running.png
    │      │          scan.gif
    │      │          
    │      └─http
    │              request.go
    │              response.go
    │              
    └─yamlpoc
        │  errors.go
        │  
        ├─cel
        │      cel.go
        │      definition.go
        │      implementation.go
        │      
        ├─parse
        │      parse.go
        │      
        ├─requests
        │      cache.go
        │      requests.go
        │      
        └─structs
                cache.go
                poc.go
                requests.pb.go
                requests.proto
                reverse.go
                tasks.go
```

## 02-官方包库

- [ ] [bufio](https://pkg.go.dev/bufio)
- [ ] [bytes](https://pkg.go.dev/bytes)
- [ ] [compress/flate](https://pkg.go.dev/compress/flate)
- [ ] [compress/gzip](https://pkg.go.dev/compress/gzip)
- [ ] [container/list](https://pkg.go.dev/container/list)
- [ ] [context](https://pkg.go.dev/context)
- [ ] [crypto/cipher](https://pkg.go.dev/crypto/cipher)
- [ ] [crypto/md5](https://pkg.go.dev/crypto/md5)
- [ ] [crypto/rand](https://pkg.go.dev/crypto/rand)
- [ ] [crypto/tls](https://pkg.go.dev/crypto/tls)
- [ ] [crypto/x509](https://pkg.go.dev/crypto/x509)
- [ ] [embed](https://pkg.go.dev/embed)
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
- [ ] [io](https://pkg.go.dev/io)
- [ ] [io/fs](https://pkg.go.dev/io/fs)
- [ ] [io/ioutil](https://pkg.go.dev/io/ioutil)
- [ ] [log](https://pkg.go.dev/log)
- [ ] [math/rand](https://pkg.go.dev/math/rand)
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
- [ ] [path](https://pkg.go.dev/path)
- [ ] [path/filepath](https://pkg.go.dev/path/filepath)
- [ ] [reflect](https://pkg.go.dev/reflect)
- [ ] [regexp](https://pkg.go.dev/regexp)
- [ ] [regexp/syntax](https://pkg.go.dev/regexp/syntax)
- [ ] [runtime](https://pkg.go.dev/runtime)
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

## 03-第三方库

- [ ] https://github.com/cpuguy83/go-md2man
- [ ] https://github.com/fatih/color
- [ ] https://github.com/google/martian
- [ ] https://github.com/mattn/go-colorable
- [ ] https://github.com/mattn/go-isatty
- [ ] https://github.com/russross/blackfriday
- [ ] https://github.com/urfave/cli/v2
- [ ] https://github.com/xrash/smetrics


## 04-开发设计

本部分尽可能的列举出ffuf开发中的一些设计模式、使用到的Go语言技术等。

- [ ] 数据类型

- [ ] 函数方法
- [ ] 并发协程

## 05-不足之处

## 06-二开计划

- https://github.com/Goqi/Erwscan