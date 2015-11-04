# Orders


``` sequence-hand
Title: Opening An Order
participant Client
participant Webhook Callback URL
participant Tevo API
Client->Tevo API: POST /orders/open
Tevo API->Client: partial Response Object
Tevo API->Webhook Callback URL: full Response Object
```

## Response Object

Endpoints which have variable processing times, can respond with Response objects.
All 
Response Objects are an abstraction which wraps certain requests.
They 

|         |                                           |
|---------|-------------------------------------------|
| id      | Response Object id                                      |
| state   | state of request in our system {pending, in_process, completed, errored} |
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
```
=>  POST /orders/open
<=  RESPONSE
    {
      :id,
      :state => pending,
      :result,
      :error,
      :params,
      :order_details nil
    }
<= WEBHOOK
  {
    :id,
    :state,
    :result,
    :error,
    :params,
    :order_details => {
      :id
    }
  }

=> GET /requests/orders/:id
<= RESPONSE
  {
    :id,
    :state,
    :result,
    :error,
    :params,
    :order_details => {
      :id
      ...
    }
  }

=> GET /orders/show
<= RESPONSE
  {
    :id
  }
```
