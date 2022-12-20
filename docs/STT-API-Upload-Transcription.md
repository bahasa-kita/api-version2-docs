# **Speech to Text Transcription API**
API STT Documentation is guidance for communicate with bahasakita speech recognition service.

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
![Diagram Process](/asset/stt-transcript.png "Diagram Process")
 
## **Upload STT API**
  In order to use this API, you required to create account by registering yourself.

### **"How to Use" Flow**
  1. Get your token with [Our API](./Auth-API.md) 
  2. Upload the audio you want to transcribe and send the other information like target languange of transcribe to `endpoint-post`. 
  3. Wait for response, it will give you uuid code and message status of transcripting process.
  4. If you want know percentage of your transcripting process, you can get it from `endpoint-get` when message status is `'inprogress'`.
  5. If you want get the result of transcribe, you can get it from `endpoint-get` when message status is `'process success'`.
   
### **Host:**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)

### **Endpoint**
 Post data: `/v2/prod/stt/async/upload` \
 Get data: `/v2/prod/stt/async/content/<uuid>`

### **Method:**
  `POST` - When you want upload file and send the other information \
  `GET` - When you want get percentage of process or get the result of transcribe 

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
  | target_language | String | Target language of transcribe with language code format like `'id'` for Indonesia |
  | diarization | Boolean | Transcripting process will include diarization process if `True` |
  | subtitle_cc | Boolean | Transcripting process will also create subtitle file if `True` |
  | priority | String | `'high'` or `'reguler'`, if empty priority it will be `'reguler'` |

#### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | target_language | String | Target language of transcribe |
  | diarization | Boolean | Including Process Diarization or Not |
  | subtitle_cc | Boolean | Creating subtitle file or Not |
  | uuid | String | Used for get the result of transcribe |
  | message_status | String | `'process success'`, `'process failed'`, `'process inquery'` or `'inprogress'` message |

#### **Example Response :**
```json
{
  "bk":
    "data":{ 
      "target_language":<string>,
      "diarization":<boolean>,
      "subtitle_cc":<boolean>,
      "uuid":<string>
    },
    "message_status" :  <string> 
}
```
### **Sample Post in Python:**
```python
import requests
import os
import time
from argparse import ArgumentParser

def main():
    parser = ArgumentParser()
    parser.add_argument("-f", "--file", dest="filename",
                help="write report to FILE", metavar="FILE")
    parser.add_argument("-l", "--language", dest="target_language",
                default="id", help="set target language of transcribe")
    parser.add_argument("-d", "--diarization", dest="diarization",
                default=False, help="including diarization process")
    parser.add_argument("-s", "--subtitle", dest="subtitle_cc",
                default=True, help="create subtitle file")
    parser.add_argument("-p", "--priority", dest="priority",
                default="reguler", help="priority of transcribe task")

    args = parser.parse_args()
    
    if not os.path.exists(args.filename) :
        parser.print_help()
        return

    url_post = "https://api.bahasakita.co.id/v2/prod/stt/async/upload"
    
    headers={'Authorization': 'Bearer <your token>'}
    
    file = {
        'file': open(args.filename,'rb')
    }
    
    data = {
        'target_language': args.target_language,
        'diarization': args.diarization,
        'subtitle_cc': args.subtitle_cc,
        'priority': args.priority
    }

    post_response = requests.request("POST", url_post, headers=headers, files=file, data=data).json()
    print(post_response)
    
if __name__ == "__main__":
    main()
```

### **Request - GET** (Retrieve Result)
#### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Authorization | `Bearer token` |
#### **Response when Message Status `'inprogress'`**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | message status | String | `inprogress` message |
  | progress status | String | Percentage progress or null |

#### **Example Response :**
```json
{
    "bk": {
        "message_status": "inprogress",
        "progress": <percentage> or null
    }
}
```

#### **Response when Message Status `'process success'`**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | target_language | String | Target language of transcribe |
  | uuid | String | Used for get the result of transcribe |
  | total_segments | Integer | Total segment of transcribe result |
  | transcripts | List | Result of transcribe like text, start time, end time, speaker, etc. |
  | subtitle_cc | String (base64) | Subtitle file in base64 format, you must decode it |
  | message status | String | `'process success'`, `'process failed'`, `'process inquery'` or `'inprogress'` message |

#### **Example Response :**
```json
{
  "bk": {
    "data": {
      "target_language": <string>,
        "uuid": <string>,
        "total_segments": <int>, 
        "transcripts":[
          {
            "text":"halo nama saya",
            "text_confidence":0.9,
            "start_time":<float>,
            "end_time":<float>,
            "speaker": "speaker_01", 
            "words":[
              {
                "score":0.9,
                "word":"halo",
                "from":0.00,
                "length":0.1
              },
            ]
          }
        ],
      "subtitle_cc": <string> (base64)
    },
    "message_status": <string>
  }
}
```



### **Sample Get in Python:**
```python
import requests
import base64
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
    
    headers={'Authorization': 'Bearer <your token>'}
    
    uuid_code = args.uuid    
    url_get = f"https://api.bahasakita.co.id/v2/prod/stt/async/content/{uuid_code}"

    while True:
        get_response = requests.request("GET", url_get, headers=headers).json()
        print(get_response)
        if "bk" in get_response and "data" in get_response["bk"] and "transcripts" in get_response["bk"]["data"]:
            if "subtitle_cc" in get_response["bk"]["data"] and get_response["bk"]["data"]["subtitle_cc"] is None:
                pass
            else:
                with open("subtitle.srt", "wb") as srt:
                    srt.write(base64.decodebytes(str.encode(get_response["bk"]["data"]["subtitle_cc"])))
            break
        time.sleep(1.0)


if __name__ == "__main__":
    main()
```