# Single Processes

<strong>Single process</strong> is intended for performing simple jobs like rendering/ fetching  html, capturing a screenshot or print web page to PDF. It is similar to a [Task](#tasks-amp-processes). But the general difference is that a Single Process can be run only once and returns result immediately after finishing. 


Examples of Single process types are listed here:

- [Fetch/ Render HTML pages](#fetch-html)  
- [Capture screenshots](#take-a-screenshot)
- [Print URLs to PDF](#convert-files-to-pdf)


## Fetch HTML

>Base Fetcher

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/fetch?api_key=YOUR_API_KEY -d \
'{
  "type":"base",
  "url":"https://anysite.com",
  "proxy": "country-any"
}'
```

>Chrome Fetcher

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/fetch?api_key=YOUR_API_KEY -d \
'{
  "type":"chrome",
  "url":"http://google.com",
  "proxy":"country-any",
  "waitDelay":0.5,
  "actions": [
        {
            "input": {
                "inputSelector": "#search-box",
                "inputText": "Search Term"
            }
        },
        {
            "click": {
                "clickSelector": "#button"
            }
        },
        {
            "waitFor": {
                "waitForSelector": ":root"
            }
        },
        {
            "scroll": {
                "paginate": "10"
            }
        }
    ]
}'
```

Fetch endpoint is used for web pages download. Regular pages are fetched "as is" using standard http requests. But real headless chrome web browser is used for rendering dynamic Javascript driven web pages.

### Base Fetcher

Base fetcher uses standard http requests to download regular pages. It works faster than Chrome fetcher. 

### Chrome Fetcher

Chrome fetcher is intended for rendering dynamic Javascript based content. It sends requests to Chrome running in headless mode.

<aside class="notice">Read more information about Fetcher types at <a href="https://dataflowkit.com/render-web#fetchers" target="_blank">https://dataflowkit.com/render-web#fetchers</a></aside>

### Parameters

Parameter | Description
--------- | -----------
type | If set to <code>"base"</code>, Base fetcher is used for downloading web page content. Use <code>"chrome"</code> for fetching content with headless chrome browser. 
url | Specify url to download.
proxy | Specify proxy like country-sk
waitDelay | Specify a custom delay (in seconds). This may be useful if certain elements of the web site need to be rendered after initial page load. (Chrome fetcher type only) 
actions | Use actions to automate manual workflows while rendering web pages. They simulate real-world human interaction with pages. (Chrome fetcher type only) 

### Fetch Response
Fetch returns utf8 encoded web page content. 


<aside class="notice">Use <a href="https://dataflowkit.com/render-web" target="_blank">visual payload generator </a> to create Ready-to-run code for the most popular programming languages. </aside>


## Capture a Screenshot

>Create a PNG Screenshot from URL

```shell 
curl --request POST \
    --url https://api.dataflowkit.com/v1/convert/url/screenshot?api_key=  \
    -H "Content-Type: application/json" \
    -d '{
    "url": "https://dataflowkit.com",
    "proxy": "country-au",
    "width": 1920,
    "height": 1080,
    "offsetx": 50,
    "offsety": 50,
    "scale": 1,
    "format": "jpeg",
    "quality": 90,
    "waitDelay": 0.5,
    "actions":[]
}'
```

Dataflow Kit Screenshot endpoint is intended for taking screenshots from web pages. 

It returns the png/ jpeg captured screenshot download link. 


Parameter | Default | Description 
---------- | ----- | ----- 
url | - |Remote web page URL to take a screenshot from
format | png | Sets the Format of output image. Values: png, jpeg 
quality | 80 | Sets the Quality of output image. Compression quality from range \[0..100\] (jpeg only).
fullPage | false | takes a screenshot of a full web page. It ignores offsetX, offsety, width and height argument values.
clipSelector | - | captures a screenshot of specified HTML element. For example, pass CSS selector like "#clipped-element" as an Value.
offsetx | 0 | X offset in device independent pixels (dip).
offsety | 0 | Y offset in device independent pixels (dip).
width | 800 | Rectangle width in device independent pixels (dip). 
height | 600 | Rectangle height in device independent pixels (dip).
scale | 1 | Page scale factor. range \[0.1..3\] defaults to 1
waitDelay | - | Specify a custom delay (in seconds) before making of a Screenshot. This may be useful if certain elements of the web site need to be rendered after initial page load. (e.g. CSS animations, JavaScript effects, etc.)

<aside class="notice">The most comfortable way to generate a payload for taking screenshots is to use <a href="https://dataflowkit.com/url-to-screenshot" target="_blank">URL-to-Screenshot code generator.</a></aside>


## Print URL to PDF

>Create an Converter Task specifying payload configuration

```shell
curl --request POST \
        --url https://api.dataflowkit.com/v1/convert/url/pdf?api_key=  \
        -H "Content-Type: application/json" \
        -d '{
          "url": "https://dataflowkit.com",
          "proxy": "country-at",
          "paperSize": "A4",
          "landscape": false,
          "printBackground": false,
          "printHeaderFooter": true,
          "scale": 1,
          "pageRanges": "",
          "marginTop": 0.4,
          "marginLeft": 0.4,
          "marginRight": 0.4,
          "marginBottom": 0.4,
          "waitDelay": 0.5,
          "actions":[]
}'
```


Parameter | Default | Description 
---------- | ----- | ----- 
url | - | The full URL address (including HTTP/HTTPS) of web page that you want to print to PDF	
proxy | - | Select country to locate proxy to pass requests through to target web sites.
orientation | false | Paper orientation. Set landscape = true for portrait orientation.
Page size | "A4" | Page size parameter consists of the most popular page formats. Possible values are: "A3", "A4", "A5", "A6", "Letter", "Legal", "Tabloid"
Print background | false | Print background graphics in the PDF.
Page ranges | - | Specify page ranges to convert, e.g., '1-4, 6, 10-12'. Defaults to the empty value, which means convert all pages.
Scale | 1 |	By default, PDF document content is generated according to the size and dimensions of the original web page content. Using Scale parameter you can specify a custom zoom factor from 0.1 to 5.0 of the webpage rendering.
marginTop | 0.4 inches | Top Margin of the PDF
marginLeft | 0.4 inches | Left Margin of the PDF
marginRight | 0.4 inches | Right Margin of the PDF
marginBottom | 0.4 inches | Bottom Margin of the PDF
Header and Footer | false | Turn the header/footer on or off. They include the date, name of the web page, the page URL and how many pages the document you're printing.
Wait Delay | - | Specify a custom delay (in seconds) before generation of a PDF. This may be useful if certain elements of the web site need to be rendered after initial page load. (e.g. CSS animations, JavaScript effects, etc.)
Actions | - | Actions simulate real-world human interaction with pages. They can be used to automate manual workflows before a PDF conversion is performed.

<aside class="notice">Using <a href="https://dataflowkit.com/url-to-pdf" target="_blank">URL-to-PDF code generator</a> is the easiest way to generate a payload to be run in Dataflow kit cloud. </aside>
