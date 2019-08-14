# Convert to PDF

Convert PDF endpoint is used for converting URL, local HTML, Markdown and Office documents to PDF. 

HTML and Markdown conversions are performed using Google Chrome headless browser. 

Assets: You can send your header, footer, images, fonts, stylesheets and so on for converting your HTML and Markdown to PDFs.

## URL 

```shell 
curl --request POST \
    --url https://api.dataflowkit.com/v1/convert/url/pdf?api_key=YOUR_API_KEY \
    --header 'Content-Type: multipart/form-data' \
    --form remoteURL=https://dataflowkit.com \
    --form marginTop=0 \
    --form marginBottom=0 \
    --form marginLeft=0 \
    --form marginRight=0
```

Use Dataflow Kit endpoint <code>/convert/url/pdf</code> to convert remote URL to PDF. 

It accepts POST requests with a multipart/form-data Content-Type.

Parameter | Default | Description 
---------- | ----- | ----- 
remoteURL | - |Remote URL to be converted to PDF
marginTop | 0 | 
marginBottom | 0 | 
marginLeft | 0 | 
marginRight | 0 | 

<aside class="warning">
When converting a remote URL to PDF, you should remove all margins. If not, some of the content of the page might be hidden.
</aside>

 

## HTML 

```shell
curl --request POST \
    --url https://api.dataflowkit.com/v1/convert/html/pdf?api_key=YOUR_API_KEY \
    --header 'Content-Type: multipart/form-data' \
    --form files=@index.html \
    --form files=@header.html \
    --form files=@footer.html \
    --form files=@style.css \
    --form files=@img.png \
    --form files=@font.woff \
    --form paperWidth=8.27 \
    --form paperHeight=11.27 \
    --form marginTop=1.2 \
    --form marginBottom=1.2 \
    --form marginLeft=1 \
    --form marginRight=1 \
    --form landscape=true
```
DFK endpoint <code>/convert/html/pdf</code> is intended for HTML file conversions.

Just send a POST requests with a multipart/form-data Content-Type.

Parameter | Default | Description 
---------- | ----- | ----- 
files | - | Specify html files to be converted to PDF. The main file index.html is required. All others parameters are optional
paperWidth | 8.27 | 
paperHeight | 11.69 |
marginTop | 1 | 
marginBottom | 1 |
marginLeft | 1 | 
marginRight | 1 |
landscape | false | By default, it will be rendered with portrait orientation.

Using parameters you can customize the resulting PDF file.

Paper size and margins have to be provided in inches.

By default, it will be rendered with A4 size, 1 inch margins and portrait orientation.


### Header and footer

> header.html file sample

```html
<html>
<head>
  <style>
    body {
      font-size: 14px;
      margin: 100px auto;
    }
  </style>
</head>
<body>
  <h1>Header</h1>
  <p><span class="date"></span></p>
  <p>
    <span class="pageNumber"></span> of <span class="totalPages"></span>
  </p>
</body>
</html>
```

You may also specify a header and/or a footer in the resulting PDF. Respectively, a file named <code>header.html</code> and <code>footer.html</code>

They should be a complete HTML document like:

The following classes are helpfull for injecting printing values:

Class | Description
---- | ------
date | formatted print date
title | document title
pageNumber | current page number
totalPage | total pages in the document

<aside class="notice">CSS properties from <code>header.html</code> override the ones from <code>index.html</code>. Also, <code>footer.html</code> CSS properties override the ones from <code>header.html</code>.
</aside>

### Assets

>Adding assets to resulted PDF

```html
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>New PDF</title>
  </head>
  <body>
    <img src="logo.jpg">
    <h1>Hello world!</h1>
  </body>
</html>
```
You may also include additional files like images, fonts, stylesheets and so on to a rendered PDF file.

They have to be located in the same directory as <code>index.html</code>. 

Using external paths for Google fonts, images is ok.

## Markdown

```shell
curl --request POST \
    --url https://api.dataflowkit.com/v1/convert/markdown/pdf?api_key=YOUR_API_KEY \
    --header 'Content-Type: multipart/form-data' \
    --form files=@index.html \
    --form files=@file.md
```
> Sample index.html file for Markdown to PDF conversion

```html
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>New PDF</title>
  </head>
  <body>
    {{ toHTML .DirPath "file.md" }}
  </body>
</html>
```

Use Dataflow Kit endpoint <code>/convert/markdown/pdf</code> to convert Markdown format to PDF.

It accepts POST requests with a multipart/form-data Content-Type.

Converting from Markdown to PDF works the same way as HTML to PDF endpoint does.

The only difference is that you have access to the Go template function toHTML in the file index.html. This function will convert a given markdown file to HTML.

Please refer to [HTML conversion](#html) section for details about parameters used for Markdown to PDF conversion.

## Ofice 

Dataflow Kit endpoint <code>/convert/office/pdf</code> is used for Office document to PDF conversions.

It accepts POST requests with a multipart/form-data Content-Type.

The following file formats are supported:

 Text | Spreadsheets | Presentations  
---- | ------ | -------
.txt | .xls | .ppt
.rtf | .xlsx | .pptx
.fodt | .ods | .odp
.doc | |
.docx | |
.odt | |

All files will be merged into a single resulting PDF.

```shell
curl --request POST \
    --url https://api.dataflowkit.com/v1/convert/ofice/pdf?api_key=YOUR_API_KEY \
    --header 'Content-Type: multipart/form-data' \
    --form files=@document1.docx \
    --form files=@document2.doc \
    --form files=@spreadsheet.xlsx \
    --form landscape=true
```

Parameter |  Description 
---------- |  ----- 
files |  Specify document files to be converted to PDF. At least on file have to be specified. All others parameters are optional
landscape |  By default, it will be rendered with portrait orientation.

## Merge

```shell
curl --request POST \
    --url https://api.dataflowkit.com/v1/merge/pdf?api_key=YOUR_API_KEY \
    --header 'Content-Type: multipart/form-data' \
    --form files=@pdf1.pdf \
    --form files=@pdf2.pdf \
    --form files=@pdf3.pdf
```

DFK Merge endpoint <code>/merge/pdf</code> is intended for merging several PDFs into one resulting PDF.

It accepts POST requests with a multipart/form-data Content-Type.

Just send some PDF files and DFK API will merge them and return the resulting PDF file.

<aside class="notice">
PDF files will be merged alphabetically.
</aside>

## Results

```shell
curl --request GET \
     --url  "https://dfk-storage.ams3.digitaloceanspaces.com/result_office2pdf_2019-08-09_14%3A39.pdf?X-Amz-Signature=b23fffd81b29f4a597eaa6c29b34501144d1687e6d08bc33141ddae9f7ff1f69"
```

As a results a link to resulted PDF file returned.

Run the script on the right to download results providing the link.

## Webhooks

```shell 
curl --request POST \
    --url https://api.dataflowkit.com/v1/convert/html/pdf?api_key=YOUR_API_KEY \
    --header 'Content-Type: multipart/form-data' \
    --form files=@index.html \
    --form webhookURL='http://mywebsite.com/webhook/'
```

All PDF conversion endpoints accept a form field named webhookURL.

If provided, Dataflow Kit API will send the resulting PDF file in a POST request with the <code>application/pdf</code> Content-Type to given URL.

TODO: ???? fileSize | The size of converted file in Megabytes for conversion Tasks. This field is ignored for web data / SERP extraction Tasks. 