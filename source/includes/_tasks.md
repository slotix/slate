# Tasks

>Task object

```json
{
  "id": "tCcB4hfFP6wvBRe2gwZv9aJp",
  "payloadID": "t-0WMEZ-Bc9sWGHAMsYvP7y4",??? ubrat' payloadID + storeID nafig chto li ??? my k nim mojem dostuchat'sja i cherez task endpoints cherez task ID
  "storeID": "B4hfFP6wvBRddewrfseZv9aJp",
  "requests": 1000,
  "fileSize": 10,
  "status": "completed",
  "createdAt": "2019-04-14T23:09:38",
  "startTime": "2019-04-14T23:09:38",
  "endTime": "2019-04-14T23:10:40",
  "elapsed": "60"
}
```

Task object represents an instance of web data extraction or conversion job that was run at a given time with a given set of parameters. It has the following properties:

Property |  Description 
---------- |  ----- 
id | A globally unique id representing this Task.
payloadId | An MD5 hash generated value from the corresponding collection or conversion scheme that belongs to this task.
storeId | A globally unique id representing the collected or converted data belongs to this task .
requests | The number of successful requests for web data extraction tasks that have been performed by this task so far. This field is ignored for data conversion tasks. 
fileSize | The size of converted file in Megabytes for conversion tasks. This field is ignored for web data extraction tasks. 
status | The status of the task. It can be one of <code>initialized</code>, <code>queued</code>, <code>running</code>, <code>cancelled</code>, <code>completed</code>, or <code>error</code>.
createdAt | The time that this task was created at, in UTC +0000.
startTime | The time that this task was started at, in UTC +0000.
endTime | The time that this task was stopped. This field will be null if the run is either initialized or running. Time is in UTC +0000.
elapsed | Period of time in seconds since startTime till endTime. 

The next sections describe all of the HTTP endpoints that can be used to manipulate Tasks.

## Run a task

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/tasks/{TASK_ID}/run?api_key={YOUR_API_KEY}
```

<code>Run</code> method starts running a task in the Dataflow Kit cloud. This method will return immediately the status of task, while the task continues in the background. You can use webhooks or polling to figure out when the data for this task is ready in order to retrieve it.

If successful, returns the status of task that was launched.

## Cancel a task

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/tasks/{TASK_ID}/cancel?api_key={YOUR_API_KEY}
```

<code>Cancel</code> method cancels a task and changes its status to cancelled. Any data that was extracted so far will be available.

## Task info

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/tasks/{TASK_ID}/info?api_key={YOUR_API_KEY}
```

Gets a task object that contains all the details about a specific task.

## Delete a task.

>Delete a task.

```shell
curl --request DELETE \
     --url https://api.dataflowkit.com/v1/tasks/{TASK_ID}?api_key={YOUR_API_KEY}
```

This endpoint deletes a specific task along with corresponding resulted data.


## Get a list of tasks

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/tasks?api_key={YOUR_API_KEY}
```

This endpoint returns the list of all tasks that the user created or used. The response is a list of tasks where each object contains a basic information about a single task. By default, the records are sorted by the <code>createdAt</code> field in ascending order.

## Get a task payload.

>Get a task's payload configuration scheme

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/tasks/{TASK_ID}/scheme?api_key={YOUR_API_KEY}
```

Gets an object that contains all the details about a specific payload of either task's collection configuration scheme or file conversion settings.

## Get a task results.

>Get a task's results after completion.

```shell
curl --request POST \
     --url https://api.dataflowkit.com/v1/tasks/{TASK_ID}/store?api_key={YOUR_API_KEY}
```

Once after task completes the job, its status changes to <code>completed</code>.

### Web Data extraction task

If successful, returns the link to resulted data in either CSV, MS Excel, JSON, JSON Lines or XML format, depending on the format parameter from specified collection scheme.

The error is returned otherwise.

### File conversion task

If successful, returns the link to resulted data in specific format, depending on conversion destination setting.

The error is returned otherwise.
