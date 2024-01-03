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