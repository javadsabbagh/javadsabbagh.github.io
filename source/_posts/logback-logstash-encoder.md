---
title: Logstash encoder for Logback
date: 2019-10-23 17:04:47
tags: [java, logging]
categories:
- Java
---

Create *src/resources/logback.xml* file:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    <springProperty scope="context" name="applicationName" source="spring.application.name"/>
    <springProperty scope="context" name="logstash-server" source="spring.logstash.server"/>
    <property resource="bootstrap.properties"/>
    <property name="CONSOLE_LOG_PATTERN"
              value="%clr(%d{yyyy-MM-dd HH:mm:ss.SSS}){faint} %clr(${LOG_LEVEL_PATTERN:-%5p}) %clr([${applicationName:-}]){yellow} %clr(${PID:- }){magenta} %clr(---){faint} %clr([%15.15t]){faint} %clr(%-40.40logger{39}){cyan} %clr(:){faint} %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}"/>

    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>${CONSOLE_LOG_PATTERN}</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <FileNamePattern>${logback.path}/${logback.fileName}.%d{yyyy-MM-dd}.%i.log.zip</FileNamePattern>
            <maxFileSize>${logback.maxFileSize}</maxFileSize>
            <maxHistory>${logback.maxHistory}</maxHistory>
        </rollingPolicy>
        <encoder class="net.logstash.logback.encoder.LogstashEncoder">
            <!--
                        Note: if timezone is not given, default system timezone will be set.
                        <timeZone>Asia/Tehran</timeZone>
                        <timeZone>UTC</timeZone>
            -->
        </encoder>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>${logback.file.level}</level>
        </filter>
    </appender>

    <root level="${logback.root.level}">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
    </root>

</configuration>
```

Add its dependency in pom.xml:
```xml
    <dependency>
        <groupId>net.logstash.logback</groupId>
        <artifactId>logstash-logback-encoder</artifactId>
        <version>5.3</version>
    </dependency>
```

Add to spring-boot property file (bootstrap.properties):

```ini
logback.path=${user.dir}/log
logback.maxFileSize=100MB
logback.maxHistory=150
logback.root.level=INFO
logback.file.level=INFO
logback.fileName=clr-returned
```