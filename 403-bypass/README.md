# 403 Bypass



## FFUF

- [ ] Path fuzzing

```bash
ffuf -w 403_url_payloads.txt -u http://example.com/auth_pathFUZZ -fc 403,401,400
```



- [ ] HTTP Header Fuzzing

```bash
ffuf -w 403_bypass_header_names.txt:HEADER -w 403_bypass_header_values.txt:VALUE -u http://example.com/auth_path -H "HEADER:VALUE" -fc 403,401,400
```



- [ ] Common HTTP Ports Fuzzing

```bash
ffuf -w common-http-ports.txt:PORT -u http://example.com/auth_path -H "Host: example.com:PORT" -fc 403,401,400
```



- [ ] HTTP Methods Fuzzing

```bash
ffuf -w http-methods.txt:METHOD -u http://example.com/auth_path -X "METHOD" -fc 403,401,400
```



- [ ]  User Agent Fuzzing

```bash
ffuf -w user-agents.txt:AGENT -u http://example.com/auth_path -H "User-Agent: AGENT" -fc 403,401,400
```



## nuclei

```bash
nuclei -u http://example.com/auth_path/ -t 403-bypass-nuclei-templates -tags fuzz -timeout 10 -c 200 -v
```

**Note**: Add the slash symbol after the path whether it is a directory or file

Example:

- http://example.com/directory/
- http://example.com/directory/file.ext/

### Sources

- https://docs.google.com/presentation/d/1ek6DzXKBQd6xUiVNGRT33pMACs8M13CSoYCkgepDKZk/edit#slide=id.gaa5321e139_0_0
- https://github.com/Karanxa/Bug-Bounty-Wordlists/blob/main/403_header_payloads.txt
- https://github.com/Karanxa/Bug-Bounty-Wordlists/blob/main/403_url_payloads.txt
- https://github.com/iamthefrogy/frogy/blob/main/ports
- https://annevankesteren.nl/2007/10/http-methods
- https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/User-Agents/UserAgents.fuzz.txt

