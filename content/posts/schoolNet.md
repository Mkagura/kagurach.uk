---
title: "Writeup: APIs of a certain service"
date: 2023-04-01T13:23:39+08:00
draft: true
---

# 0 USE AT YOUR OWN RISK

Basic Variable Tables:
|--|--|
|Vars|Description|
|base_url|The base URL of that site,with port (801);|
|myip|Your ip got from DHCP server|
|hostname|You should catch this yourself|

# 1 Login
vars:
|---|---|
|wlanacip|Catch yourself *'never changed?'*|
|id|your id in school|
|pass|your password|
|op|your network operator|
|PHPSESSID|PHPSESSID|
|Unknown|I dont know what this id *This is an ip addr begins with 10.*|
code:
```bash
curl 'http://'${base_url}'/eportal/?c=ACSetting&a=Login&protocol=http:&hostname='${hostname}'&iTermType=1&wlanuserip='${myip}'&wlanacip='${wlanacip}'0&wlanacname=XL-BRAS-SR8806-X&mac=00-00-00-00-00-00&ip='${myip}'&enAdvert=0&queryACIP=0&loginMethod=1' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H 'Cookie: program=2; vlan=0; ip='${Unknown}'; md5_login2=%2C0%2C'${id}'@'${op}'%7C'${pass}'; ssid=null; areaID=null; PHPSESSID='${PHPSESSID}'' \
  --data-raw 'DDDDD=%2C0%2C'${id}'%40'${op}'&upass='${pass}'&R1=0&R2=0&R3=0&R6=0&para=00&0MKKey=123456&buttonClicked=&redirect_url=&err_flag=&username=&password=&user=&cmd=&Login=&v6ip=' 
  --insecure
```

# 2 Send a SMS
vars:
|---|---|
|n|phone number|
|t|JS: Math.random()|
code:
```bash
curl -X GET 'http://'${base_url}'/sms.php?p='${n}'&t=${t}' \ 
-H 'Content-Type: application/x-www-form-urlencoded'
```
Return:
```json
{"result":"ok","msg":"","acc":"${n}'","pwd":"..."}

```
# 3 Log an user off
var:
|---|---|
|idop|your id in school with operator's suffix|
|callback|callback,like *jQueryxxxxxxxxxxxxxxx_yyyyyyyyyyyyyyyyyyy*|

code:
```bash
curl 'http://'${base_url}'/1/eportal/?c=IsOnline&a=logout&callback='${callback}'&account='${idop}'&_=156467841534' \
  -H 'Accept: */*' \
  -H 'Connection: keep-alive' \
  -H 'Referer: '${base_url}'/' \
  --insecure
  ```