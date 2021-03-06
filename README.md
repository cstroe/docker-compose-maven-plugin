# Docker Compose Maven Plugin

[![Build Status](https://travis-ci.org/dkanejs/docker-compose-maven-plugin.svg?branch=master)](https://travis-ci.org/dkanejs/docker-compose-maven-plugin)
[![Join the chat at https://gitter.im/dkanejs/docker-compose-maven-plugin](https://badges.gitter.im/dkanejs/docker-compose-maven-plugin.svg)](https://gitter.im/dkanejs/docker-compose-maven-plugin?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.dkanejs.maven.plugins/docker-compose-maven-plugin/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.dkanejs.maven.plugins/docker-compose-maven-plugin)

## Quickstart

Available on Maven Central.

```
<dependency>
    <groupId>com.dkanejs.maven.plugins</groupId>
    <artifactId>docker-compose-maven-plugin</artifactId>
    <version>1.0.1</version>
</dependency>
```
## About
Maven plugin for running basic `docker-compose` commands with Maven.

This can be used as part of the Maven lifecycle or as a utility to bring `docker-compose` commands to your Maven toolkit.

## Usage
### Goals
* up - runs `docker-compose up`
* down - runs `docker-compose down`

### Properties
#### composeFile

`composeFile` - Location of the compose file e.g. `${project.basedir}/docker-compose.yml`

The Plugin assumes your docker file is in `${project.basedir}/src/main/resources/docker-compose.yml`

This can be changed in the configuration section of the plugin:

```
<configuration>
    <composeFile>${project.basedir}/docker-compose.yml</composeFile>
</configuration>
```
#### detachedMode

`detachedMode` - Run in detached mode e.g. `docker-compose up -d`

The plugin will not run in detached mode by default.

This can be changed in the configuration section of the plugin:

```
<configuration>
    <detachedMode>true</detachedMode>
</configuration>
```
#### removeVolumes

`removeVolumes` - Delete volumes e.g. `docker-compose down -v`

The plugin will not remove any volumes you create when using the `down` goal.

This can be changed in the configuration section of the plugin:

```
<configuration>
    <removeVolumes>true</removeVolumes>
</configuration>
```

## Configuration
### Default
Below will allow use of the plugin from the `mvn` command line: 

```
<build>
    <plugins>
        <plugin>
            <groupId>com.dkanejs.maven.plugins</groupId>
            <artifactId>docker-compose-maven-plugin</artifactId>
            <version>1.0.1</version>
        </plugin>
    </plugins>
</build>
```
This assumes the compose file is in the default location and will not run in any phase of the build.

### Advanced
Below has customised the location of the `docker-compose.yml` file and has two executions defined:

```
<build>
    <plugins>
        <plugin>
            <groupId>com.dkanejs.maven.plugins</groupId>
            <artifactId>docker-compose-maven-plugin</artifactId>
            <version>1.0.1</version>
            <executions>
                <execution>
                    <id>up</id>
                    <phase>verify</phase>
                    <goals>
                        <goal>up</goal>
                    </goals>
                    <configuration>
                        <composeFile>${project.basedir}/docker-compose.yml</composeFile>
                        <detachedMode>true</detachedMode>
                    </configuration>
                </execution>
                <execution>
                    <id>down</id>
                    <phase>verify</phase>
                    <goals>
                        <goal>down</goal>
                    </goals>
                    <configuration>
                        <composeFile>${project.basedir}/docker-compose.yml</composeFile>
                        <removeVolumes>true</removeVolumes>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

This will run the following as part of the `verify` phase:
 1. `docker-compose up -d` using a `docker-compose.yml` file in a custom location 
 2. `docker-compose down -v` using a `docker-compose.yml` file in a custom location

