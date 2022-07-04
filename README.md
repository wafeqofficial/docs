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



