# Request Object

Endpoints which have variable processing times, can respond with **Request Objects**.  These objects wrap the request with information on the status of that request.  In most cases, the API invokes a webhook callback with this object.

**Overview**

<!-- toc -->

## Request Object Lifecycle

```sequence-hand
Title: Request Lifecycle
participant Client Webhook URL
participant Client
participant Tevo API
Note over Client, Tevo API: Initial Request
Client-->Tevo API: POST data (provide webhook_url)
Tevo API-->Client: (partial) Request Object
Note over Client, Tevo API: After Processing
Tevo API-->Client Webhook URL: (complete) Request Object
Note over Client, Tevo API: At Any Point
Client-->Tevo API: GET requests/{resource}/id
Tevo API-->Client: current Request Object
```

## Partial and Complete Request Objects

When the initial request is made, the response will be a Partial Request Object.
Complete Request Objects can be obtained after processing.
These objects are very similar.
The only difference is the partial request will always have a *pending* *state*, and *result* and *error* will be null.


## Request Object Properties
| Name    | Partial Value | Description                                                               |
|---------|---------------|---------------------------------------------------------------------------|
| id      |               | Request Object id                                                         |
| state   | pending       | state of request in our system *(pending, in_process, completed, errored)* |
| error   | null          | Error information for resource                                             |
| params  |               | Params provided with request                                               |
| result  | null          | Request from request                                                      |
