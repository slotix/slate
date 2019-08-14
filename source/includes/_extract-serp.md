# Extract SERPs

SERPs endpoint <code>/serp</code> crawls search engine result pages (SERPs) and extracts a list of organic results, ads, news, images and more. Specify advanced configuration parameters such as country or language to customize SERP data.

Extracted data is returned in JSON format.

The following search engines are supported:

| | | |
|-|-|-|
| google     | google_news     |  google_image |
| bing       | bing_news       | baidu        |
| duckduckgo | duckduckgo_news | youtube       |
| infospace | webcrawler | 

<aside class="notice">
When requesting <code>/serp</code> endpoint a new Task object will be created. TaskID value is returned as a response. Once a new Task is created, you manipulate directly the Task object with returned TaskID. Refer to <a href="#tasks-amp-processes">Tasks</a> section for more details. 
</aside>

## Search parameters

>Create an SERP Extractor Task.

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/serp/create?name=TASK_NAME&api_key=YOUR_API_KEY \
     -d '{
          "search_engine": "google",
          "keywords": [
            "dafaflow kit",
            "extract SERP"
          ],
          "num_pages": 3,
          "region": "us"
}'
```

|Parameter|Description|
|-|-|
|search_engine| Specify a search engine to use. Valid values are <code>google</code>, <code>google_news</code>, <code>google_image</code>, <code>bing</code>,<code>bing_news</code>, <code>duckduckgo</code>, <code>duckduckgo_news</code>,<code>youtube</code>, <code>baidu</code>, <code>infospace</code>, <code>webcrawler</code>|
|keywords| Parameter defines array of search terms.  You can use anything that you would use in a regular Search engines search. (e.g. for Google, <code>link:dataflowkit.com</code>, <code>site:twitter.com Bratislava</code>, <code>inurl:view/view.shtml</code>, etc.)|
|num_pages|the number of pages to scrape for each keyword|
|region|Specify region value to send requests from. Available values are: 'us'(United States), 'de'(Germany),  'uk'(United Kingdom), 'fr'(France). More regions will be available soon.|

## Google. Search parameters

```json
{
  "search_engine: "google",
  "keywords": ["dafaflow kit", "extract SERP"],
  "num_pages": 1,
  "google_settings": {
    "google_domain": "google.com", // the google domain used for extracting SERPs
    "gl": "us", // Geolocation. Specify country code for google search.
    "hl": "en", // Host Language of user interface. Specify interface language for google search results.
    "start": 0, // specify the results offset to use, defaults to 0.
    "num": 10, // specify the number of results per page to return, defaults to 10. Maximum is 100.
   }
}
```

You can specify additional search parameters for Google SE with the <code>google_settings</code> key.

|Parameter|Description|
|-|-|
|google_domain| Parameter defines the Google domain to use. It defaults to <code>google.com</code>. Head to the Google domains for a full list of supported [Google domains](#google-domains). |
|gl|Use gl=country parameter if you'd like to get country specific search results. (e.g. <code>gl=us</code> for the United States, <code>gl=sk</code> for Slovakia, <code>gl=de</code> for Germany, etc. See the List of available [Country codes](#country-codes)|
|hl|Parameter defines the language to use for the Google search. It's a two-letter language code. (e.g., <code>hl=en</code> for English, <code>hl=es</code> for Spanish, or <code>hl=fr</code> for French) Head to the Google languages for a full list of supported [Google languages](#interface-languages).|
|start|Parameter defines the result offset. It skips the given number of results. It's used for pagination. (e.g., 0 (default) is the first page of results, 10 is the 2nd page of results, 20 is the 3rd page of results, etc.)|
|num|Parameter defines the maximum number of results to return. (e.g., 10 (default) returns 10 results, and 100 returns 100 results).|

## Results

It returns JSON file containing results from Google, Bing, Baidu and etc. with other meta information about serached topics.

<aside class="success">
<a href="#tasks-amp-processes">Tasks and processes</a> Section will guide you through the steps from specifying parameters for SERP scraping to downloding output results.
</aside>