# Orders

<!-- toc -->

## POST /v9/orders/open

The */v9/ordres/create* endpoint has a potentially long response time.
Instead of waiting, the *open* endpoint will respond immediately with a  [Request Object][RequestObject].
This can be used to lookup the request status at a later time.

Endpoints responding with [Request Object's][RequestObject] accept a *webhook_url* property which will be automatically called when the request is finished processing.

### Properties

The POST data must follow this format.

| Name        | Required | Type | Description                                                                |
|-------------|----------|------|----------------------------------------------------------------------------|
| webhook_url | no       | String | Resource to notify when [Completed Request Object][RequestObject] is ready |
| orders      | yes      | Array |1 element array containing valid [Order POST data][OrderEndpoint]          |


### Examples

**Request**
```
{
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
  ]
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

[RequestObject]: https://ticketevolution.gitbooks.io/api-documentation/content/Chapters/request_object.html
[OrderEndpoint]: https://ticketevolution.atlassian.net/wiki/pages/viewpage.action?pageId=9994275