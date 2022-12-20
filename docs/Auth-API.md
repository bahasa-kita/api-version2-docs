
# AUTH API DOCUMENTATION
Auth API Documentation is instructions fo creating an user authorization that will be used to take advantage of all bahasakita services.


#### **Table of Contents**
  - [General API Information](#general-api-information)
  - [Tech Stack](#tech-stack)
  - `API`
    - [Get STT Token API](#get-stt-token-api) 
    - [Get TTS Token API](#get-tts-token-api)
  
## **General API Information**
  - The base endpoint is: 
    - [https://console.bahasakita.co.id](https://console.bahasakita.co.id/login) for Register Account
    - [https://api.bahasakita.co.id](https://api.bahasakita.co.id) for [REST](https://restfulapi.net/)
    - endpoints return can be JSON object or audio content-type.

## **Tech Stack**
  - **[REST](https://restfulapi.net/)**

## **Get STT Token API**
  In order to use STT service, you need to get the token with your registered account.

### **"How to Use" Flow**
  1. Register your account at [console.bahasakita.co.id](https://console.bahasakita.co.id/login), You can get tokens in the console application or
  2. Send request to the endpoint with required fields in below. 
  3. Wait for response, if `success` you can use the token to use our API service.


### **Host:**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)

### **Endpoint**
  `/v2/prod/getToken`

### **Method:**
  `POST`

### **Request**
##### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Authorization | `Basic Base64(email:password)` |

### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | message | String | `success` or `error` message|
  | token | String | |

##### **Example Response:**
```json
    {"bk":{
        "messsage":"success",
        "data" : {
            "token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
            }
        }
    }  
```


## **Get TTS Token API**
  In order to use TTS service, you need to get the token with your registered account.

### **"How to Use" Flow**
  1. Register your account at [console.bahasakita.co.id](https://console.bahasakita.co.id/login), You can get tokens in the console application or
  2. Send request to the endpoint with required fields. 
  3. Wait for response, if `success` you can use the token to use our API service.


### **Host:**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)

### **Endpoint**
  `/v2/prod/getToken`

### **Method:**
  `POST`

### **Request**
##### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Authorization | `Basic Base64(email:password)` |

### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | message | String | `success` or `error` message|
  | token | String | |

##### **Example Response:**
```json
    {"bk":{
        "messsage":"success",
        "data" : {
            "token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
            }
        }
    }  
```
