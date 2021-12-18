## Web Applications Fingerprinting tools



- [ ] wappaylzer

  you can use wappaylyzer extension for [firefox](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/) or [chrome](https://chrome.google.com/webstore/search/wappalyzer) browsers for web apps fingerprinting
  
- [ ] builtwith

  you can use builtwith extension for [firefox](https://addons.mozilla.org/en-US/firefox/addon/builtwith/) or [chrome](https://chrome.google.com/webstore/detail/builtwith-technology-prof/dapjbgnjinbpoindlpdmhochffioedbn) browsers for web apps fingerprinting
  
- [ ] httpx

  you can use httpx `-web-server` and `-tech-detect` options

  ``` bash
  # a single domain
  echo example.com | httpx -web-server -tech-detect
  
  # a list of subdomins
  cat subdomains_list.txt | httpx -web-server -tech-detect
  
  httpx -l subdomains_list.txt -web-server -tech-detect
  ```

- [ ] Aquatone

  [Aquatone](https://github.com/michenriksen/aquatone) is a tool for visual inspection of websites across a large amount of hosts and is convenient for quickly gaining an overview of HTTP-based attack surface.

  

  ```bash
  cat hosts.txt | aquatone
  ```

  

- [ ] nuclei

  [nuclei](https://github.com/projectdiscovery/nuclei) project has a good list of [templates](https://github.com/projectdiscovery/nuclei-templates) to fingerprint web apps

  ```bash
  nuclei -t ~/nuclei-templates -tags tech -u https://example.com -c 200
  ```

- [ ] whatweb

  [whatweb](https://github.com/urbanadventurer/WhatWeb) has an 1800 plugin to identify technologies, you can use it to fingerprint web apps

  ```bash
  # a single host
  whatweb example.com
  
  # a list of hosts
  whatweb --input-file=hosts.txt
  ```

  

- [ ] Error messages

  you can identify technologies via error messages, if a web app does not handle errors, and you sent malformed data to the web app, this data will cause an error, and this error may reveal the  back-end technology

  ```http
  POST / HTTP/1.1
  Host: example.com
  User-Agent: curl/7.74.0
  Accept: */*
  Content-type: application/json
  Content-Length: 8
  
  {"test":d
  ```

  you can enumerate the web app endpoints and start fuzzing them with different http methods, http headers, and body

  

  - fuzzing http methods

  ```http
  METHOD /ENDPOINT HTTP/1.1
  Host: example.com
  User-Agent: curl/7.74.0
  Accept: */*
  
  ```

  

  ```bash
  ffuf -w http_methods.txt:METHOD -w endpoints.txt:ENDPOINT -request http_request.txt
  ```

  - fuzzing http headers

  ```http
  GET /ENDPOINT HTTP/1.1
  Host: example.com
  User-Agent: curl/7.74.0
  Accept: */*
  ```

  

  ```bash
  ffuf -w http_headers_names.txt:NAME -w http_headers_values.txt:VALUE -w endpoints.txt:ENDPOINT -request http_request.txt  -H "NAME: VALUE"
  ```

  

  

