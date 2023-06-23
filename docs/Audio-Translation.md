# **Audio Translation API**
API Audio Translation Documentation is guidance for communicate with bahasakita speech recognition service.

#### **Table of Contents**
  - [General API Information](#general-api-information)
  - [Tech Stack](#tech-stack)
  - [Diagram Process](#diagram-process)
  - [Audio-Translation API](#audio-translation-api) 

## **General API Information**
  - The base endpoint is: 
    - [https://stagapi.bahasakita.co.id](https://stagapi.bahasakita.co.id) for [REST](https://restfulapi.net/)
     - All endpoints return JSON object.

## **Tech Stack**
  - **[REST](https://restfulapi.net/)**  

## **Diagram Process**
 ![Diagram Process](/asset/Audio-Translation.png "Diagram Process") 
 
## **Audio Translation API**
  In order to use this API, you required to create account by registering yourself.

### **"How to Use" Flow**
  1. Get your token with [Our API](./Auth-API.md) 
  2. Send the text you want to translate and send the other information like source language and target languange of translation to `endpoint-post`. 
  3. Wait for response, it will give you uuid code and message status of translating process.
  4. If you want get the result of translation, you can get it from `endpoint-get` when message status is `'success'`.
   
### **Host:**
  [https://stagapi.bahasakita.co.id](https://stagapi.bahasakita.co.id)

### **Endpoint**
 Post data: `/v2/prod/speechtranslation` \
 Get data: `/v2/prod/speechtranslation/content/<uuid>` \
 Get History data: `/v2/prod/speechtranslation/monitoring`

### **Method:**
  `POST` - When you want upload file and send the other information \
  `GET` - When you want get percentage of process or get the result of transcribe 

### **Request - POST**
##### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Content-Type | `multipart/form-data` |
  | Authorization | `Bearer token` |

##### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | file | File | Your document file |
  | url | String | The url you want to retrieve the content |
  | source_language | String | Source language of translation like `'Indonesian'` |
  | target_language | String | Target language of translation like `'English'` |

##### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | uuid | String | UUID string  |
  | source_language | String | Source language of translation like `'Indonesian'` |
  | target_language | String | Target language of translation like `'English'` |
  | message_status | String | `'success'`, `'failed'`, `'inquery'` or `'inprocess'` message |
  | quota | Int | Your quota balance |

##### **Example Response :**
```json
{
    "bk": {
        "data": {
            "uuid": <string>, 
            "source_language": <sting>, 
            "target_language": <string>,
            "quota": <int>
        }, 
        "message_status": <string> // status response 
    }
}
```


### **Sample Post in Python :**
```python
import requests
import os
from argparse import ArgumentParser


def main():
    parser = ArgumentParser()
    parser.add_argument("-f", "--file", dest="filename",
                help="input data with documents (support: .wav, .mp3, .ogg, .mp4 (audio-video format))", metavar="FILE")
    parser.add_argument("-u", "--url", dest="url",
                help="input data with url-link (article link)")
    parser.add_argument("-sl", "--source-language", dest="source_language",
                default="id", help="set source language of translate")
    parser.add_argument("-tl", "--target-language", dest="target_language",
                default="en", help="set target language of translate")
    args = parser.parse_args()

    url_post = "https://stagapi.bahasakita.co.id/v2/prod/speechtranslation"       ## Sync process
    headers= {
        "Authorization": "Bearer <your token>"
    }
    
    if args.file:
        file = {
            "file": open(args.filename, "rb")
        }
        
        data = {
            "source_language": args.source_language, 
            "target_language": args.target_language, 
        }

        post_response = requests.request("POST", url_post, headers=headers, files=file, data=data).json()
    else:
        if args.url:
            data = {
                "url": args.url, 
                "source_language": args.source_language, 
                "target_language": args.target_language, 
            }

        post_response = requests.request("POST", url_post, headers=headers, data=data).json()

    print(post_response)


if __name__ == "__main__":
    main()
```

### **Request - GET (Get Data)**
##### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Authorization | `Bearer token` |

##### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | source_language | String | Source language of translation like `'Indonesian'` |
  | target_language | String | Target language of translation like `'English'` |
  | text_translation | String | The result of text translation |
  | audio | String(Base64) | The result of audio translation |
  | message_status | String | `'success'`, `'failed'`, `'inquery'` or `'inprocess'` message |
  | message_detail | String | Information detail process |
  | created | String | Created request translate datetime (isoformat) |
  | quota | Int | Remaining quota info |
  | progress | Float | Progress percentage |

##### **Example Response (Failed or Inquery) :**
```json
{
    "bk": {
        "message_status": <string> // status response 
    }
}
```

##### **Example Response (Inprogress) :**
```json
{
    "bk": {
        "progress": <float>, // progress percentage 
        "message_status": "inprogress", // status response 
        "meesage_detail": <string> // status process
    }
}
```

##### **Example Response :**
```json
{
    "bk": {
        "data": { 
            "source_language": <sting>, 
            "target_language": <string>, 
            "text_translation": <string>, 
            "audio": <base64>,
            "quota": <int>
        }, 
        "message_status": <string>, // status response 
        "created": "0000-00-00T00:00:00.000000" // YYYY-MM-DDTHH:mm:ss.ms
    }
}
```

### **Sample Get in Python:**
```python
import requests
import os
from argparse import ArgumentParser


def main():
    parser = ArgumentParser()
    parser.add_argument("-u", "--uuid", dest="uuid", default = None,
                        help="Write your uuid code")

    args = parser.parse_args()
    
    if args.uuid is None :
        parser.print_help()
        return
    
    headers = {
        "Authorization": "Bearer <your token>"
    }
    
    uuid_code = args.uuid    
    url_get = f"https://stagapi.bahasakita.co.id/v2/prod/speechtranslation/content/{uuid_code}"
    get_response = requests.request("GET", url_get, headers=headers).json()
    print(get_response)


if __name__ == "__main__":
    main()
```

### **Request - GET (Get History Data)**
##### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Content-Type | `multipart/form-data` |
  | Authorization | `Bearer token` |

##### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | username | String | Username |
  | history | String | List of request data history |
  | uuid | String | UUID |
  | input_type | String | Input type of data (file, url, or text) |
  | source | String | Resource data (namefile, urllink, or part-of-text) |
  | created_date | String | Time stamp for result |
  | char_length | Int | Length character of text |
  | total_usage | Int | total usage of translation for user |
  | message_status | String | `'success'`, `'failed'`, `'inquery'` or `'inprocess'` message |

##### **Example Response :**
```json
{
    "bk": {
        "data": { 
            "username" : <string>, 
            "history": [
                {
                    "uuid": <string>, 
                    "input_type": <string>, 
                    "source": <string>, 
                    "created_date": datetime.isoformat(), 
                    "char_length": <int>, 
                    "message_status": <string>
                }
            ]
        }, 
        "total_usage": <int>, 
        "message_status" : <string>
    }
}
```

### **Sample Get History in Python:**
```python
import requests
import os
from argparse import ArgumentParser


def main():    
    headers = {
        "Authorization": "Bearer <your token>"
    }
    
    uuid_code = args.uuid    
    url_get = f"https://stagapi.bahasakita.co.id/v2/prod/speechtranslation/monitoring"
    get_response = requests.request("GET", url_get, headers=headers).json()
    print(get_response)


if __name__ == "__main__":
    main()
```
