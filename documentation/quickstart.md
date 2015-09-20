---
layout: documentation
title: Quickstart
---

To get started using ReactivePI, you need to configure your build to include
the ReactivePI dependency. On this page you will find everything you need to
get the dependency in your project for well known buildsystems.

## Gradle
You can use the ReactivePI library from gradle with just a few lines of configuration.
Just add the JCenter repository to the build file and add the dependencies you want.

``` python
apply plugin: 'java'

repositories {
  jcenter()
}

dependencies {
  compile 'reactivepi:core_2.11:0.1'
}
```

## Maven
Using ReactivePI from Maven is slightly more complicated than using it from
Gradle. Maven uses Maven Central as the default repository to get its dependencies.
ReactivePI itself though is published in the JCenter repository.

So in order to use ReactivePI, you need to add a settings.xml file to your
project directory with the following content:

``` xml
<?xml version='1.0' encoding='UTF-8'?>
<settings xsi:schemaLocation='http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd' xmlns='http://maven.apache.org/SETTINGS/1.0.0' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
<profiles>
	<profile>
		<repositories>
			<repository>
				<snapshots>
					<enabled>false</enabled>
				</snapshots>
				<id>central</id>
				<name>bintray</name>
				<url>http://jcenter.bintray.com</url>
			</repository>
		</repositories>
		<pluginRepositories>
			<pluginRepository>
				<snapshots>
					<enabled>false</enabled>
				</snapshots>
				<id>central</id>
				<name>bintray-plugins</name>
				<url>http://jcenter.bintray.com</url>
			</pluginRepository>
		</pluginRepositories>
		<id>bintray</id>
	</profile>
</profiles>
<activeProfiles>
	<activeProfile>bintray</activeProfile>
</activeProfiles>
</settings>
```

**Important** Maven doesn't pickup the settings automatically, you need to
execute maven commands with the -s settings.xml option to use ReactivePI.

After that you can add the reactivePI dependencies which look like this
for the core library:

``` xml
<dependency>
  <groupId>reactivepi</groupId>
  <artifactId>core_2.11</artifactId>
  <version>0.1</version>
</dependency>
```

For the actors library use the following dependency:

``` xml
<dependency>
  <groupId>reactivepi</groupId>
  <artifactId>actors_2.11</artifactId>
  <version>0.1</version>
</dependency>
```

## SBT
To use ReactivePI in a SBT-based build you need to add
the jcenter repository to the build file
and set the library as a dependency.

``` scala
scalaVersion := "2.11.7"

resolvers += Resolver.jcenterRepo

libraryDependencies ++= {
  Seq(
    "reactivepi" %% "core" % "0.1",
    "reactivepi" %% "actors" % "0.1"
  )
}
```
