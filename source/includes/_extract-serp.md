# Extract SERPs

To crawl search engine result pages (SERPs) you can run either single process or create a task. SERPs collection service extracts a list of organic results, news, images and more. Specify advanced configuration parameters such as country or languages to customize output SERP data.

The following search engines are supported:

| | |
|-|-|
| Google Web | Google Images |
| Google News | Google Shopping |
| Bing | DuckDuckGo |
| Baidu | Yandex |


<aside class="notice">
To run SERPs service as a single process send a request with corresponding payload to <code>/serp</code> endpoint.
Learn more info about <a href="#single-processes">Single processes</a>.
</aside>
 <aside class="notice">
To create SERPs extraction task follow the steps described in <a href="#tasks-amp-processes">Tasks</a> section. 
</aside>


## Search parameters

>Create an SERP Extractor Task.

```shell
curl --request POST \
        --url https://api.dataflowkit.com/v1/extract?api_key=YOUR_API_KEY \
        -H 'Content-Type: application/json' \
        -d '{
    "name": "google",
    "request": {
        "url": "https://www.google.com/search?q=dataflow+kit&lr=lang_de&gl=at",
        "proxy": "country-at",
        "type": "chrome"
    },
    "fields": [
        {
            "name": "selector1",
            "selector": ".r>a:first-of-type",
            "attrs": [
                "href",
                "text"
            ],
            "type": 2,
            "filters": [
                {
                    "name": "trim"
                }
            ]
        }
    ],
    "paginator": {
        "nextPageSelector": ".b.navend:last-child a",
        "pageNum": 3
    },
    "format": "csv"
}'
```  

<aside class="success">Depending on the SERP extraction task requirements different sets of higly customizable parameters may be passed. In the most cases you don't need to tune them manually. Just use <a href="https://dataflowkit.com/serp" target="_blank">SERP extraction Code generator</a>. It is the easiest way to generate a payload to be run in Dataflow kit cloud.</aside>

|Parameter|Description|Notes|
|-|-|-|
|name	| Collection name |	required |
|url|url holds the link to a Search Engine to use, and other optional parameters like languages or country.| required. See URL GET parameters description below.|

#### URL GET parameters
||||
|-|-|-|
|q| Parameter defines encoded search term. You can use anything that you would use in a regular Search engines search. (e.g. for Google, <code>link:dataflowkit.com</code>, <code>site:twitter.com Bratislava</code>, <code>inurl:view/view.shtml</code>, etc.) See The Complete List of 42 Advanced <a href="https://ahrefs.com/blog/google-advanced-search-operators/" target="_blank">Google Search Operators</a>|<code>q</code> parameter is used by google, Bing, DuckDuckGo. <code>text</code> is used as query holder by Yandex SE. Chineese Baidu uses <code>wd</code> for this purpose.|
|tbm| <code>tbm</code> is a special Google parameter used to differentiate between search types| - <code>tbm=isch</code> - Google Images, - <code>tbm=nws</code> - Google News,  - <code>tbm=shop</code> - Google Shopping|  
|lr|Restricts the search to documents written in a particular languages.|Google uses <code>lang_{two-letter lang code}</code> to specify languages and <code>&#124;</code> as a delimiter. (e.g., lang_sk&#124;lang_de will only search Slovak and German pages). See the <a href="https://developers.google.com/custom-search/v1/cse/list">full list</a> of possible values for Google. For Bing specify <code>setLang=en</code> parameter. In Yandex use <code>lang=ca</code> parameter|
|gl|Specify the country to search from. It's a two-letter country code. (e.g., <code>sk</code> for Slovakia, or <code>us</code> for the United States).| For Google see the <a href="https://developers.google.com/custom-search/docs/xml_results_appendices#countryCodes">Country Codes</a> page for a list of valid values. For Bing <code>cc=at</code> parameter is used.|

|Parameter|Description|Notes|
|----|-|-------|
|proxy|Select country to locate proxy to pass requests through to target web sites. **NOTE: You Always have to use proxy when requesting SERPs** | Use <code>country-{two-letter lang code}</code> to locate proxy in specified country or <code>country-any</code> for random proxy. (e.g., <code>country-us</code> will pass all requests to US proxy; <code>country-any</code> will pass proxified requests to random country;). |
|fields | Set of definite CSS selectors (patterns) used to gather data from Search Engine Result Pages.| Payloads for collecting search results (SERP data) from the most popular Search Engines are available.  These payloads are fully customizable. |
|pageNum|Specify number of pages to crawl. |Defaults to 1|
|format|Select format of output data.|Possible Values are CSV, JSON(L), XML|


## Results

Extracted data is returned in CSV, JSON, JSON(Lines) or XML format.