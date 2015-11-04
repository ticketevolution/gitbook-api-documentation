# Response Object

Endpoints which have variable processing times, can respond with **Response Objects**.  These objects wrap the request with information on the status of that request.  In most cases, the API invokes a webhook callback with this object.

## Response Object Lifecycle

``` sequence
Title: Initial Request
participant Client
participant Tevo API
Client->Tevo API: POST data (provide webhook_url)
Tevo API->Client: (partial) Response Object
```

``` sequence
Title: After Processing Request
participant Client Webook URL
participant Tevo API
Tevo API->Client Webhook URL: (complete) Response Object
```

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