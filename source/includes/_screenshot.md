# Take a screenshot

```shell 
curl --request POST \
    --url https://api.dataflowkit.com/v1/screenshot?api_key={YOUR_API_KEY} \
    --header 'Content-Type: multipart/form-data' \
    --form remoteURL=https://dataflowkit.com \
    --form format=png \
    --form qality=80 \
    --form xoffset=0 \
    --form yoffset=0 \
    --form width=800 \
    --form height=600 \
    --form scale=1 \
    -o result.png
```

Dataflow Kit Screenshot endpoint is intended for taking screenshots from web pages and store them in the DFK cloud. 

It accepts POST requests with a multipart/form-data Content-Type.

Parameter | Default | Description 
---------- | ----- | ----- 
remoteURL | - |Remote URL to take a screenshot from
format | png | Sets the Format of output image. Values: png, jpeg 
qality | 80 | Sets the Quality of output image. Compression quality from range \[0..100\] (jpeg only).
wholePage | false | takes a screenshot of a whole web page. It ignores xoffset, yoffset, width and height argument values.
xoffset | 0 | X offset in device independent pixels (dip).
yoffset | 0 | Y offset in device independent pixels (dip).
width | 800 | Rectangle width in device independent pixels (dip). 
height | 600 | Rectangle height in device independent pixels (dip).
scale | 1 | Page scale factor. defaults to 1