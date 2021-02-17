## How to use our Payment API

### Constructing the API Call

Commerce Hub's API request consists of the [Header](#request-header) followed by the [Request Body](#request-body).

### Request Header

| Variable | Type | Length | Description/Values |
| -------- | :--: | :------------: | ------------------ |
| `Content-Type` | *string* |  | The content type. Valid Value (application/json) |
| `Client-Request-Id` | *string* |  | A client-generated ID for request tracking and signature creation, unique per request. This is also used for idempotency control. We recommend 128-bit UUID format. |
| `REQUEST_UUID` | *string* |  | Key given to merchant after boarding associating their requests with the appropriate app in Apigee. |
| `Timestamp` | *integer* |  | Epoch timestamp in milliseconds in the request from a client system. Used for Message Signature generation and time limit (5 mins). |
| `Message-Signature` | *string* |  | Used to ensure the request has not been tampered with during transmission. The Message-Signature is the Base64 encoded HMAC hash (SHA256 algorithm with the API Secret as the key.) For more information, refer to the supporting documentation on the [Developer Portal](url). |

##### Sample Header

```json
header: {
      "Content-Type": "application/json",
      "Client-Request-Id": "Client request ID goes here",
      "REQUEST_UUID": "REQUEST_UUID goes here",
      "Timestamp": "Date().getTime() goes here",
      "Message-Signature": "Message Signature goes here"
    },
```


### Request Body

The body of the transaction Request differs as per the transaction which is getting initiated. Below is the sample body for [charge](../Transactions/Charges.md) request.
</br>

##### Sample Request Body

```json
{
  "amount": {
    "total": "12.04",
    "currency": "USD"
  },
  "paymentSource": {
    "sourceType": "PaymentCard",
    "card": {
      "cardData": "4005550000000019",
      "expirationMonth": "02",
      "expirationYear": "2035",
      "securityCode": "123"
    }
  },
  "transactionDetails": {
    "captureFlag": true
  }
},
```

### Sample API Call

A standard API call to execute a Primary Transaction might look like this:

```json

{
    method: "POST",
    url: "https://test.fiservapps.com/commer-hub/payments/v1/charges",
    headers: {
      "Content-Type": "application/json",
      "Client-Request-Id": "Client request ID goes here",
      "REQUEST_UUID": "REQUEST_UUID goes here",
      "Timestamp": "Date().getTime() goes here",
      "Message-Signature": "Message Signature goes here"
    },
    body: JSON.stringify({
      {
		"amount": {
		"total": "12.04",
		"currency": "USD"
		},
		"paymentSource": {
			"sourceType": "PaymentCard",
			"card": {
				"cardData": "4005550000000019",
				"expirationMonth": "02",
				"expirationYear": "2035",
				"securityCode": "123"
			}
		},
		"transactionDetails": {
			"captureFlag": true
		}
	}
	})
}

```

