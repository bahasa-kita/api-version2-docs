# **Speech to Text Diarization API**
API STT-Diarization Documentation is guidance for communicate with bahasakita speech recognition service.

#### **Table of Contents**
  - [General API Information](#general-api-information)
  - [Tech Stack](#tech-stack)
  - [Diagram Process](#diagram-process)
  - [Upload STT API](#upload-stt-api) 

## **General API Information**
  - The base endpoint is: 
    - [https://api.bahasakita.co.id](https://api.bahasakita.co.id) for [REST](https://restfulapi.net/)
     - All endpoints return JSON object.

## **Tech Stack**
  - **[REST](https://restfulapi.net/)**  

## **Diagram Process**
  ![Diagram Process](/asset/stt-diarization.png "Diagram Process")
 
 
## **Upload STT API**
  In order to use this API, you required to create account by registering yourself.

### **"How to Use" Flow**
  1. Get your token with [Our API](./Auth-API.md) 
  2. Upload the audio you want to diarization process to `endpoint-post`. 
  3. Wait for response, it will give you uuid code and message status of diarization process.
  4. If you want get the result of diarization, you can get it from `endpoint-get`.
   
### **Host:**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)

### **Endpoint**
 Post data: `/v2/prod/diarization/async/upload` \
 Get data: `/v2/prod/diarization/async/content/<uuid>`

### **Method:**
  `POST` - When you want upload file\
  `GET` - When you want get the result of transcribe 

### **Request - POST** (Upload Audio)
#### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Content-Type | `multipart/form-data` |
  | Authorization | `Bearer token` |

#### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | file | File | Your audio file  |

#### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | uuid | String | Used for get the result of transcribe |
  | message_status | String | `'success'`, `'failed'`, `'inquery'` or `'inprogress'` message |
  | quota | Int | Your quota balance |

#### **Example Response :**
```json
{
    "bk":{
        "data": { 
            "uuid":<string>,
            "quota": <int>
        },
        "message_status" : <string> 
    }
}
```
### **Sample Post in Python:**
```python
import requests
from argparse import ArgumentParser


def main():
    parser = ArgumentParser()
    parser.add_argument("-f", "--file", dest="filename",
                help="write report to FILE", metavar="FILE")

    args = parser.parse_args()
    if not os.path.exists(args.filename) :
        parser.print_help()
        return

    url_post = "https://api.bahasakita.co.id/v2/prod/diarization/async/upload"
    headers={"Authorization": "Bearer <your token>"}
    
    file = {
        'file': open(args.filename,'rb')
    }
    
    post_response = requests.request("POST", url_post, headers=headers, files=file).json()
    print(post_response)
  

if __name__ == "__main__":
    main()
```

### **Request - GET** (Retrieve Result)
#### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Authorization | `Bearer token` |

#### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | total_segments | Integer | Total segment of transcribe result |
  | diarization | List | Result of diarization like start time, end time, and speaker |
  | qota | Int | Remaining quota info |
  | message status | String | `'success'`, `'failed'`, `'inquery'` or `'inprogress'` message |

#### **Example Response :**
```json
{
    "bk": {
        "data": {
            "total_segments": 2,
            "diarization": [
                {
                    "start_time": 2.1009375000000006,
                    "end_time": 43.697812500000005,
                    "speaker": "SPEAKER_00"
                },
                {
                    "start_time": 46.127812500000005,
                    "end_time": 50.025937500000005,
                    "speaker": "SPEAKER_00"
                }
            ]
        },
        "quota": <int>,
        "message_status": "process success"
    }
}
```


### **Sample Get in Python:**
```python
import requests
import time
from argparse import ArgumentParser


def main():
    parser = ArgumentParser()
    parser.add_argument("-u", "--uuid", dest="uuid", default = None,
                        help="Write your uuid code")

    args = parser.parse_args()
    if args.uuid is None :
        parser.print_help()
        return
    
    headers={"Authorization": "Bearer <your token>"}
    uuid_code = args.uuid    
    url_get = f"https://api.bahasakita.co.id/v2/prod/diarization/async/content/{uuid_code}"
    while True:
        get_response = requests.request("GET", url_get, headers=headers).json()
        print(get_response)
        if "bk" in get_response and "data" in get_response["bk"] and "diarization" in get_response["bk"]["data"]:
            break
        time.sleep(1.0)


if __name__ == "__main__":
    main()
```