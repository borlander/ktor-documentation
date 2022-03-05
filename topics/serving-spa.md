[//]: # (title: Serving single-page applications)

<var name="artifact_name" value="ktor-server-single-page"/>
<var name="plugin_name" value="SinglePageApplication"/>

<tldr>
<p>
<b>Required dependencies</b>: <code>io.ktor:%artifact_name%</code>
</p>
<var name="example_name" value="single-page-application"/>
<include from="lib.topic" element-id="download_example"/>
</tldr>

<link-summary>
Ktor provides the ability to serve single-page applications, including React, Angular, Vue, and so on.
</link-summary>

Ktor provides the ability to serve single-page applications, including React, Angular, or Vue, using the `%plugin_name%` plugin.


## Add dependencies {id="add_dependencies"}

<include from="lib.topic" element-id="add_ktor_artifact_intro"/>
<include from="lib.topic" element-id="add_ktor_artifact"/>

## Install %plugin_name% {id="install_plugin"}

<include from="lib.topic" element-id="install_plugin"/>


## Configure %plugin_name% {id="configure"}

`%plugin_name%` allows you to define where you want the content to be served from: a local filesystem or the classpath.
You need at least to specify a folder/resource package containing a single-page application.

### Serve framework-specific applications {id="serve-framework"}

`%plugin_name%` allows you to serve a build of your single-page application created using a specific framework, such as React, Angular, Vue, and so on. 
Suppose we have the `react-app` folder in a project root containing a React application.
The application has the following structure and the `index.html` file as the main page:

```text
react-app
├── index.html
├── ...
└── static
    └── ...
```

To serve this application, pass the folder name to the `react` function inside the plugin configuration block:

```kotlin
install(SinglePageApplication) {
    react("react-app")
}
```

`%plugin_name%` looks up `index.html` automatically. 
To learn how to customize a default page, see [](#serve-customize).

> For other frameworks, use corresponding functions, such as `angular`, `vue`, `ember`, and so on.



### Customize serving settings {id="serve-customize"}

To demonstrate how to serve a single-page application from resources, let's suppose our application is placed inside the `sample-web-app` resource package, which has the following structure:

```text
sample-web-app
├── main.html
├── ktor_logo.png
├── css
│   └──styles.css
└── js
    └── script.js
```

To serve this application, the following configuration is used:

```kotlin
```
{src="snippets/single-page-application/src/main/kotlin/com/example/Application.kt" lines="7-11,13"}

- `useResources`: Enables serving an application from a resource package.
- `filesPath`: Specifies the path under which an application is located.
- `defaultPage`: Specifies `main.html` as a default resource to serve.
- `ignoreFiles`: Ignores paths that contain `.txt` at the end.

You can find the full example here: [single-page-application](https://github.com/ktorio/ktor-documentation/tree/%current-branch%/codeSnippets/snippets/single-page-application).
