---
title: Spring Cloud with Jaeger
date: 2019-09-30 13:08:29
tags: [spring-cloud, jaeger]
categories:
- [spring]
---

Jaeger is an implementation of OpenTracing API which is written in go hence has a very good performance. The other well-known implementation is Zipkin which is developed by Netflix in Java.

## 1. Running jaeger server
```sh
docker run -d --name jaeger \
  -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 \
  -p 5775:5775/udp \
  -p 6831:6831/udp \
  -p 6832:6832/udp \
  -p 5778:5778 \
  -p 16686:16686 \
  -p 14268:14268 \
  -p 9411:9411 \
  jaegertracing/all-in-one
```
## 2. Adding jaeger support to spring cloud
Add this dependency into the pom.xml:
```xml
        <!-- OpenTracing Tracing & Jaeger -->
        <dependency>
            <groupId>io.opentracing.contrib</groupId>
            <artifactId>opentracing-spring-cloud-starter</artifactId>
            <version>0.3.2</version>
            <!-- Exclude the modules that you don't want to trace or instrument -->
            <exclusions>
                <exclusion>
                    <groupId>io.opentracing.contrib</groupId>
                    <artifactId>opentracing-spring-cloud-jdbc-starter</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>io.opentracing.contrib</groupId>
                    <artifactId>opentracing-jms-1</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>io.opentracing.contrib</groupId>
                    <artifactId>opentracing-jms-spring</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>io.jaegertracing</groupId>
            <artifactId>jaeger-client</artifactId>
            <version>1.0.0</version>
        </dependency>
```

## 3. Create configuration bean

```java
import io.jaegertracing.internal.samplers.ProbabilisticSampler;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class TraceConfig {
    @Bean
    public io.opentracing.Tracer tracer() {
        return new io.jaegertracing.Configuration("manger-dashboard")
                .withSampler(new io.jaegertracing.Configuration.SamplerConfiguration().withType(ProbabilisticSampler.TYPE).withParam(1))
                .withReporter(new io.jaegertracing.Configuration.ReporterConfiguration())
                .getTracer();
        //return new // tracer instance of your choice (Zipkin, Jaeger, LightStep)
    }
}
```
