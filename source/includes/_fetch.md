# Fetch

Fetch endpoint is used for web pages download. Regular pages are fetched "as is" using standard http requests. But real web browser is required for rendering dynamic Javascript driven web pages.

## Parameters

Parameter | Description
--------- | -----------
type | If set to "base", Base fetcher is used for downloading web page content. Use "chrome" for fetching content with headless chrome browser. 
url | Specify url to download.
proxy | 

## Fetch Response

Fetch returns utf8 encoded web page content. 


## Base Fetcher

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/fetch?api_key={YOUR_API_KEY} -d \
'{
  "type":"base",
  "url":"http://google.com",
  "proxy: "1.2.3.4:3128"
}'
```

Base fetcher uses standard http requests to download regular pages. It works faster than Chrome fetcher. 

## Chrome Fetcher

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/fetch?api_key={YOUR_API_KEY} -d \
'{
  "type":"chrome",
  "url":"http://google.com",
  "proxy: "1.2.3.4:3128"
}'
```

Chrome fetcher is intended for rendering dynamic Javascript based content. It sends requests to Chrome running in headless mode.