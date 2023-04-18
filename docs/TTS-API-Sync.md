# **Text to Speech API**

Synchronous Text-to-Speech API Documentation is guidance for communicate with bahasakita speech synthesize service, wait for the processing to finish. The text in each synchronous request is limited to 300 characters.


#### **Table of Contents**
  - [General API Information](#general-api-information)
  - [Tech Stack](#tech-stack)
  - [Synchronous TTS API](#synchronous-tts-api) 

## **General API Information**
  - The base endpoint is: 
    - [https://api.bahasakita.co.id](https://api.bahasakita.co.id) for [REST](https://restfulapi.net/)
     - All endpoints return JSON object.

## **Tech Stack**
  - **[REST](https://restfulapi.net/)**
  
 
## **Synchronous TTS API**
  In order to use this API, you required to create account by registering yourself.

### **"How to Use" Flow**
  1. Get your token with [Our API](./Auth-API.md), and set in the request header for authorization. 
  2. Send your message in `plantext` or `SSML`. How to use SSML, you can check this link [SSML](https://github.com/bahasa-kita/ssml-doc) 
  3. You will get the message response in the json format containing audio file into base64-encoded data.
   
### **Host:**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)

### **Endpoint**
  `/v2/prod/tts/sync`

### **Method:**
  `POST`

### **Request**
#### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Content-Type | `multipart/form-data` |
  | Authorization | `Bearer token` |

#### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | label | String | Text type `plainttext` or `ssml` |
  | file | File | Your document file to generated text |
  | url | String | The url you want to retrieve the content to generated text |
  | text | String | Text to be generated |
  | language | String | Choose Language for Synthesize |
  | volume | String | Set volume output synthesize (Volume Range Value from `1` to `10`)|
  | pitch | String | Set pitch output synthesize (Pitch Value is `x-low`, `low`, `normal`, `high`, `x-high`) |
  | speed | String | Set speed output synthesize (Speed Range Value from `0.1` to `2.0`) |

##### **Example Request with plaintext**
This is an example of generating text using the plaintext type

```text
text: selamat datang di rumah kami ini
```

##### **Example Request with SSML**
This is an example of generating text using SSML type. To understand more, you can follow this [SSML](https://github.com/bahasa-kita/ssml-doc)

```text
text: <speak> Kamu urutan <say-as interpret-as="ordinal">10</say-as> dalam baris. </speak>
```

#### **Response Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | quota | Int | Quota Balance of Text-To-Speech |
  | audio | string | Output audio in base64 format |
  | format | String | Audio extension format |
  | sample-rate | Int | Value of output audio sample-rate |
  | precision | Int | Value of output audio precision (bit-rate) |
  | channel | String | Value of output audio channel |
  | encoding | String | Value of output audio encoding |
  | text | String | Text to be generated |
  | language | String | Language for Synthesize |
  | volume | Int | Set volume output synthesize |
  | pitch | String | Set pitch output synthesize |
  | speed | Float | Set speed output synthesize |

##### **Example Response:**
```json
{
    "bk": {
        "data": {
            "text": "selamat datang.", 
            "audio": base64(audio),
            "audio_format": { 
                "format": "wav", 
                "sample-rate": 16000, 
                "precision": 16, 
                "channel": "mono", 
                "encoding":"signed-integer"
            },
            "quota": 98753
        }, 
        "config": {
            "language": "id",
            "volume": 1, 
            "pitch": "normal", 
            "speed": 1.0
        }  
    }
}

```

### **Sample Call in Python:**
```python
import requests
import json
import base64


url = "https://apidev.bahasakita.co.id/v2/prod/tts/sync"
token = "<your_token>" #set your token   


def main():
    text = "Halo saya TTS bahasakita"
    label = "text"
    language = "id"
    volume = "1"
    pitch = "normal"
    speed = "1.0"

    headers={"Authorization": f"Bearer {token}"}
    data = {
        "text": text,
        "label" : label,
        "language": lang,
        "volume": volume,
        "pitch" : pitch,
        "speed" : speed
    }

    response = requests.request("POST", url,headers=headers, data=data)
    if response.status_code == 200:
        print('audio succeed')        
        body = json.loads(response.text)
        try:
            encoded_audio = body['bk']['data']['audio']
            format_audio = body['bk']['data']['audio_format']['format']
            audio = base64.b64decode(encoded_audio) 
            audiofile = "audiofile." + format_audio
            with open(audiofile, "wb") as f:
                f.write(audio)
                f.close()
            #os.system("play "+audiofile) #install sox tools for play recording audio .
        except:
            print("audio failed")
    else:
        print(response.text)


if __name__ == "__main__":
    main()
```