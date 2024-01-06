- nginx logs query
```
{compose_service="nginx"} | json 
| __error__=`` 
| line_format " {{.status}} {{.client_ip}} {{.url}} {{.request_method}} {{.protocol}}"


```

-- nginx log volume graph 
```
count_over_time({compose_service="nginx"} |= `` [$__auto])
```


- mod sec logs 

-- logs query : 
```
{compose_service="nginx"} 
!= "nginxmainlog" 
|= "error" 
| json messages="transaction.messages[0].message", rule_id ="transaction.messages[0].details.ruleId", data ="transaction.messages[0].details.data", client_ip="transaction.client_ip" | line_format "Attacker -> {{.client_ip}} || Details -> {{.messages}} || Rule ID -> {{.rule_id}} \n More info -> {{.data}} " 
```

-- attacks per uri
```
sum by (uri) (count_over_time({compose_service="nginx"} != "nginxmainlog" | json uri="transaction.request.uri" |  __error__="" [$__interval]))

```
```
sum by (server,msg, rule_id) (count_over_time({compose_service="nginx"} | json uri="transaction.request.uri"  |= "$service" |= "ModSecurity" |= "coreruleset" | pattern "<date> <time> [<level>] <_> ModSecurity: <modsec_action> [file \"/usr/local/etc/modsecurity/coreruleset/rules/<rule_set>.conf<_> [id \"<rule_id>\"<_> [msg \"<msg>\"] <_>[ver \"<owasp_version>\"]<_>] [tag <tags>] [host<_>, client: <client>, server: <server>, request: \"<request_method> <ressource> <http_version>\", <_>" |  __error__="" [$__interval]))

```sum


-- bar chart o
