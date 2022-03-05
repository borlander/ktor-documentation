[//]: # (title: Logging)

<tldr>
<p>
<b>Required dependencies</b>: <code>io.ktor:ktor-client-logging</code>
</p>
<var name="example_name" value="client-logging"/>
<include from="lib.topic" element-id="download_example"/>
</tldr>

Ktor client provides the capability to log HTTP calls using the `Logging` plugin.
This plugin provides different logger types for different platforms:
- On [JVM](http-client_engines.md#jvm), Ktor uses [SLF4J API](http://www.slf4j.org/) as a facade for various logging frameworks (for example, [Logback](https://logback.qos.ch/) or [Log4j](https://logging.apache.org/log4j)).
- For [Native targets](http-client_engines.md#native), the `Logging` plugin provides a logger that prints everything to `STDOUT`.


## Add dependencies {id="add_dependencies"}
To enable logging, you need to include the following artifacts in the build script:
* (Optional) An artifact with the required SLF4J implementation, for example, [Logback](https://logback.qos.ch/):
  <var name="group_id" value="ch.qos.logback"/>
  <var name="artifact_name" value="logback-classic"/>
  <var name="version" value="logback_version"/>
  <include from="lib.topic" element-id="add_artifact"/>
  
* The `ktor-client-logging` artifact:
  <var name="artifact_name" value="ktor-client-logging"/>
  <include from="lib.topic" element-id="add_ktor_artifact"/>
  

## Install Logging {id="install_plugin"}
To install `Logging`, pass it to the `install` function inside a [client configuration block](create-client.md#configure-client):
```kotlin
val client = HttpClient(CIO) {
    install(Logging)
}
```

## Configure Logging {id="configure_plugin"}

The example below shows how to configure the `Logging` plugin:
- The `logger` property is set to `Logger.DEFAULT`, which uses an SLF4J logging framework. For Native targets, set this property to `Logger.SIMPLE`.
- The `level` property specifies the logging level.

```kotlin
```
{src="/snippets/client-logging/src/main/kotlin/com/example/Application.kt"}
