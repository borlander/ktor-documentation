[//]: # (title: Making requests)

<link-summary>
Learn how to make requests and specify various request parameters: a request URL, an HTTP method, headers, and the body of a request.
</link-summary>

After [setting up the client](create-client.md), you can make HTTP requests. The main way for making HTTP requests is the [request](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request/request.html) function that can take a URL as a parameter. Inside this function, you can configure various request parameters: 
* Specify a request URL.
* Specify an HTTP method, such as `GET`, `POST`, `PUT`, `DELETE`, `HEAD`, `OPTION`, or `PATCH`.
* Add headers and cookies.
* Set the body of a request, for example, a plain text, a data object, or form parameters.

These parameters are exposed by the [HttpRequestBuilder](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request/-http-request-builder/index.html) class.

```kotlin
```
{src="snippets/_misc_client/RequestMethodWithoutParams.kt" interpolate-variables="true" disable-links="false"}

Note that this function allows you to receive a response as an `HttpResponse` object. `HttpResponse` exposes API required to get a response body in various ways (a string, a JSON object, etc.) and obtain response parameters, such as a status code, content type, headers, and so on. You can learn more from the [](response.md) topic.

> `request` is a suspending function, so requests should be executed only from a coroutine or another suspend function. You can learn more about calling suspending functions from [Coroutines basics](https://kotlinlang.org/docs/coroutines-basics.html).

## Specify a request URL {id="url"}

### Specify URL string {id="url-string"}

The `request` function can take a URL as a parameter:

```kotlin
```
{src="snippets/_misc_client/RequestMethodWithoutParams.kt" lines="4-6" interpolate-variables="true" disable-links="false"}

### Configure URL components separately {id="url-components"}

Another way to specify the URL is the `url` parameter exposed by `HttpRequestBuilder`. This parameter accepts [URLBuilder](https://api.ktor.io/ktor-http/io.ktor.http/-u-r-l-builder/index.html) and allows you to specify various URL components separately:

```kotlin
val response: HttpResponse = client.request {
    url {
        protocol = URLProtocol.HTTPS
        host = "ktor.io"
    }
}
```

> To configure a base URL for all requests, you can use the [DefaultRequest](default-request.md#url) plugin. 


## Set request parameters {id="parameters"}
In this section, we'll see on how to specify various request parameters, including an HTTP method, headers, and cookies. If you need to configure some default parameters for all requests of a specific client, use the [DefaultRequest](default-request.md) plugin.


### HTTP method {id="http-method"}

When calling the `request` function, you can specify the desired HTTP method using the `method` property:

```kotlin
```
{src="snippets/_misc_client/RequestMethodWithParams.kt"}

In addition to the `request` function, `HttpClient` provides specific functions for basic HTTP methods: [get](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request/get.html), [post](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request/post.html), [put](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request/put.html), and so on. For example, you can replace the example above with the following code:
```kotlin
```
{src="snippets/_misc_client/GetMethodWithoutParams.kt"}

### Headers {id="headers"}

To add headers to the request, you can use the following ways:
- The [headers](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request/headers.html) function allows you to add several headers at once:
   ```kotlin
   ```
   {src="snippets/_misc_client/GetMethodWithHeaders.kt"}
- The [header](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request/header.html) function allows you to append a single header.
- The `basicAuth` and `bearerAuth` functions add the `Authorization` header with a corresponding HTTP scheme.
   > For advanced authentication configuration, refer to [](auth.md).



### Cookies {id="cookies"}
To send cookies, use the [cookie](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request/cookie.html) function:

```kotlin
```
{src="snippets/_misc_client/GetMethodWithCookies.kt"}

Ktor also provides the [HttpCookies](http-cookies.md) plugin that allows you to keep cookies between calls. If this plugin is installed, cookies added using the `cookie` function are ignored.


### Query parameters {id="query_parameters"}

To add <emphasis tooltip="query_string">query string</emphasis> parameters, call the [parameter](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request/parameter.html) function:

```kotlin
```
{src="snippets/_misc_client/GetMethodWithQueryParam.kt"}

Note that the `value` parameter accepts an `Any` instance. 
So, if you need to specify several values for a single key, you can pass these values as a `List` object.


## Set request body {id="body"}
To set the body of a request, you need to call the `setBody` function exposed by [HttpRequestBuilder](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request/-http-request-builder/index.html). This function accepts different types of payloads, including plain text, arbitrary class instances, form data, byte arrays, and so on. Below we'll take a look at several examples.

### Text {id="text"}
Sending plain text as body can be implemented in the following way:
```kotlin
```
{src="snippets/_misc_client/PostMethodWithBody.kt"}


### Objects {id="objects"}
With the enabled [ContentNegotiation](serialization-client.md) plugin, you can send a class instance within a request body as JSON. To do this, pass a class instance to the `setBody` function and set the content type to `application/json` using the [contentType](https://api.ktor.io/ktor-http/io.ktor.http/content-type.html) function:

```kotlin
```
{src="snippets/client-json-kotlinx/src/main/kotlin/com/example/Application.kt" lines="29-32"}

You can learn more from the [](serialization-client.md) help section.

### Form parameters {id="form_parameters"}
The Ktor client provides the [submitForm](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request.forms/submit-form.html) function for sending form parameters using both `x-www-form-urlencoded` and `multipart/form-data` types. The example below shows how to send form parameters encoded as `multipart/form-data`:
* `url` specifies a URL for making a request.
* `formParameters` a set of form parameters built using `Parameters.build`.

```kotlin
```
{src="snippets/client-submit-form/src/main/kotlin/com/example/Application.kt" lines="12-22"}

You can find the full example here: [client-submit-form](https://github.com/ktorio/ktor-documentation/tree/%current-branch%/codeSnippets/snippets/client-submit-form).

> To send form parameters encoded in URL, set `encodeInQuery` to `true`.


### Upload a file {id="upload_file"}

If you need to send a file with a form, you can use the following approaches:

- Use the [submitFormWithBinaryData](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request.forms/submit-form-with-binary-data.html) function. In this case, a boundary will be generated automatically.
- Call the `post` function and pass the [MultiPartFormDataContent](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request.forms/-multi-part-form-data-content/index.html) instance to the `setBody` function. Note that the `MultiPartFormDataContent` constructor also allows you to pass a boundary value.

For both approaches, you need to build form data using the [formData](https://api.ktor.io/ktor-client/ktor-client-core/io.ktor.client.request.forms/form-data.html) function.

<tabs>

<tab title="submitFormWithBinaryData">

```kotlin
```
{src="snippets/client-upload/src/main/kotlin/com/example/Application.kt" lines="13-24"}

</tab>

<tab title="MultiPartFormDataContent">

```kotlin
```
{src="snippets/client-upload-progress/src/main/kotlin/com/example/Application.kt" lines="16-33"}

</tab>

</tabs>

`MultiPartFormDataContent` also allows you to override a boundary and content type as follows:

```kotlin
```
{src="snippets/client-upload-progress/src/main/kotlin/com/example/Application.kt" lines="39-43"}

You can find the full examples here:
- [client-upload](https://github.com/ktorio/ktor-documentation/tree/%current-branch%/codeSnippets/snippets/client-upload)
- [client-upload-progress](https://github.com/ktorio/ktor-documentation/tree/%current-branch%/codeSnippets/snippets/client-upload-progress)


## Parallel requests {id="parallel_requests"}

When sending two requests at once, the client suspends the second request execution until the first one is finished. If you need to perform several requests at once, you can use [launch](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/launch.html) or [async](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/async.html) functions. The code snippet below shows how to perform two requests asynchronously:
```kotlin
```
{src="snippets/client-parallel-requests/src/main/kotlin/com/example/Application.kt" lines="12,19-23,28"}

To see a full example, go to [client-parallel-requests](https://github.com/ktorio/ktor-documentation/tree/%current-branch%/codeSnippets/snippets/client-parallel-requests).


## Cancel a request {id="cancel-request"}

If you need to cancel a request, you can cancel a coroutine that runs this request. The [launch](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/launch.html) function returns a `Job` that can be used to cancel the running coroutine:
```kotlin
```
{src="snippets/_misc_client/CancelRequest.kt"}

Learn more about [Cancellation and timeouts](https://kotlinlang.org/docs/cancellation-and-timeouts.html).
