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
  | source_language | String | Source language of audio with language code format like `'id'` for Indonesia. Default value is `detect`, if `source_language` not set or `source_language` is `detect` system will detect languages |
  | target_language | String | Target language of transcribe with language code format like `'id'` for Indonesia |
  | diarization | Boolean | Transcripting process will include diarization process if `True` |
  | subtitle_cc | Boolean | Transcripting process will also create subtitle file if `True` |
  | check_summary | Boolean | Summary process will also create if `True` |
  | max_sentence_summary | Int | Total sentence for summary |
  | title | String | Topics or Keywords or Title for summary |
  | priority | String | `'high'` or `'reguler'`, if empty priority it will be `'reguler'` |

##### **Language List**
  | Code-Language | Language |
  | ------ | ------ |
  | detect | Language Detect |
  | en | Inggris / English |
  | zh | Cina / Chinese |
  | de | Jerman / German |
  | es | Spanyol / Spanish |
  | ru | Rusia / Russian |
  | ko | Korea / Korean |
  | fr | Perancis / French |
  | ja | Jepang / Japanese|
  | pt | Portugis / Portuguese |
  | tr | Turki / Turkish |
  | pl | Polandia / Polish |
  | ca | Katalan / Catalan |
  | nl | Belanda / Dutch |
  | ar | Arab / Arabic |
  | sv | Swedia / Swedish |
  | it | Italia / Italian |
  | id | Indonesia / Indonesian |
  | hi | India / Hindi |
  | fi | Finlandia / Finnish |
  | vi | Vietnam / Vietnamese |
  | he | Ibrani / Hebrew |
  | uk | Ukraina / Ukraininan|
  | el | Yunani / Greek |
  | ms | Malay |
  | cs | Ceko / Czech |
  | ro | Romania / Romanian |
  | da | Dansk / Danish |
  | hu | Hungaria / Hungarian |
  | ta | Tamil |
  | no | Norwegia / Norwegian |
  | th | Thai |
  | ur | Urdu |
  | hr | Kroasia / Croatian |
  | bg | Bulgaria / Bulgarian |
  | lt | Lithuania / Lithuanian|
  | ml | Malayalam |
  | cy | Welsh |
  | sk | Slovakia / Slovak |
  | te | Telugu |
  | lv | Latvia / Latvian |
  | bn | Benggala / Bengala |
  | sr | Serbia / Serbian |
  | az | Azerbaijani |
  | sl | Slovenia / Slovenian |
  | kn | Kanada / Kannada|
  | et | Estonia / Estonian |
  | mk | Makedonia / Macedonian |
  | eu | Basque |
  | is | Islandia / Icelandic |
  | hy | Armenia / Armenian |
  | ne | Nepali |
  | bs | Bosnia / Bosnian |
  | kk | Kazakh |
  | sq | Albania / Albanian |
  | sw | Swahili |
  | gl | Galicia / Galician |
  | mr | Marathi |
  | pa | Punjabi |
  | si | Sinhala |
  | km | Khmer |
  | sn | Shona |
  | yo | Yoruba |
  | so | Somalia / Somali |
  | af | Afrikaans |
  | oc | Occitan |
  | ka | Georgia / Georgian |
  | be | Belarusia / Belarusian |
  | tg | Tajik |
  | sd | Sindhi |
  | gu | Gujarat |
  | am | Amharik / Amharic |
  | yi | Yiddish |
  | lo | Lao |
  | uz | Uzbek |
  | fo | Faroe / Faroese |
  | ht | Kreol Haiti / Haiti Creol|
  | ps | Pashto |
  | tk | Turkmenistan / Turkmen |
  | nn | Nynorsk |
  | mt | Malta / Maltese|
  | sa | Sansekerta / Sanskrit |
  | lb | Luksemburg / Luxembourgish |
  | my | Myanmar |
  | bo | Tibet |
  | tl | Tagagalog |
  | as | Assam |
  | tt | Tatar |
  | ln | Lingala |
  | ha | Hausa |
  | ba | Bashkir |
  | jw | Jawa / Javanese |
  | su | Sunda / Sundanese |

#### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | source_language | String | Source language of audio |
  | target_language | String | Target language of transcribe |
  | diarization | Boolean | Including Process Diarization or Not |
  | subtitle_cc | Boolean | Creating subtitle file or Not |
  | summary | Boolean | Creating summary or Not |
  | uuid | String | Used for get the result of transcribe |
  | message_status | String | `'success'`, `'failed'`, `'inquery'` or `'inprogress'` message |
  | quota | Int | Your quota balance |

#### **Example Response :**
```json
{
    "bk": {
        "data":{ 
            "source_language": <string>,
            "target_language": <string>,
            "diarization": <boolean>,
            "subtitle_cc": <boolean>,
            "check_summary": <boolean>,
            "uuid": <string>,
            "quota": <int>
        },
        "message_status": <string>
    }
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
    parser.add_argument("-sl", "--source-language", dest="source_language",
                default="id", help="source language of audio")
    parser.add_argument("-tl", "--language", dest="target_language",
                default="id", help="set target language of transcribe")
    parser.add_argument("-d", "--diarization", dest="diarization",
                default=False, help="including diarization process")
    parser.add_argument("-s", "--subtitle", dest="subtitle_cc",
                default=True, help="create subtitle file")
    parser.add_argument("-sm", "--summary", dest="summary",
                default=True, help="create subtitle file")
    parser.add_argument("-t", "--title", dest="titlle",
                default=True, help="create subtitle file")
    parser.add_argument("-m", "--max-sentence", dest="max_sentence",
                default=True, help="create subtitle file")
    parser.add_argument("-p", "--priority", dest="priority",
                default="reguler", help="priority of transcribe task")

    args = parser.parse_args()
    if not os.path.exists(args.filename) :
        parser.print_help()
        return

    url_post = "https://api.bahasakita.co.id/v2/prod/stt/async/upload"
    headers={"Authorization': 'Bearer <your token>"}

    file = {
        "file": open(args.filename, "rb")
    }

    data = {
        "source_language": args.source_language,
        "target_language": args.target_language,
        "diarization": args.diarization,
        "subtitle_cc": args.subtitle_cc,
        "check_summary": args.summary,
        "title": args.title,
        "max_sentence_summary": args.max_sentence,
        "priority": args.priority
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

#### **Response when Message Status `'failed'` or `'inquery'`**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | message status | String | status process |

#### **Example Response (Inquery or Failed) :**
```json
{
    "bk": {
        "message_status": "inquery"
    }
}
```

#### **Response when Message Status `'inprogress'` or `'success'`**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | source_language | String | Source language of audio |
  | target_language | String | Target language of transcribe |
  | uuid | String | Used for get the result of transcribe |
  | duration | Int | Information duration audio (second), information will be displayed if status is `success` |
  | total_segments | Integer | Information total segment of transcribe result, information will be displayed if status is `success` |
  | transcripts | List | Result of transcribe like text, start time, end time, speaker, etc. |
  | summary | List | Result summary, null string if summary false, data will be displayed if status is `success` |
  | subtitle_cc | String (base64) | Subtitle file in base64 format, you must decode it, information will be displayed if status is `success` |
  | quota | Int | Remaining quota info, information will be displayed if status is `success` |
  | message status | String | `'success'`, `'inprogress'` message |
  | progress | Float | Percentage progress or null, information will be displayed if status is `inprogress` |

#### **Example Response (In-Progress) :**
```json
{
    "bk": {
        "uuid": <string>,
        "data": {
            "source_language": <string>,
            "target_language": <string>,
            "transcripts": [
                {
                    "text":"halo nama saya",
                    "start_time":<float>,
                    "end_time":<float>,
                    "speaker": "speaker_01",
                }
            ]
        },
        "progress": <float>,
        "message_status": "inprogress"
    }
}
```

#### **Example Response (Success) :**
```json
{
    "bk": {
        "data": {
            "source_language": <string>,
            "target_language": <string>,
            "uuid": <string>,
            "duration": <int>,
            "total_segments": <int>, 
            "transcripts": [
                {
                    "text":"halo nama saya",
                    "start_time":<float>,
                    "end_time":<float>,
                    "speaker": "speaker_01",
                }
            ],
            "subtitle_cc": <string> (base64),
            "summary": <str>
        },
        "quota": <int>,
        "message_status": "success"
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
    
    headers={"Authorization": "Bearer <your token>"
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
