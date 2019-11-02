---
title: Keycloak Get Token
date: 2019-11-02 10:08:12
tags: [keycloak, jwt]
categories:
- DevOps
---

## <p dir='rtl'>گرفتن توکن جدید از keycloak</p>

<p dir='rtl'>در هر یک از روش های مورد نظر فیلدهای زیر را وارد نمایید:</p>
- server
- port
- realm
- client_id
- user_name
- passowrd

### CURL
```bash
curl -X POST \
  http://<server>:<port>/auth/realms/<realm>/protocol/openid-connect/token \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'client_id=<client_id>&username=<user_name>&password=<password>&grant_type=password'
```

### Postman
![postman keycloak token](get-token-postman.jpg)

### JMeter
![1. JMeter get token](jmeter-get-token-1.jpg)
![2. Http request config](jmeter-get-token-2.jpg)
![3. Request format](jmeter-get-token-3.jpg)
![4. Extract token to variable](jmeter-get-token-4.jpg)
