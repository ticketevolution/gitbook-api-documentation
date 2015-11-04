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
