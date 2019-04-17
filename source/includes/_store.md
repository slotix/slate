# Stores

>Store object

```json
{
  "storeToken": "B4hfFP6wvBRddewrfseZv9aJp",
  "taskToken": "tCcB4hfFP6wvBRe2gwZv9aJp",
  "createdAt": "2019-04-14T23:09:38",
  "records": 1000,
  "fileSize": 10,
  "md5sum": "f82f56816560943564803e005cb71d26"
}
```

Stores section describes DFK API endpoints to manage Key-value stores for saving and reading data records or files. Each data record is represented by a unique key and associated with a MIME content type. Key-value stores are ideal for saving web pages, scraped outputs, PDFs. For more information, see the Key-value store documentation.

Property |  Description 
---------- |  ----- 
storeID | A globally unique id representing the collected or converted data that this task belongs to.
taskID | A globally unique id representing this Task.
createdAt | The time that this task was created at, in UTC +0000.
records | The number of records in the store.
fileSize | The size of converted file in Megabytes.
md5sum | The md5sum of the results. This can be used to check if any results data has changed between two runs of a task.
