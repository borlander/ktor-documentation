[//]: # (title: DoubleReceive)

<var name="plugin_name" value="DoubleReceive"/>
<var name="artifact_name" value="ktor-server-double-receive"/>

<tldr>
<p>
<b>Required dependencies</b>: <code>io.ktor:%artifact_name%</code>
</p>
<var name="example_name" value="double-receive"/>
<include from="lib.topic" element-id="download_example"/>
</tldr>

The `%plugin_name%` plugin provides the ability to [receive a request body](requests.md#body_contents) several times with no `RequestAlreadyConsumedException` exception.
This might be useful if a [plugin](Plugins.md) is already consumed a request body, so you cannot receive it inside a route handler.
For example, you can use `%plugin_name%` to log a request body using the [CallLogging](call-logging.md) plugin and then receive a body one more time inside the `post` [route handler](Routing_in_Ktor.md#define_route).

> The `%plugin_name%` plugin uses an experimental API that is expected to evolve in the upcoming updates with potentially breaking changes.
>
{type="note"}

## Add dependencies {id="add_dependencies"}

<include from="lib.topic" element-id="add_ktor_artifact_intro"/>
<include from="lib.topic" element-id="add_ktor_artifact"/>

## Install %plugin_name% {id="install_plugin"}

<include from="lib.topic" element-id="install_plugin"/>

After that, you can [receive a request body](requests.md#body_contents) several times and every invocation returns the same instance.
For example, you can enable logging of a request body using the [CallLogging](call-logging.md) plugin...

```kotlin
```
{src="snippets/double-receive/src/main/kotlin/com/example/Application.kt" lines="18-25"}

... and then get a request body one more time inside a route handler.

```kotlin
```
{src="snippets/double-receive/src/main/kotlin/com/example/Application.kt" lines="27-30"}

You can find the full example here: [double-receive](https://github.com/ktorio/ktor-documentation/tree/%current-branch%/codeSnippets/snippets/double-receive).


## Configure %plugin_name% {id="configure"}
With the default configuration, `%plugin_name%` provides the ability to [receive a request body](requests.md#body_contents) as the following types:

- `ByteArray` 
- `String`
- `Parameters` 
- [data classes](serialization.md#create_data_class) used by the `ContentNegotiation` plugin

By default, `%plugin_name%` doesn't support:

- receiving different types from the same request
- receiving a [stream or channel](requests.md#raw)

To overcome these limitations, set the `cacheRawRequest` property to `false`:

```kotlin
```
{src="snippets/double-receive/src/main/kotlin/com/example/Application.kt" lines="15-17"}
