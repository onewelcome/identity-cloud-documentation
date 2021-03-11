# Events API

This section describes the REST APIs that delivers details about operations executed within Onegini Access.

## Authentication

All endpoints contained within the Events API are secured by API client credentials. To configure an API client, see the 
[API Configuration topic guide](../../topics/technical-app-management/api-configuration/api-configuration.md).

It requires an API client with the scope `onegini_api_events` (Events API).

## Endpoints

### Search for events

* Endpoint: `/api/v1/events`
* Method: GET

Pagination specific request parameters:

| Param       | Required | Default value | Description                                                                  
|-------------|----------|---------------|------------------------------------------------------------------------------
| page        | no       | 0             | Specific result page. If the requested page is larger then the last available page, empty result set is returned. 
| size        | no       | 20            | Number of the results within single page. The maximum page size is 1000.

Search criteria request parameters:

| Param              | Example value                                | Search criteria    
|--------------------|----------------------------------------------|--------------------------------------------------------------------
| user_id            | b3948273-117b-413a-9f8f-7e7750bbecc8         | Return events specific to a given user. The same user may be interacting via different clients. This criteria is case insensitive.
| client_id          | 873BE193F83821A32DA4…CCACB1C44F7434          | Return events specific to a given client. This could be either a web client or a specific mobile client.
| event_type         | ADMIN_CLIENT_DELETED                         | Return events of a specific type. See [appendix](../../appendix/access-events.md) for a complete list of events. Event type needs to be given in uppercase and with a underscore (`_`) as a separator. Multiple parameters are allowed. 
| exclude_event_type | AUTHZ_REQUEST_GRANT_CREATED                  | Return events of a specific type. See [appendix](../../appendix/access-events.md) for a complete list of events. Event type needs to be given in uppercase and with a underscore (`_`) as a separator. Multiple parameters are allowed. 
| transaction_id     | ONEGINI_95830280-7ade-4527-95ce-67db29dc33d5 | Return events related to a specific transaction. 
| start_date         | 1555405987532                                | Return events which occurred at or after a specific point in time.
| end_date           | 1555405989532                                | Return events which occurred before or at a specific point in time.
| end_date_exclusive | true                                         | Exclude from the results the events that occurred exactly at a point in time specified by `end_date` timestamp.

All search criteria filters are optional, when multiple filters are specified all conditions need to be met (ie. conditions are combined using `AND` operator).
`event_type` is the only exception where it will use an `OR` so you can filter on a specific set of events.
The results are sorted descending by a timestamp (latest first).

JSON Attributes:

| Attribute            | Description                                                                                  
|----------------------|---------------------------------------------------------------------------------------------------------------------
| `event_identifier`   | UUID identifying the event
| `event_name`         | Display friendly name of the event type
| `event_type`         | Raw value of the event type
| `client_id `         | Client identifier associated with the event
| `app_name`           | Name of the mobile app or web client associated with the event
| `transaction_id`     | Transaction identifier associated with the event
| `user_id`            | User identifier associated with the event
| `client_ip`          | Ip address of the device associated with the event
| `user_agent`         | User agent of the device associated with the event
| `event_agent_user`   | User identifier of agent who performed the action. This usually represents a user with administrative privileges
| `occurred`           | Milliseconds elapsed from epoch since the event occurred
 

```http
GET /api/v1/events?event_type=ADMIN_CLIENT_DELETED&event_type=API_DEVICE_REMOVED&start_date=1555405987532 HTTP/1.1
Host: onegini.example.com
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
    "result_set": [
        {
            "event_identifier": "0b2463c1-5808-482f-ab20-b9971924cb5a",
            "event_name": "Admin client deleted",
            "event_type": "ADMIN_CLIENT_DELETED",
            "client_id": "873BE193F83821A32DA41FDB6712ABCE89DF105E9C874A7452CCACB1C44F7434",
            "app_name": "Example app",
            "transaction_id": "ONEGINI_95830280-7ade-4527-95ce-67db29dc33d5"
            "user_id": "b3948273-117b-413a-9f8f-7e7750bbecc8",
            "client_ip": "192.168.0.1",
            "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36",
            "event_agent_user": "admin",
            "occurred": 1555405987532
        },
        {
            ... more search results
        }
    ],
    "pagination": {
        "total_results": 3,
        "offset": 0,
        "page_size": 10,
        "max_visible": 5,
        "pages_before": 2,
        "range_start": 1,
        "range_end": 3,
        "first_page": true,
        "last_page": true,
        "number_of_pages": 1,
        "previous_page": null,
        "next_page": null,
        "visible_pages": []
    }
}
```

