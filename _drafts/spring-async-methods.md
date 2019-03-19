---
layout: post
title:  "Spring Java - Working with Asynchronous (async) Methods"
author: sma
categories: [ Java, Spring, Spring, Spring-boot ]
image: assets/images/bridge-cars-evening-344544.jpg
tags: [featured]
---

Asynchronous (_async_) programming improves overall responsiveness of an application. By using  _asynchronous programming_ techniques you can avoid performance bottlenecks. In this article we will explore support of asynchronous programing in **Spring framework** using **Java**.

**Spring framework** provides **@Async** annotation, we can mark a method with **@Async** annotation  to make it _asynchronous_. **@Async** annotation will make the method execute in a separate thread so the caller of this method will not wait for the completion of called method.

### Project Overview and setup
We will build **GitHub User Information** lookup service in this article. Let's start by create simple spring boot application using [Spring Initializr](https://start.spring.io) generator. Open [Spring Initializr](https://start.spring.io) website and create new **Spring Boot** application using following configuration.

{:class="table table-responsive "}
| Option                | Value             |
| --------------------- | -------------     |
| Project Type          | Maven Project     |
| Language              | Java              |
| Version               | 2.1.3             |
| Group                 | im.sma            |
| Artifact              | spring-async      |
| Dependencies to add   | Web               |

![Spring Initializr]({{ site.baseurl }}/assets/images/spring-initializr.jpg)

Click the **Generate Project** button to download the project. Unzip and open the  project in your favorite IDE, I am using **IDEA IntelliJ**.

Open the `pom.xml` file and add `jackson-databind` dependency under `dependencies` section of opened `pom.xml` file.

```
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
</dependency>
```

Following listing shows the contents `pom.ml` file.

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.3.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>im.sma</groupId>
	<artifactId>spring-async</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>spring-async</name>
	<description>Demo project for Spring async methods</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

### Github user domain model class
Create a new class named `GitHubUser` under package `im.sma.springasync.domain`, the contents of the files are listed below.

```
package im.sma.springasync.domain;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class GitHubUser {
    private String name;
    private String blog;

    public GitHubUser() {
    }

    public GitHubUser(String name, String blog) {
        this.name = name;
        this.blog = blog;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getBlog() {
        return blog;
    }

    public void setBlog(String blog) {
        this.blog = blog;
    }

    @Override
    public String toString() {
        return "GitHubUser{" +
                "name='" + name + '\'' +
                ", blog='" + blog + '\'' +
                '}';
    }
}
```


The `@JsonIgnoreProperties` annotation signals Spring to ignore any attributes not listed in the class. 
**Note** *@JsonIgnoreProperties* annotation will ignore the attributes not listed in `GitHubUser` class during *serialization / deserialization* process using Jackson JSON library.

### Creating the Lookup service
Create a new package named `im.sma.springasync.service` and add a new class `GitHubLookupService` under this package.

```

```


    