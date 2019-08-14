# Single Processes

<strong>Single process</strong> is intended for performing simple jobs like fetching an html, make a screenshot or convert a file. It is similar to a [Task](#tasks-amp-processes). But the general difference is that a Single Process can be run only once and returns result immediately after finishing. 


Examples of Single process types are listed here:

- [Fetch HTML pages](#fetch-html)  
- [Make a screenshot](#take-a-screenshot)
- [Convert files to PDF](#convert-files-to-pdf)


## Fetch HTML

Fetch endpoint is used for web pages download. Regular pages are fetched "as is" using standard http requests. But real headless web browser is used for rendering dynamic Javascript driven web pages.

### Parameters

Parameter | Description
--------- | -----------
type | If set to <code>"base"</code>, Base fetcher is used for downloading web page content. Use <code>"chrome"</code> for fetching content with headless chrome browser. 
url | Specify url to download.
proxy | Specify custom proxy as http://1.2.3.4:3128


### Fetch Response
Fetch returns utf8 encoded web page content. 

### Base Fetcher

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/fetch?api_key=YOUR_API_KEY -d \
'{
  "type":"base",
  "url":"http://google.com",
  "proxy": "http://0.0.0.0:55555"
}'
```

Base fetcher uses standard http requests to download regular pages. It works faster than Chrome fetcher. 

### Chrome Fetcher

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/fetch?api_key=YOUR_API_KEY -d \
'{
  "type":"chrome",
  "url":"http://google.com",
}'
```

Chrome fetcher is intended for rendering dynamic Javascript based content. It sends requests to Chrome running in headless mode.



## Take a Screenshot

>Create a PNG Screenshot from URL

```shell 
curl --request POST \
    --url https://api.dataflowkit.com/v1/screenshot?api_key=YOUR_API_KEY \
    --header 'Content-Type: multipart/form-data' \
    --form remoteURL=https://dataflowkit.com \
    --form format=png \
    --form qality=80 \
    --form fullPage=false \
    --form xoffset=0 \
    --form yoffset=0 \
    --form width=800 \
    --form height=600 \
    --form scale=1 \
    -o result.png
```

Dataflow Kit Screenshot endpoint is intended for taking screenshots from web pages. 

It returns the link to png or jpeg captured screenshot. 

It accepts POST requests with a multipart/form-data Content-Type.

Parameter | Default | Description 
---------- | ----- | ----- 
remoteURL | - |Remote URL to take a screenshot from
format | png | Sets the Format of output image. Values: png, jpeg 
qality | 80 | Sets the Quality of output image. Compression quality from range \[0..100\] (jpeg only).
fullPage | false | takes a screenshot of a full web page. It ignores xoffset, yoffset, width and height argument values.
xoffset | 0 | X offset in device independent pixels (dip).
yoffset | 0 | Y offset in device independent pixels (dip).
width | 800 | Rectangle width in device independent pixels (dip). 
height | 600 | Rectangle height in device independent pixels (dip).
scale | 1 | Page scale factor. defaults to 1


## Convert files to PDF

>Create an Converter Task specifying payload configuration

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/convert/{from}/pdf?api_key=YOUR_API_KEY \
     -d '{JSON Conversion Payload}'
```

<code>{from}</code> parameter has one of the following values: 

- url
- html
- markdown
- office


If successful, returns the link to resulted data in PDF format.

The error is returned otherwise.

<aside class="notice">Refer to <a href="#convert-to-pdf">Convert to PDF</a> section describing possible parameters for Web data extraction tasks.</aside>