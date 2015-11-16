# Request Object

Endpoints which have variable processing times, can respond with **Request Objects**.  These objects wrap the request with information on the status of that request.  In most cases, the API invokes a [webhook][webhook] callback with this object.

**Overview**

<!-- toc -->

## Webhooks

These objects are primarily used with endpoints supporting webhooks.

>WebHooks are an HTTP POST callback request sent to URL of a user's choice in response to some event occurring. They offer simple and effective server to server communication without long running connections. As discussed on the WebHooks wiki they have a number of uses; we're using them to send real-time events. ~ [Phil Leggetter][webhook]


## Request Object Lifecycle

These endpoints support both traditional POST/GET endpoints, as well as a POST/POST webhook behavior.  

### POST/GET Lifecycle
```sequence
Title: Request Lifecycle
participant Client
participant Tevo API
Client-->Tevo API: POST data (provide webhook_url)
Tevo API-->Client: (partial) Request Object
Note over Client, Tevo API: Poll Show Endpoint Until State Changed
Client->Tevo API: GET requests/{resource}/id
Tevo API-->Client: Current Request Object
```

### POST/POST Lifecycle

```sequence
Title: Request Lifecycle
participant Client
participant Tevo API
Client->Tevo API: POST data (provide webhook_url)
Tevo API-->Client: (partial) Request Object
Note over Client, Tevo API: After Processing
Tevo API->Client: POST (complete) Request Object
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
| result  | null          | Result of request                                                      |

## Examples

### /v9/orders/open

**Request**
```
{
  "webhook_url": "http://client.com/webhook_callback",
  "orders": [
    {
      "buyer_id": "1116",
      "seller_id": "1235",
      "items": [
        {
          "price": "10",
          "quantity": 2,
          "ticket_group_id": "244910671",
          "ticket_group_signature": "1fUXD9W+uimltBl57wrWmgUKoFCIk8kkGNt5HXOOPcg=--lcCF2yLSNRx3QeiHY4SI/g=="
        }
      ]
    }
  ],
}
```

**Initial Response**
```
{
  "id": 123,
  "state": "pending",
  "error": null,
  "result": null,
  "params": {
    "webhook_url": "http://client.com/webhook_callback",
    "orders": **order_create
}
```
[**order_create](https://ticketevolution.atlassian.net/wiki/pages/viewpage.action?pageId=9994275)

**Webhook Response after processing**
```
{
  "id": 123,
  "state": "completed",
  "error": null,
  "result": **order_show,
  "params": {
    "webhook_url": "http://client.com/webhook_callback",
    "orders": [
      {
        "buyer_id": "1116",
        "seller_id": "1235",
        "items": [
          {
            "price": "10",
            "quantity": 2,
            "ticket_group_id": "244910671",
            "ticket_group_signature": "1fUXD9W+uimltBl57wrWmgUKoFCIk8kkGNt5HXOOPcg=--lcCF2yLSNRx3QeiHY4SI/g=="
          }
        ]
      }
    ],
  }
}
```
[**order_show](https://ticketevolution.atlassian.net/wiki/pages/viewpage.action?pageId=4129639)

[webhook]: http://www.programmableweb.com/news/what-are-webhooks-and-how-do-they-enable-real-time-web/2012/01/30