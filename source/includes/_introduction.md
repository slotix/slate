# Introduction

Welcome to the Dataflow Kit (DFK) API! 

DFK’s API enables you to programatically manage and run your web data extraction and conversion tasks, and retrieve extracted data.

Curl, Go, Python, Node.js, and PHP code examples are available. You can view them in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right. By default, curl is selected so that you can try out the commands in your terminal.

## Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass a valid API Key with each request
curl --request POST \
     --url https://api.dataflowkit.com/v1/{API-ENDPOINT}?api_key={YOUR_API_KEY} -d \
'{
  "foo":"bar"
}'
```

>API-ENDPOINT corresponds to specified API endpoint. 
>Make sure to replace `{YOUR_API_KEY}` with your API key.

Dataflow Kit API needs to be authenticated by passing a secret API Key to the request URL as the <code>api_key</code> query parameter. The API Key can be found in the Control Panel at [developer portal](https://app.dataflowkit.com). 

Dataflow Kit expects for the API key to be included in all API requests to the server as query parameter that looks like the following: 

`api_key=YOUR_API_KEY`

<aside class="success">
You must replace <code>YOUR_API_KEY</code> with your personal API key.
</aside>

<aside class="warning">
IMPORTANT: Do not share the API Key with untrusted parties, or use it directly from a client-side code, unless you fully understand the consequences!
</aside>

## Backwards Compatibility

The Dataflow Kit API may be changed in a backwards-compatible way at any time. Backwards compatibility means that new methods, objects, statuses, fields in responses, etc. may be added at any time, but existing ones **will never be renamed or removed**.

If there are backward incompatible changes that need to be made to our API, we will release a new API version. The previous API version will be maintained for at least a year after releasing the new version.

The current API version 1 is both available via the /v1 prefix or no prefix at all. For clarity we recommend using the API endpoints including the /v1 prefix. The upcoming API version 2 will use the /v2 prefix.