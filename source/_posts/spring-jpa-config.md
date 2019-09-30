---
title: Common Spring JPA Config
date: 2019-09-30 13:23:55
tags: [spring-jpa]
categories:
- [spring]
---

## Add JPA data source with granule logging enabled

```ini
spring.datasource.url= jdbc:oracle:thin:@<db>:<port>:<sid>
spring.datasource.username=<user_name>
spring.datasource.password=<password>
spring.datasource.driver-class-name=oracle.jdbc.OracleDriver

spring.jpa.hibernate.ddl-auto=none
spring.jpa.generate-ddl=false
spring.jpa.show-sql=true
logging.level.org.hibernate.SQL=debug
logging.level.org.hibernate.type.descriptor.sql=trace
```

The last two configs are specific to Hibernates.