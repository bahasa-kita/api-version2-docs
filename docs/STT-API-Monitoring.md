# **Speech-to-Text (Monitoring) APIs**
The `Speech-To-Text Monitoring APIs documentation` serves as a comprehensive guide for interacting with Bahasakita Cognitive AI services in monitor processing. This results including information about your status.

## **Table of Contents**
  - [General API Information](#general-api-information)
  - [Tech Stack](#tech-stack)
  - [Diagram Process](#diagram-process)
  - [User Monotoring](#user-monitoring) 

### **General API Information**
  - The base endpoint is: 
    - [https://api.bahasakita.co.id](https://api.bahasakita.co.id) for [REST](https://restfulapi.net/)
     - All endpoints return JSON object.

### **Tech Stack**
  - **[REST](https://restfulapi.net/)**

### **Diagram Process**
  ![Diagram Process](/asset/stt-user-monitor.png "Diagram Process")
 
 
### **User Monitoring**
  In order to use this API, you required to create account by registering yourself.

## **How to Use: Step by Step**
  1. Get your token with [Our API](./Auth-API.md) 
  2. Request GET To monitor information about your processes.
  3. Wait for response, this will return the information status that has been processed.
   
### **Host:**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)

### **Endpoint**
  `/v2/prod/stt/monitoring`

### **Method:**
  `Get`

### **Data Structure**
#### **Request**
##### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Authorization | `Bearer token` |

#### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | username | String | String of username |
  | inquery | List | Lists of inquery (data in queuing) |
  | process | List | Lists of process (data in process) |
  | failed | List | Lists of failed (data failed to process) |
  | uuid | Key of Object Json | uuid session process |
  | filename | String | audio name from upload |
  | duration | Float | audio duration in seconds |
  | msg_in | Int | Counting of input message from user |
  | msg_out | Int |  Counting of output message taken by user |
  | message_status | String | message status `failed` or `success` |

##### **Example Response**
```json
{
    "bk":{
        "data": {
            "username": "xxx",
            "inquery": [
                {
                    "<uuid>": {
                        "filename": "audioname",
                        "duration": <float>,
                        "created": "YYYY-MM-DD HH:mm:ss",
                        "filesize": <int - "MB">,  
                    }
                },
            ],
            "process": [
                  {
                    "<uuid>": {
                        "filename": "audioname",
                        "duration": <float>,
                        "created": "YYYY-MM-DD HH:mm:ss",
                        "filesize": <int - "MB">, 
                    }
                },
            ],
            "failed": [
                {
                    "<uuid>": {
                        "filename": "audioname",
                        "duration": <float>,
                        "created": "YYYY-MM-DD HH:mm:ss",
                        "filesize": <int - "MB">, 
                    }
                },
            ],
            "success": [
                {
                    "<uuid>": {
                        "filename": "audioname",
                        "duration": <float>,
                        "created": "YYYY-MM-DD HH:mm:ss",
                        "filesize": <int - "MB">, 
                    }
                },
            ]
        },
      "msg_in": 1,
      "msg_out": 1,
      "message_status": "success or error message",
    },
}

```

### **Sample Call in Python**
```python
import requests
import sys


def main():
    url = "https://api.bahasakita.co.id/v2/prod/stt/monitoring"
    headers = {
        "Authorization": "Bearer <your token>"
    }

    response = requests.request("GET", url,headers=headers)
    if "bk" in response:
        print(response.json())
    else:
        print(response.text)


if __name__ == "__main__":
    main()

```