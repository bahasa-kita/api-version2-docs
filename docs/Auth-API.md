# AUTHORIZATION APIs
The "Authorization APIs documentation" is instructions for create an user authorization, that will be used to take advantage of All Bahasakita services.

## **Table of Contents**
  - [General API Information](##general-api-information)
  - [Tech Stack](##tech-stack)
  - `API`
    - [Get Token Service API](##get-token-api)
  - [Sample in Python](##sample-in-python)
  
## **General API Information**
  - The base endpoint is: 
    - [https://dikte.in](https://dikte.in/#/regis?) for Register Account
    - [https://api.dikte.in](https://api.dikte.in) for [REST](https://restfulapi.net/)
    - endpoints return is JSON format.

## **Tech Stack**
  - **[REST](https://restfulapi.net/)**

## **Get APIKEY Session Token**
  In order to use our services, you need to get the token with your registered account.

## **How to Use: Step by Step**
  - Register your account at [dikte.in](https://dikte.in/#/regis?), You can get tokens in the console application 
  
    Or
  
  - Send `your credentials` with request `GET`to the endpoint for get token services.
  - Wait for the response, if status is `success`. You will obtain tokens for use with the Cognitive AI services.

### **Host:**
  [https://api.dikte.in](https://dikte.in)

### **Endpoint**
  `/v2/prod/getTokenServices`

### **Method:**
  `GET`

### **Request**
#### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Authorization | `Basic <Base64(email:password)>` |

### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | message_status | String | `success` or `error` message|
  | diktein_credits_token | String | data for speech to text services, contains `expires_date`, `quota`, `token`, and `type` |

#### **Example Response:**
```json
    {
        "bk": {
            "diktein_credits_token": {
                "expire_date": "YYYY-MM-DDTHH:mm:ss",
                "quota": <int>,
                "token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "type": "diktein-credits"
            },
            "message_status": "success"
        }
    }
```

## **Sample in Python**
```python
import requests
import json
import base64


def main():
    url = f"https://api.dikte.in/v2/prod/getTokenServices"

    auth = base64.b64encode(f"{str(username)}:{str(password)}".encode("utf-8")).decode("ascii")
    headers_apikey = {
        "Authorization": f"Basic {auth}"
    }

    response_get = requests.request("GET", url, headers=headers_apikey)
    if response_get.status_code == 200:
        token = response_get.json()["bk"]["diktein_credits_token"]["token"]

if __name__ == "__main__":
    main()
```
