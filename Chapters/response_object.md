# Response Object

Endpoints which have variable processing times, can respond with **Response Objects**.  These objects wrap the request with information on the status of that request.  In most cases, the API invokes a webhook callback with this object.

### Summary

<!-- toc -->

## Response Object Lifecycle

```sequence-hand
Title: Request Lifecycle
participant Client Webhook URL
participant Client
participant Tevo API
Note over Client, Tevo API: Initial Request
Client-->Tevo API: POST data (provide webhook_url)
Tevo API-->Client: (partial) Response Object
Note over Client, Tevo API: After Processing
Tevo API-->Client Webhook URL: (complete) Response Object
Note over Client, Tevo API: At Any Point
Client-->Tevo API: GET requests/{resource}/id
Tevo API-->Client: current Response Object
```

## Response Object Properties
|         |                                           |
|---------|-------------------------------------------|
| id      | Response Object id                        |
| state   | state of request in our system *(pending, in_process, completed, errored)* |
| result  |                                           |
| error   |                                           |
| params  |                                           |
| payload |                                           |

```
{
  :id,
  :state,
  :result,
  :error,
  :params,
  :payload
}
```

```
partial Response Object 
{
  :id,
  :state => pending,
  :result,
  :error,
  :params,
  :payload nil
}
```