# Quarkus Doma Extension
[![Build](https://github.com/quarkiverse/quarkiverse-doma/workflows/Build/badge.svg)](https://github.com/quarkiverse/quarkiverse-doma/actions?query=workflow%3ABuild)
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-1-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

[Doma](https://github.com/domaframework/doma) is a database access framework with support for type-safe Criteria API and SQL templates.

This extension provides the following features:

- Hot reloading
- Automatic bean register
- Automatic SQL execution on startup
- Configuration
- Multiple Datasources
- Support for native images

### Hot reloading

In development mode, Doma extension reloads SQL and Script files when they are changed.

### Automatic bean register

Doma extension registers all DAO beans to the Quarkus CDI container.

### Automatic SQL import on startup

Doma extension executes ``import.sql`` when Quarkus starts.

### Configuration

You can write the following configurations in your application.properties file: 

```
quarkus.doma.sql-file-repository=greedy-cache
quarkus.doma.naming=none
quarkus.doma.exception-sql-log-type=none
quarkus.doma.dialect=h2
quarkus.doma.batch-size=10
quarkus.doma.fetch-size=50
quarkus.doma.max-rows=500
quarkus.doma.query-timeout=5000
quarkus.doma.sql-load-script=import.sql
```

The above properties are all optional.

### Multiple Datasources

You can bind Doma's configurations to each datasource as follows:

```
# default datasource
quarkus.datasource.db-kind=h2
quarkus.datasource.username=username-default
quarkus.datasource.jdbc.url=jdbc:h2:tcp://localhost/mem:default
quarkus.datasource.jdbc.min-size=3
quarkus.datasource.jdbc.max-size=13

# inventory datasource
quarkus.datasource.inventory.db-kind=h2
quarkus.datasource.inventory.username=username2
quarkus.datasource.inventory.jdbc.url=jdbc:h2:tcp://localhost/mem:inventory
quarkus.datasource.inventory.jdbc.min-size=2
quarkus.datasource.inventory.jdbc.max-size=12

# Doma's configuration bound to the above default datasource
quarkus.doma.dialect=h2
quarkus.doma.batch-size=10
quarkus.doma.fetch-size=50
quarkus.doma.max-rows=500
quarkus.doma.query-timeout=5000
quarkus.doma.sql-load-script=import.sql

# Doma's configuration bound to the above inventory datasource
quarkus.doma.inventory.dialect=h2
quarkus.doma.inventory.batch-size=10
quarkus.doma.inventory.fetch-size=50
quarkus.doma.inventory.max-rows=500
quarkus.doma.inventory.query-timeout=5000
quarkus.doma.inventory.sql-load-script=import.sql
```

You can inject the named Doma's resource 
using the `io.quarkus.agroal.DataSource` qualifier as follows:

```java
@Inejct
Config defaultConfig;

@Inejct
Entityql defaultEntityql;

@Inejct
NativeSql defaultNativeSql;

@Inejct
@DataSource("inventory")
Config invetoryConfig;

@Inejct
@DataSource("inventory")
Entityql inventoryEntityql;

@Inejct
@DataSource("inventory")
NativeSql inventoryNativeSql;
```

### Support for native images

Doma extension recognizes reflective classes and resources,
and includes them into your native image without additional configurations.

## Installing

### Gradle

```groovy
dependencies {
    annotationProcessor("org.seasar.doma:doma-processor:2.44.0")
    implementation("org.seasar.doma:doma-core:2.44.0")
    implementation("io.quarkiverse.doma:quarkus-doma-deployment:0.0.1-SNAPSHOT")
}
```

### Maven

```xml
...
<properties>
    <doma.version>2.44.0</doma.version>
    <quarkus-doma.version>0.0.1-SNAPSHOT</quarkus-doma.version>
    <compiler-plugin.version>3.8.1</compiler-plugin.version>
</properties>
...
<dependencies>
    <dependency>
        <groupId>org.seasar.doma</groupId>
        <artifactId>doma-core</artifactId>
        <version>${doma.version}</version>
    </dependency>
    <dependency>
        <groupId>io.quarkiverse.doma</groupId>
        <artifactId>quarkus-doma-deployment</artifactId>
        <version>${quarkus-doma.version}</version>
    </dependency>
</dependencies>
...
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>${compiler-plugin.version}</version>
            <configuration>
                <source>11</source>
                <target>11</target>
                <!-- the parameters=true option is critical so that RESTEasy works fine -->
                <parameters>true</parameters>
                <annotationProcessorPaths>
                    <path>
                        <groupId>org.seasar.doma</groupId>
                        <artifactId>doma-processor</artifactId>
                        <version>${doma.version}</version>
                    </path>
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/nakamura-to"><img src="https://avatars3.githubusercontent.com/u/404633?v=4" width="100px;" alt=""/><br /><sub><b>Toshihiro Nakamura</b></sub></a><br /><a href="https://github.com/quarkiverse/quarkiverse-doma/commits?author=nakamura-to" title="Code">💻</a> <a href="#maintenance-nakamura-to" title="Maintenance">🚧</a></td>
  </tr>
</table>

<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!