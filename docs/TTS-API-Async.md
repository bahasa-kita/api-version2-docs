# **Text to Speech API**

Asynchronous Text-to-Speech API Documentation is guidance for communicate with bahasakita speech synthesize service, without having to wait for synthesis processing to complete and return an audio url path result. The text in each asynchronous request is limited to 3000 characters.


#### **Table of Contents**
  - [General API Information](#general-api-information)
  - [Tech Stack](#tech-stack)
  - `API`
    - [Asynchronous TTS API](#asynchronous-tts-api) 
    - [Retrieving Result](#retrieving-result-api)

## **General API Information**
  - The base endpoint is: 
    - [https://api.bahasakita.co.id](https://api.bahasakita.co.id) for [REST](https://restfulapi.net/)
     - endpoints return can be JSON object or audio content-type.

## **Tech Stack**
  - **[REST](https://restfulapi.net/)**
  
 
## **Asynchronous TTS API**
  In order to use this API, you required to create account by registering yourself.

### **"How to Use" Flow**
  1. Get your token with [Our API](./Auth-API.md), and set in the request header for authorization.  
  2. Send your message in `plantext` or `SSML`. How to use SSML, you can check this link [SSML](https://github.com/bahasa-kita/ssml-doc) 
  3. You will get return an audio url path and an expired date response indicating when the audio can still be retrieved.
   
### **Host:**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)

### **Endpoint**
  `/v2/prod/tts/async`

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
  | expired | string | Expired Date of Text-To-Speech Audio |
  | path | String | Path url of Generated Audio |
  | text | String | Text to be generated |
  | language | String | Language for Synthesize |
  | volume | Int | Set volume output synthesize |
  | pitch | String |Set pitch output synthesize |
  | speed | Float | Set speed output synthesize |

###### **Example Response:**
```json
{ 
    "bk": {
        "data": { 
            "text":"selamat datang di rumah kami ini.",
            "path":"https://api.bahasakita.co.id/v2/prod/tts/async/content/330576b24f47dd7501a08af9ebcd2e7b4cdd7f2d30a67b245f291359d31687f353d0478d9540367cbeb039a854cac080.wav",
            "expired": "2022-07-22T04:11:01Z",
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

## **Retrieving Result API**
  In order to use this API, you required to create account by registering yourself. 
  Using `urlpath` object we previously from [async TTS API](#asynchronous-tts-api) response.  

### **"How to Use" Flow**
  1. Get your token with [Our API](./Auth-API.md), and set in the request header for authorization.  
  2. Get audio result by accessing an audio URL path.
  3. It's sucessful, you can play audio result. 

### **Host:**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)

### **Endpoint**
  `/v2/prod/tts/async/content/<uuid>`

### **Method:**
  `GET`

### **Request**
#### **Headers**
  | Name | Format |
  | ------ | ------ |
   | Authorization | `Bearer token` |

#### **Example Request**
This is an example of get TTS audio from uuid.

```json
GET https://api.bahasakita.co.id/v2/prod/tts/async/content/<uuid>

```
### **Responses**
  | Status | Meaning | Description |
  | ------ | ------ | ------ |
  | 200 | OK, Audio Available or Audio Synthesize | processing succes or processing synthesize |
  | 400 | Request Failed  | a processing error occurred|
  | 404 | Not Found  | audio not found, maybe because it hasn't been synthesized yet|
  | 410 | Gone  | audio has passed expired date, so it's time to generate again |

##### **Detail Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | message_status | String | `'success'`, `'failed'`, `'inquery'` or `'inprocess'` message |
  | Audio | String(Base64) | Return audio(base64) |
  | quota | Int | Remaining quota info |
  | progress | Float | Progress percentage |

##### **Example Response (Inprogress) :**
```json
{
    "bk": {
        "progress": <float>, // progress percentage 
        "message_status": <string>, // status response 
    }
}
```

##### **Example Response :**
```json
{
    "bk": {
        "audio": <base64>,
        "quota": <int>,
        "message_status": <string>, // status response 
    }
}
```

### **Sample Call in Python:**
```python
import requests
import time
import json


url = "https://api.bahasakita.co.id/v2/prod/tts/async"
token ="<your_token>" # set your token


def main():
    text = "Halo saya TTS bahasakita"
    label = "text"
    language = "id"
    volume = "1"
    pitch = "normal"
    speed = "1.0"
    
    urlpath, expired_date = tts_async(text, label, language, volume, pitch, speed)
    if urlpath is not None :
        print("expired-date audio :", expired_date)
        audio = get_audio(urlpath)
        audiofile = "audiofile.wav"
        try:
            with open(audiofile, "wb") as f:
                f.write(audio)
                f.close()
            #os.system("play "+audiofile) #install sox tools for play recording audio .
        except:
            print("audio failed")
    else:
        print(" TTS failed")

def tts_async(text : str, label : str, language : str, volume : str, pitch : str, speed : str):
    path = None
    expired = None

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
        body = json.loads(response.text)        
        path = body['bk']['data']['path']
        expired = body['bk']['data']['expired']
    else:
        print(response.text)

    return path,expired

def get_audio(path : str):
    result = None
    headers={'Authorization': 'Bearer '+token+''}        
    while True:
      url_get = f"https://api.bahasakita.co.id/v2/prod/tts/async/content/{uuid}"
        response = requests.get(path, headers=headers)
        if response.status_code == 200:
            if "message_status" in response.json()["bk"] and response.json()["bk"]["message_status"] == "success":
                    audio = base64.b64decode(response.json()["bk"]["audio"])
                    break
                else:
                    print(json.dumps(response.json(), indent=4))
                    continue
        elif response.status_code == 401:
            print("invalid token")    
            break
        elif response.status_code == 400:
            print("processing failed")
            break
        elif response.status_code == 410:
            print("audio url path expired")    
            break
        else:
            print("unidentified")
            break

    return result


if __name__ == "__main__":
    main()
```
