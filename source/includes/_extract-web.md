# Extract data from web

<code>/extract</code> endpoint crawls web pages and extracts data like text, links or images following the specified rules. Dataflow kit uses CSS selectors to find HTML elements in web pages and to extract data from. Extracted data is returned in CSV, MS Excel, JSON, JSON(Lines) or XML format.

## Collection scheme

>Here is a simple collection object:

```json
'{
    "name":"test.dataflowkit.com",
    "request":{
        "url":"https://test.dataflowkit.com/persons/page-0",
        "type":"chrome"
    },
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
    "paginator":".page-link",
    "path":false
}'
```

Collection scheme represents settings for data extraction from specified web site. It has the following properties:

Property | Description | Required
--------- | ----------- | -----------
name | Collection name | required
request | Request parameters for downloading html pages. Refer to [Fetch HTML](#fetch-html) section for more details about request parameters  | required
url | url holds the the starting web page address to be downloaded. URL is required. | required
type | type specifies fetcher type which may be "base" or "chrome" value. If omited "base" fetcher is used by default | optional
fields | A set of fields used to extract data from a web page. A Field represents a given chunk of data to be extracted from every block on each page. Read more about [field types](#field-types-and-attributes)| required
name | Field name is used to aggregate results. | required
selector | Selector represents a CSS selector for data extraction within the given block. Pass in "." to use the root block's selector. | required
attrs | A set of attributes to extract from a Field. Find more information about [attributes](#field-types-and-attributes)  | required 
filters | [Filters](#filters) are used to pre-processing of text data when extracting. | optional
details | Details is an optional field strictly for Link extractor type. It guides scraper to parse additional pages following the links according to the set of fields specified inside ["details"](#details).  | optional
paginator | Paginator is used to scrape multiple pages. If there is no paginator in Scheme, then no pagination is performed and it is assumed that the initial URL is the only page. Read more about [paginators](#paginator) | optional
path | Path is a special field for navigation only. It is used to collect information from detailed pages. No results from the current page will be returned. Defaults to false. TODO: Add path example | optional
format | Extracted data is returned either in CSV, MS Excel, JSON, JSON(Lines) or XML format. | required
delivery | Email, Amazon S3 bucket, FTP, Dropbox, etc. *Not implemented yet*

<aside class="notice">
When requesting <code>/extract</code> endpoint a new Task object will be created. TaskID value is returned as a response. Once a new Task is created, you manipulate directly the Task object with returned TaskID. Refer to <a href="#tasks-amp-processes">Tasks</a> section for more details. 
</aside>

<aside class="notice">
Sometimes Base Fetcher is unable to extract data from a web page. Empty results may be returned while parsing Java Script generated pages. Extractor then attempts to force Chrome Fetcher to render the same dynamic javascript driven content automatically.
</aside>

## Field types and attributes

There are 3 predefined field types:

**Text**  extracts human-readable text from the selected element and from all its child elements. HTML tags are stripped and only text is returned.

**Link** is used for link extraction and website navigation.Capture <code>href</code>(URL), <code>text</code> attributes or specify a special *Path* option for navigation only. When *Path* option specified, all other selectors become disable and no results from the current page will be returned.

**Image** selector extracts <code>src</code> (URL) and <code>alt</code> attributes of an image.

## Filters

Filters are used to manipulate text data when extracting.

The following filters are available:

**Trim** returns a copy of the Field's text/ attribute, with all leading and trailing white space removed.

**Normal case** leaves the case and capitalization of text/ attribute exactly as is.

**UPPERCASE** makes all of the letters in the Field's text/ attribute uppercase.

**lowercase** makes all of the letters in the Field's text/ attribute lowercase.

**Capitalize** capitalizes the first letter of each word in the Field's text/ attribute

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

e.x. the currency signs removed from product pricesls 

The whole match (group 0) will be returned as a result.
Some useful examples are listed below:

Input text | Regex | Result
---------- | ----- | ------
price: 10.99€ | <code>[0-9]+\.[0-9]+</code> | 10.99
id: H18JKDX4 | <code>[A-Z0-9]{8}</code> | H18JKDX4
date: 2018-10-19 | <code>[0-9]{4}\-[0-9]{2}\-[0-9]{2}</code> | 2019-04-02

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
          "paginator":"",
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

Paginator is used to scrape multiple pages. It extracts the next page from a document by querying a given CSS selector and extracting the given HTML attribute from the resulting element. 

There are three paginator types. 

**"Next link"** paginator type is used on pages containing Next Button Paginator link. 

**"Infinite scroll"** automatically loads content while user scrolls page down.

**"Load more Button"** looks like "Next link" but loads content on its click.

Type represents paginator type. The following are available: "next", "more", "infinite" Selector represents corresponding CSS selector for the "Next" link or "Load more" Button paginator types page along with Attr belong exclusively to "Next" link paginator to define HTML element attribute for the next page. 


## Point-and-click toolkit


The most comfortable way to define fields for extraction is to use [Dataflow Kit Visual interface](https://dataflowkit.com/dfk) 

Just click elements on loaded page and then export collection to a file. 

![Select Elements](https://dataflowkit.com/static/img/selector-types.png)

![Export collection](https://dataflowkit.com/static/img/export-import.png)

<aside class="success">
<a href="#tasks-amp-processes">Tasks and processes</a> Section will guide you through the steps from specifying CSS Selectors on a web page to downloading results in CSV, MS Excel, JSON, JSON(Lines) or XML format. 
</aside>