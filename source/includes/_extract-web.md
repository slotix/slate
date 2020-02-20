# Extract data from web

<code>/extract</code> endpoint crawls web pages and extracts data like text, links or images following the specified rules. Dataflow kit uses CSS selectors to find HTML elements in web pages and to extract data from. Extracted data is returned in CSV, MS Excel, JSON, JSON(Lines) or XML format.

<aside class="notice"><a href="https://medium.com/hackernoon/json-lines-format-76353b4e588d" target="_blank">Why store data in JSON Lines format?</a> Read our article at HackerNoon.
</aside>

## Collection scheme

>Here is a simple collection object:

```json
'{
    "name":"test.dataflowkit.com",
    "request":{
        "url":"https://test.dataflowkit.com/persons/page-0",
        "type":"chrome",
        "proxy":"country-any"
    },
    "commonParent":".parent",
    "fields":[
        {
            "name":"Number",
            "selector":".badge-primary",
            "attrs":["text"],
            "type":1,
            "filters":[
                {
                    "name":"trim"
                }
            ]
        },
        {
            "name":"Name",
            "selector":"#cards a",
            "attrs":["href","text"],
            "type":2,
            "filters":[
                {
                    "name":"trim"
                }
            ]
        },
        {
            "name":"Picture",
            "selector":".card-img-top",
            "attrs":["src","alt"],
            "type":0,
            "filters":[
                {
                    "name":"trim"
                }
            ]
        }
    ],
    "paginator":{
        "nextPageSelector":".page-link",
        "pageNum":2
        },
    "path":false,
    "format":"JSON"
}'
```

Collection scheme represents settings for data extraction from specified web site. It has the following properties:

Property | Description | Required
--------- | ----------- | -----------
name | Collection name | required
request | Request parameters for downloading html pages. Refer to [Fetch HTML](#fetch-html) section for more details about request parameters  | required
url | url holds the the starting web page address to be downloaded. | required
type | type specifies fetcher type which may be "base" or "chrome" value. If omited "base" fetcher is used by default | optional
commonParent | commonParent specifies common ancestor block for all fields used to extract data from a web page |optional
fields | A set of fields used to extract data from a web page. A Field represents a given chunk of data to be extracted from every block on each page. Read more about [field types](#field-types-and-attributes)| required
name | Field name is used to aggregate results. | required
selector | Selector represents a CSS selector for data extraction within the given block. | required
attrs | A set of attributes to extract from a Field. Find more information about [attributes](#field-types-and-attributes)  | required 
type | Selector type. ( 0 - image, 1 - text, 2 - link) | required
filters | [Filters](#filters) are used to pre-processing of text data when extracting. | optional
details | Details is an optional field strictly intended for Link extractor type. Details themself represent independent collection to extract data from linked pages. Read more at ["details"](#details) | optional
paginator | Paginator is used to scrape multiple pages. If there is no paginator in Scheme, then no pagination is performed and it is assumed that the initial URL is the only page. Read more about [paginators](#paginator) | optional
path | Path is a special field for navigation only. It is used to collect information from detailed pages. No results from the current page will be returned. Defaults to false. | optional
format | Extracted data is returned either in CSV, MS Excel, JSON, JSON(Lines) or XML format. | required


<aside class="notice">
When requesting <code>/task/create</code> endpoint a new Task object will be created and returned. Refer to <a href="#tasks-amp-processes">Tasks</a> section for more details.
</aside>

<aside class="notice">
Sometimes Base Fetcher is unable to extract data from a web page. Empty results may be returned while parsing Java Script generated pages. Extractor then attempts to force Chrome Fetcher to render the same dynamic javascript driven content automatically.
</aside>

## Field types and attributes

There are 3 predefined field types:

**Text**  extracts human-readable text from the selected element and from all its child elements. HTML tags are stripped and only text is returned.

**Link** is used for link extraction and website navigation. Capture <code>href</code>(URL)  attribute and , <code>link text</code> or specify a special *Path* option for navigation only. When *Path* option specified, all other selectors will be ignored and no results from the current page will be returned.

**Image** selector extracts <code>src</code> (URL) and <code>alt</code> attributes of an image.

## Filters

Filters are used to manipulate text data when extracting.

The following filters are available:

**Trim** returns a copy of the Field's text/ attribute, with all leading and trailing white space removed.

**Normal** leaves the case and capitalization of text/ attribute exactly as is.

**UPPERCASE** makes all of the letters in the Field's text/ attribute uppercase.

**lowercase** makes all of the letters in the Field's text/ attribute lowercase.

**Capitalize** capitalizes the first letter of each word in the Field's text/ attribute

**Concatinate** joins text array element into a single string

<aside class="notice">
Filters can be applied for Text, Link and Image extractor types. Image alt attribute, Link Text and Text are influenced by specified filters.
</aside>

## Regular Expressions

```json
"filters":[ 
    {  
      "name":"regex",
      "param":"[\\d.]+"
    }
]
```


For more advanced text formatting regular expression can be used. 

e.x. the currency signs removed from product prices. 

The whole match (group 0) will be returned as a result.
Some useful examples are listed below:

Input text | Regex | Result
---------- | ----- | ------
price: 10.99€ | <code>[0-9]+\.[0-9]+</code> | 10.99
phone: 0 (944) 244-18-22 | <code>\w+</code> | 09442441822


## Details
>Some parts are omited for brevity

```json
...
"fields":[
  {
      "name":"link2details",
      "selector":"h3 a",
      "details":{
          "name":"DetailsPage",
          "request":{
              "url":"http://example.com/details1/index.html",
              "type":"",
          },
          "fields":[
              {
                  "name":"title",
                  "selector":"h1",
                  "attrs":[
                      "text"
                  ],
              }
          ],
          "paginator":{},
          "path":false,
      },
      "attrs":[
          "href",
          "text"
      ],
  },
  ],
...
```

The *Link* field type might serve as a navigation link to a details page containing additional data.

So following the links from the main page, elements on detailed page can be gathered into separate collection.

Special <code>Path</code> option is used for navigation only. When <code>Path</code> option specified, no results from the current page will be returned. But grouped results from details pages will be returned instead.

Detailed page consists of its own fields and may contain paginators and deeper leveled detailed pages' collections. 

## Paginator

Paginator is used to scrape multiple pages. It extracts the next page from a document by querying a given CSS selector. 

There are three paginator types. 

**"Next link"** paginator type is used on pages containing link pointing to a next page. The next page link is extracted from a document by querying href attribute of a given element's CSS selector. 

**"Infinite scroll"** paginator type automatically loads additional page content while user scrolls page down.

**"Load more Button"** paginator type looks like "Next link" but behaves as "Infinite scroll" paginator type. It loads additional page content on its click.


## Point-and-click toolkit


The most easiest way to define fields for extraction is to use [Dataflow Kit Visual interface](https://dataflowkit.com/dfk) 

Just click elements on loaded page and then export collection to a file. 

![Select Elements](https://dataflowkit.com/static/img/selector-types.png)

![Export collection](https://dataflowkit.com/static/img/export-import.png)

<aside class="success">
<a href="#tasks-amp-processes">Tasks and processes</a> Section will guide you through the steps from specifying CSS Selectors on a web page to downloading results in CSV, MS Excel, JSON, JSON(Lines) or XML format. 
</aside>

<aside class="success">
To get started follow the <a href="https://dataflowkit.com/getting-started">Point and Click tutorial</a>
</aside>