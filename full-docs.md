# Send your first invoice in minutes

1. Create an API key from your [Wafeq Developer console](https://app.wafeq.com/c/).

2. Make a `POST` request like below. Make sure to replace your `<api_key>` with your API key.

```shell
curl --location --request POST 'https://api.wafeq.com/api/v1/invoices/bulk_send/' \
--header 'Authorization: Api-Key <api_key>' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "reference": "X-1234",
        "invoice_number": "INV-1234",
        "invoice_date": "2021-01-01",
        "currency": "AED",
        "language": "en",
        "tax_amount_type": "TAX_INCLUSIVE",
        "contact": {
            "name": "Jane Doe",
            "address": "Jane Doe Residency, Dubai, UAE",
            "email": "jane.doe@example.com"
        },
        "channels": [
            {
                "medium": "email",
                "data": {
                    "subject": "Invoice X from the Company of John Doe",
                    "message": "<p>Please find attached your invoice.</p>",
                    "recipients": {
                        "to": [
                            "jane.doe@example.com"
                        ],
                        "cc": [],
                        "bcc": []
                    }
                }
            }
        ],
        "line_items": [
            {
                "name": "Item 1",
                "description": "Item description 1",
                "quantity": 2,
                "price": 40.00,
                "tax_rate": {
                    "rate": 0.05,
                    "name": "VAT"
                }
            },
            {
                "name": "Item 2",
                "description": "Item description 2",
                "quantity": 3,
                "price": 20.00,
                "tax_rate": {
                    "rate": 0.05,
                    "name": "VAT"
                }
            }
        ]
    }
]'
```
  
  
A successful request will queue the invoice to be sent and will return the following response:

```json
{
    "detail": "1 Tasks queued"
}
```

# Authentication

> To authorize, use this code:

```shell
curl --location --request GET 'api_endpoint_here' 
--header 'Authorization: Api-Key <api_key>'
```


> Make sure to replace `<api_key>` with your API key.

Wafeq uses API keys to allow access to the API.

Wafeq expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Api-Key <api_key>`

{% callout %}
You must replace <code>&lt;api_key&gt;</code> with your organizations API key.
{% /callout %}

# E-Invoicing API

This API sends ZATCA-compliant e-invoices in large volumes.  
- It will send invoices by email in English or Arabic.  
- The email will include the invoice in PDF format that is compliant with ZATCA and that includes the QR code  
  
Note: To use this API, you must have an account on Wafeq and have configured your organization details (name, logo, address, VAT number). These details will be used when generating the QR code.

## Send bulk invoices

```shell
# Below is a sample request to send an e-invoice
curl--location --request POST 'https://api.wafeq.com/api/v1/invoices/bulk_send/' \
--header 'Authorization: Api-Key <api_key>' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "reference": "X-1234",
        "invoice_number": "INV-1234",
        "invoice_date": "2021-01-01",
        "currency": "AED",
        "language": "en",
        "tax_amount_type": "TAX_INCLUSIVE",
        "contact": {
            "name": "Jane Doe",
            "address": "Jane Doe Residency, Dubai, UAE",
            "email": "jane.doe@example.com"
        },
        "channels": [
            {
                "medium": "email",
                "data": {
                    "subject": "Invoice X from the Company of John Doe",
                    "message": "<p>Please find attached your invoice.</p>",
                    "recipients": {
                        "to": [
                            "jane.doe@example.com"
                        ],
                        "cc": [],
                        "bcc": []
                    }
                }
            }
        ],
        "line_items": [
            {
                "name": "Item 1",
                "description": "Item description 1",
                "quantity": 2,
                "price": 40.00,
                "tax_rate": {
                    "rate": 0.05,
                    "name": "VAT"
                }
            },
            {
                "name": "Item 2",
                "description": "Item description 2",
                "quantity": 3,
                "price": 20.00,
                "tax_rate": {
                    "rate": 0.05,
                    "name": "VAT"
                }
            }
        ]
    }
]'
```

> The above command returns JSON structured like this:

```json
{
    "detail": "1 Tasks queued"
}
```

This endpoint is used to send bulk invoices which are ZATCA compliant.

### HTTP Request

`POST https://api.wafeq.com/api/v1/invoices/bulk_send/`

### Request Body

Attributes | Type | Required | Description
--------- | ---- | -------- | ----------- 
reference | `string` | `False` | Invoice reference code
invoice_number | `string` | `True` | Invoice number
invoice_date | `string` | `True` | Invoice date in `YYYY-MM-DD` e.g. `2021-01-01`
currency | `string` | `True` | Currency to be used in the invoice
language | `string` | `True` | Language in which the invoice will be generated. `en` or `ar`.
notes | `string` | `False` | Extra notes to be added at the end of the invoice
tax_amount_type | `string` | `False` | Tax type of the amounts in line items, i.e. `TAX_INCLUSIVE` or `TAX_EXCLUSIVE`
contact | [`Contact`](#contact) | `True` | Contact details to be displayed on the Invoice
channels | `array[`[`Channel`](#channel)`]` | `True` | List of channels to be used to send the invoice
line_items | `array[`[`LineItem`](#lineitem)`]` | `True` | List of line items and their details


# Rounding logic

While calculating tax total and the subtotal, each individual line amount is rounded to 2 decimal places.
The method used to round is Round half up.

| Name | Amount | Tax Rate | Tax (before rounding) | Tax (after rounding) | Totals |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Line item 1 | SAR 45.30 | 5% | SAR 2.265 | SAR 2.27 |  |
| Line item 2 | SAR 33.45 | 10% | SAR 3.345 | SAR 3.35 |  |
| Total Tax Amounts |  |  |  | SAR 5.62 |  |
| **Total** | SAR 78.75 |  |  |  | SAR 84.37 |

