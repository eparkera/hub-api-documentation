# API Documentation: ePark Hub
ePARK documentation on how to communicate with our hub

# Version 1.3.1

> *Last updated: 2024-12-12*
> 

The ePark HUB Service API allows users to create, update and retrieve parking tickets through HTTP requests. All requests are sent to the base URL: **[https://hub.eparkera.se](https://hub.eparkera.se/)**.

Sandbox base URL: **[https://sandbox-hub.eparkera.se](https://sandbox-hub.eparkera.se/)**.

> *Tickets TTL is 5 minutes in sandbox environment and logs are available for 30 days*

**Authentication**: Bearer Token

**Response:** JSON

---

## **Endpoints**

### **Create Ticket**

Endpoint: **`/ticket`**

HTTP Method: **`POST`**

Create a new parking ticket.

### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| order_reference | string, min:10 max:255 | Yes | A unique identifier for the parking ticket. |
| starts_at | UTC Date (Y-m-dTH:i:s+00:00) | Yes | The starting time of the parking ticket. Iso8601String |
| ends_at | UTC Date (Y-m-dTH:i:s+00:00) | Yes | The ending time of the parking ticket. Iso8601String |
| cancelled_at | UTC Date (Y-m-dTH:i:s+00:00) | No | The time the parking ticket was ended. Iso8601String |
| license_plate_number | string | Yes | The license plate number of the vehicle parked. |
| price | int | No | The price of the parking ticket in cents. |
| price_excl_vat | bool | No | Whether the price is excluding VAT. Default is false. |
| currency | string | No | The currency used for the payment. Default is "SEK". |
| lat | float | No | The latitude of the location where the vehicle parked. |
| lon | float | No | The longitude of the location where the vehicle parked. |
| public_zone_code | string | No | The public area code where the vehicle parked. |

### Response

See ticket response below

### Error Responses

**`401 Unauthorized`**

```
{
    "message": "Unauthenticated."
}
```

**`404 Not Found`**

```
{
    "message": "Public zone code not found.",
    "errors": {
        "public_zone_code": [
            "Make sure you have access to this public zone code."
        ]
    }
}
```

**`422 Unprocessable Content`**

```
{
    "message": "Validation failed.",
    "errors": {
        "order_reference": [
            "The order reference has already been taken."
        ]
    }
}
```
---

### Update **Ticket**

Endpoint: **`/ticket/{order_reference}`**

HTTP Method: **`PATCH`**

Update parking ticket.

### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| starts_at | UTC Date (Y-m-dTH:i:s+00:00) | No | The starting time of the parking ticket. Iso8601String |
| ends_at | UTC Date (Y-m-dTH:i:s+00:00) | No | The ending time of the parking ticket. Iso8601String |
| cancelled_at | UTC Date (Y-m-dTH:i:s+00:00) | No | The time the parking ticket was ended. Iso8601String |
| license_plate_number | string | No | The license plate number of the vehicle parked. |
| price | int | No | The price of the parking ticket in cents. |
| price_excl_vat | bool | No | Whether the price is excluding VAT. Default is false. |
| currency | string | No | The currency used for the payment. Default is "SEK". |
| lat | float | No | The latitude of the location where the vehicle parked. |
| lon | float | No | The longitude of the location where the vehicle parked. |
| public_zone_code | string | No | The public area code where the vehicle parked. |

### Response

See ticket response below

### Error Responses

**`401 Unauthorized`**

```
{
    "message": "Unauthenticated."
}
```

**`404 Not Found`**

```
{
    "message": "Session not found.",
    "errors": {
        "session": [
            "Make sure you have access to this session."
        ]
    }
}
```

**`422 Unprocessable Content`**

```
{
    "message": "Validation failed.",
    "errors": {
        "ends_at": [
            "The ends at must be a date after starts at."
        ]
    }
}
```

---

### Get **Ticket**

Endpoint: **`/ticket/{order_reference}`**

HTTP Method: **`GET`**

Update parking ticket.

### Response

The response body contains the details of the parking ticket.

| Parameter | Type | Description |
| --- | --- | --- |
| order_reference | string | A unique identifier for the parking ticket. |
| starts_at | UTC Date (Y-m-dTH:i:s+00:00) | The starting time of the parking ticket. Iso8601String |
| ends_at | UTC Date (Y-m-dTH:i:s+00:00) | The ending time of the parking ticket. Iso8601String |
| cancelled_at | UTC Date (Y-m-dTH:i:s+00:00) | The time the parking ticket was cancelled. Iso8601String |
| license_plate_number | string | The license plate number of the vehicle parked. |
| price | int | The price of the parking ticket in cents. |
| price_ex_vat | int | The price of the parking ticket excluding VAT. |
| currency | string | The currency used for the payment. |
| lat | Float | Coordinare |
| lon | Float | Coordinate |
| public_zone_code | string | The public area code where the vehicle parked. |

### Error Responses

**`401 Unauthorized`**

```
{
    "message": "Unauthenticated."
}
```

**`404 Not Found`**

```
{
    "message": "Session not found.",
    "errors": {
        "session": [
            "Make sure you have access to this session."
        ]
    }
}
```

---

### Get **Zones**

Endpoint: **`/zones`**

HTTP Method: **`GET`**

Get all current zones

### Response

The response body contains the details of the zone

| Parameter | Type | Description |
| --- | --- | --- |
| public_zone_code | string | The public area code where the vehicle parked. |
| title | string | Title of the zone |

---

### Search Free Time Used

Endpoint: **`/free-time/{licensePlateNumber}/{publicZoneCode}`**

HTTP Method: **`GET`**

Search free time used for this license plate on this zone.

### Request Parameters

| Parameter          | Type            | Required | Description |
|--------------------|-----------------| --- | --- |
| licensePlateNumber | string, min:2 max:255 | Yes | The license plate number of the vehicle parked. |
| publicZoneCode     | string          | Yes | The public area code where the vehicle parked. |

### Response

The response body contains the free parking time used for the current day.

| Parameter | Type   | Description                                        |
| --- |--------|----------------------------------------------------|
| license_plate_number | string | The license plate number of the vehicle parked.    |
| public_zone_code | string | The public area code where the vehicle parked.     |
| free_time_used | int    | The amount of free time parked today, in seconds. |

### Error Responses

**`401 Unauthorized`**

```
{
    "message": "Unauthenticated."
}
```

**`404 Not Found`**

```
{
    "message": "Public zone code not found.",
    "errors": {
        "public_zone_code": [
            "Make sure you have access to this public zone code."
        ]
    }
}
```

**`422 Unprocessable Content`**

```
{
    "message": "Validation failed.",
    "errors": {
        "license_plate_number": [
            "License plate number must be at least 2 characters."
        ]
    }
}
```

---

### Search Max Time Used

Endpoint: **`/max-time/{licensePlateNumber}/{publicZoneCode}`**

HTTP Method: **`GET`**

Search max time used for this license plate on this zone.

### Request Parameters

| Parameter          | Type            | Required | Description |
|--------------------|-----------------| --- | --- |
| licensePlateNumber | string, min:2 max:255 | Yes | The license plate number of the vehicle parked. |
| publicZoneCode     | string          | Yes | The public area code where the vehicle parked. |

### Response

The response body contains the max parking time used for the current day.

| Parameter            | Type   | Description                                   |
|----------------------|--------|-----------------------------------------------|
| license_plate_number | string | The license plate number of the vehicle parked. |
| public_zone_code     | string | The public area code where the vehicle parked. |
| max_time_used        | int    | The amount of minutes parked.       |

### Error Responses

**`401 Unauthorized`**

```
{
    "message": "Unauthenticated."
}
```

**`404 Not Found`**

```
{
    "message": "Public zone code not found.",
    "errors": {
        "public_zone_code": [
            "Make sure you have access to this public zone code."
        ]
    }
}
```

**`422 Unprocessable Content`**

```
{
    "message": "Validation failed.",
    "errors": {
        "license_plate_number": [
            "License plate number must be at least 2 characters."
        ]
    }
}
```