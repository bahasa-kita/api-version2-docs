
# AUTH API DOCUMENTATION
Auth API Documentation is instructions fo creating an user authorization that will be used to take advantage of all bahasakita services.

## **Table of Contents**
  - [General API Information](##general-api-information)
  - [Tech Stack](##tech-stack)
  - `API`
    - [Get Token Service API](##get-token-api)
  - [Sample in Python](##sample-in-python)
  
## **General API Information**
  - The base endpoint is: 
    - [https://dikte.in](https://dikte.in/#/regis?) for Register Account
    - [https://api.bahasakita.co.id](https://api.bahasakita.co.id) for [REST](https://restfulapi.net/)
    - endpoints return is JSON format.

## **Tech Stack**
  - **[REST](https://restfulapi.net/)**

## **Get APIKEY Session Token**
  In order to use our services, you need to get the token with your registered account.

### **"How to Use" Flow**
  1. Register your account at [dikte.in](https://dikte.in/#/regis?), You can get tokens in the console application or
  2. Send request to the endpoint with required fields. 
  3. Wait for response, if `success` you can use the token to use our API service.

### **Host:**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)

### **Endpoint**
  `/v2/prod/tts/getTokenServices`

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
  | token_data_stt | String | data for stt services, contains `expires_date`, `quota`, `token`, and `type` |
  | token_data_tts | String | data for tts services, contains `expires_date`, `quota`, `token`, and `type` |
  | token_data_text_translate | String | data for text-translate services, contains `expires_date`, `quota`, `token`, and `type` |
  | token_data_audio_translate | String | data for audio-tanslate services, contains `expires_date`, `quota`, `token`, and `type` |

#### **Example Response:**
```json
    {
        "bk": {
            "token_data_stt": {
                "expire_date": "YYYY-MM-DDTHH:mm:ss",
                "quota": <int>,
                "token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "type": "STT"
            },
            "token_data_tts": {
                "expire_date": "YYYY-MM-DDTHH:mm:ss",
                "quota": <int>,
                "token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "type": "TTS"
            },
            "token_data_text_translate": {
                "expire_date": "YYYY-MM-DDTHH:mm:ss",
                "quota": <int>,
                "token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "type": "text-translate"
            },
            "token_data_audio_translate": {
                "expire_date": "YYYY-MM-DDTHH:mm:ss",
                "quota": <int>,
                "token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "type": "audio-translate"
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
    url = f"https://api.bahasakita.co.id/v2/prod/getTokenServices"

    auth = base64.b64encode(f"{str(username)}:{str(password)}".encode("utf-8")).decode("ascii")
    headers_apikey = {
        "Authorization": f"Basic {auth}"
    }

    response_get = requests.request("GET", url, headers=headers_apikey)
    if response_get.status_code == 200:
        token_stt = response_get.json()["bk"]["token_data_stt"]
        token_tts = response_get.json()["bk"]["token_data_tts"]
        token_text_translate = response_get.json()["bk"]["token_data_text_translate"]
        token_audio_translate = response_get.json()["bk"]["token_data_audio_translate"]

if __name__ == "__main__":
    main()
```
