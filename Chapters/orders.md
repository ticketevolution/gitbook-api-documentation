# Orders

<!-- toc -->

## POST /v9/orders/open

The */v9/ordres/create* endpoint has a potentially long response time.
Instead of waiting, the *open* endpoint will respond immediately with a  [Request Object][RequestObject].
This can be used to lookup the request status at a later time.

Endpoints responding with [Request Object's][RequestObject] accept a *webhook_url* property which will be automatically called when the request is finished processing.

### Properties

The POST data must follow this format.

| Name        | Required | Description                                                                |
|-------------|----------|----------------------------------------------------------------------------|
| webhook_url | no       | Resource to notify when [Completed Request Object][RequestObject] is ready |
| orders      | yes      | 1 element array containing valid [Order POST data][OrderEndpoint]          |

[RequestObject]: https://ticketevolution.gitbooks.io/api-documentation/content/Chapters/request_object.html
[OrderEndpoint]: https://ticketevolution.atlassian.net/wiki/pages/viewpage.action?pageId=9994275
