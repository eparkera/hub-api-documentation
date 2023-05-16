# API Documentation: ePark Hub
ePARK documentation on how to communicate with our hub

# Version 1.2.2

> *Last updated: 2023-05-16*
> 

The ePark HUB Service API allows users to create, update and retrieve parking tickets through HTTP requests. All requests are sent to the base URL: **[https://hub.epark.se](https://hub.epark.se/)**.

**Authentication**: Barer Token

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

---

### Update **Ticket**

Endpoint: **`/ticket/{order_reference}`**

HTTP Method: **`PATCH`**

Update parking ticket.

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
| public_area_code | string | No | The public area code where the vehicle parked. |

### Response

See ticket response below

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
| public_area_code | string | The public area code where the vehicle parked. |

---

### Get **Zones**

Endpoint: **`/zones`**

HTTP Method: **`GET`**

Get all current zones

### Response

The response body contains the details of the zone

| Parameter | Type | Description |
| --- | --- | --- |
| public_area_code | string | The public area code where the vehicle parked. |
| title | string | Title of the zone |
