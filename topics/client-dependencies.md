[//]: # (title: Adding client dependencies)

<link-summary>Learn how to add client dependencies to the existing project.</link-summary>

To use the Ktor HTTP client in your project, you need to add at least two dependencies: a client dependency and an [engine](http-client_engines.md) dependency. If you want to extend the client functionality with specific [plugins](http-client_plugins.md), you also need to add the appropriate dependencies.

## Client dependency {id="client-dependency"}
The main client functionality is available in the `ktor-client-core` artifact. Depending on your build system, you can add it in the following way:
<var name="artifact_name" value="ktor-client-core"/>
<include from="lib.topic" element-id="add_ktor_artifact"/>


## Engine dependency {id="engine-dependency"}
An [engine](http-client_engines.md) is responsible for processing network requests. There are different client engines available for various platforms, such as Apache, CIO, Android, iOS, and so on. For example, you can add a `CIO` engine dependency as follows:
<var name="artifact_name" value="ktor-client-cio"/>
<include from="lib.topic" element-id="add_ktor_artifact"/>

For a full list of dependencies required for a specific engine, see [](http-client_engines.md#dependencies).

> To learn how to add engine dependencies in a multiplatform mobile project, see [](getting_started_ktor_client_multiplatform_mobile.md#ktor-dependencies).

## Plugin dependency {id="plugin-dependency"}
Ktor lets you use additional client functionality ([plugins](http-client_plugins.md)) that is not available by default, for example, logging, authorization, or serialization. Most of them are provided in separate artifacts. You can learn which dependencies you need from a topic for a required plugin.