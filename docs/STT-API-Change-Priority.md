# **Speech to Text (Change Priority) API**
API STT Documentation is guidance for communicate with bahasakita speech recognition service. This part about how to change transcription priority from file upload request.

## **Table of Contents**
  - [General API Information](#general-api-information)
  - [Tech Stack](#tech-stack)
  - [Diagram Process](#diagram-process)
  - [Change Priority](#change-priority) 

### **General API Information**
  - The base endpoint is: 
    - [https://api.bahasakita.co.id](https://api.bahasakita.co.id) for [REST](https://restfulapi.net/)
     - All endpoints return JSON object.

### **Tech Stack**
  - **[REST](https://restfulapi.net/)**

### **Diagram Process**
  ![Diagram Process](/asset/stt-change-priority.png "Diagram Process")
 
 
### **Change Priority**
  In order to use this API, you required to create account by registering yourself.

### **"How to Use" Flow**
  1. Get your token with [Our API](./Auth-API.md) 
  2. Upload metadata to change priority. 
  3. Wait for response, and engine will change priority.
   
### **Host:**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)

### **Endpoint**
  `/v2/prod/stt/changePriority`

### **Method:**
  `POST`

### **Data Structure**
#### **Request**
##### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Content-Type | `application/json` |
  | Authorization | `Bearer token` |

##### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | uuid | String | `Unique ID` is obtained from the response when requesting file upload |
  | priority | String | setting priority `high` or `reguler` |

##### **Example Request**
```json
{
    "bk":{
        "data": {
            "uuid" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "priority": "high"
        }
    }
}
```
#### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | uuid | String | `Unique ID` is obtained from the response when requesting file upload |
  | priority | String | setting priority `high` or `reguler` |
  | message_status | String | `success` or `error` message|

##### **Example Response**
```json
{
    "bk":{
        "data": {
            "uuid": "xxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "priority": "high"
        },
        "message_status": "success or error message",
    }
}

```

### **Sample Call in Python**
```python
import requests
import sys
from argparse import ArgumentParser


def main():
    parser = ArgumentParser()
    parser.add_argument("-u", "--uuid", dest="uuid",
                help="uuid to set new priority")
    parser.add_argument("-u", "--priority", dest="priority",
                help="set new priority")

    args = parser.parse_args()
    if args.uuid is not None or args.priority is not None:
        parser.print_help()
        return

    url = "https://api.bahasakita.co.id/v2/prod/stt/changePriority"
    headers = {
        "Authorization': 'Bearer <your token>"
    }

    data = {
        "bk": {
            "data": {
                "uuid": args.uuid,
                "priority": args.priority
            }
        } 
    }

    response = requests.request("POST", url,headers=headers, json=data)
    if "bk" in response:
        print(response.json())
    else:
        print(response.text)


if __name__ == "__main__":
    main()
```