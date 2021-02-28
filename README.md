# Kofaf - Fire and Forget

Python tool for calling API with these features
- Asynchronous calling
- Throttling

## Usage

```
$ kofaf [dir]
```

`dir` specified the working directory, default to `~/.kofaf`

Inside working directory
- requests/
  all the executed requests move here

- responses/
  all the responses

- kofaf.yml
  configuration file

  ### Configuration Sample

```
  etsy:
    base_url: https://openapi.etsy.com
    rate_second: 10
    rate_hour: 400
    retry: 3
    wait: 2
  shopify:
    base_url: https://korakot.myshopify.com/admin
    rate_second: 2
    callback_success: "curl -X POST https://yoursite.com/post_success/ -d 'id=$file&posted_at=$datetime'"
    callback_fail: "curl -X POST https://yoursite.com/err/ -d 'id=$file&posted_at=$datetime'"

```

| level | key         | Sample               | Description                  |
| ----- | ----------- | -------------------- | ---------------------------- |
| 0     | id          | "etsy"               | identifier                   |
| 1     | rate_second | 10                   | maximum calls per second     |
| 1     | rate_hour   | 400                  | maximum calls per hour       |
| 1     | retry       | 3                    | retry 3 times before give up |
| 1     | wait        | 2                    | wait 2 seconds between retry |
| -     | $file       | order_123.json       | filename                     |
| -     | $datetime   | 2021-02-27T22:36:07Z | datetime in ISO8601          |


### Making an API Call

Kofaf will monitor the working directory and if it detect a new json file
will process the request. Then move the json file to the `requests/` folder.
The response will be saved in the `responses/` under with the same file name

### API Call File Sample

Call files are in JSON format

```
{
    "base_url": "https://korakot.myshopify.com/admin",
    "end_point": "/orders",
    "headers": {
      "X-API": "111222",
      "asa"
    },
    "body": {
      "order_id": 123,
      "customer": "abc inc."
    }
}         
```

