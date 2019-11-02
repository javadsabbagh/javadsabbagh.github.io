---
title: JavaFX with Maven
date: 2019-10-30 15:18:59
tags: [javafx]
categories:
- Java
---

# Project structure:

Add module-info.java in the default package:
```java
module org.openjfx.hellofx {
    requires javafx.controls;
    exports org.openjfx.hellofx;
}
```

pom.xml file:
```xml
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.openjfx</groupId>
    <artifactId>HelloFXWithMaven</artifactId>
    <version>1.0-SNAPSHOT</version>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.openjfx</groupId>
            <artifactId>javafx-controls</artifactId>
            <version>12.0.1</version>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
                <configuration>
                    <release>11</release>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.openjfx</groupId>
                <artifactId>javafx-maven-plugin</artifactId>
                <version>0.0.2</version>
                <executions>
                    <execution>
                        <id>default-cli</id>
                        <configuration>
                            <mainClass>org.openjfx.hellofx/org.openjfx.hellofx.App</mainClass>
                        </configuration>
                    </execution>
                    <execution>
                        <id>debug</id>
                        <configuration>
                            <options>
                                <option>-agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=*:8000</option>
                            </options>
                            <mainClass>org.openjfx.hellofx/org.openjfx.hellofx.App</mainClass>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
    <reporting>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-checkstyle-plugin</artifactId>
                <configuration>
                    <configLocation>config/sun_checks.xml</configLocation>
                </configuration>
            </plugin>
        </plugins>
    </reporting>
</project>
```

## Run 
```sh
mvn javafx:run
```