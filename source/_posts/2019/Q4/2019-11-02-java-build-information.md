---
title: Java build information
excerpt: Java build information
date: 2019-11-02 14:25:32
tags:
  - Java
categories: [Programming, Java]
---

# Detecting build version and time at runtime

It can often be useful to obtain information about artifact, version, build time and other at runtime.

## Build plugin configuration

```xml

    <build>
        <plugins>
            <plugin>
                <groupId>pl.project13.maven</groupId>
                <artifactId>git-commit-id-plugin</artifactId>
                <executions>
                    <execution>
                        <id>git-commit-id</id>
                        <goals>
                            <goal>revision</goal>
                        </goals>
                        <phase>initialize</phase>
                    </execution>
                </executions>
                <configuration>
                    <generateGitPropertiesFile>true</generateGitPropertiesFile>
                    <generateGitPropertiesFilename>${project.build.outputDirectory}/META-INF/git.properties</generateGitPropertiesFilename>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <layout>ZIP</layout>
                </configuration>
                <executions>
                    <execution>
                        <id>build-info</id>
                        <goals>
                            <goal>build-info</goal>
                        </goals>
                        <configuration>
                            <additionalProperties>
                                <encoding.source>UTF-8</encoding.source>
                                <java.source>${maven.compiler.source}</java.source>
                                <java.target>${maven.compiler.target}</java.target>
                                <java.version>${java.version}</java.version>
                                <java.specification.version>${java.specification.version}</java.specification.version>
                            </additionalProperties>
                            <outputFile>${project.build.outputDirectory}/META-INF/build-info.properties</outputFile>
                        </configuration>
                    </execution>
                    <execution>
                        <id>repackage</id>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

## Generated properties files

```bash
$ ls -l target/classes/META-INF/
total 4
-rwxrwxrwx 1 dongsheng dongsheng 288 Jan  1  1970 build-info.properties
-rwxrwxrwx 1 dongsheng dongsheng 895 Jan  1  1970 git.properties
```

## Accessing Build Properties

### Java Standard Library

```java
try (InputStream inputStream = getClass().getClassLoader().getResourceAsStream("META-INF/git.properties")) {
    Properties properties = new Properties();
    properties.load(inputStream);
    for (Map.Entry<Object, Object> e : properties.entrySet()) {
        Object key = e.getKey();
        Object value = e.getValue();
        System.out.println(String.format("%s='%s'", key, value));
    }
}
```

### Spring Framework

```java
@Autowired
private BuildProperties buildProperties;

@Autowired
private Environment environment;

buildProperties.getTime();
buildProperties.getGroup();
buildProperties.getArtifact();
buildProperties.getName();
buildProperties.getVersion();

buildProperties.get("property.name");

environment.getActiveProfiles();
environment.getProperty("property.name");
```
