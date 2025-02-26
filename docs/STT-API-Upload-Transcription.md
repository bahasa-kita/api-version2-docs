# **Speech to Text Transcription API**
API STT Documentation is guidance for communicate with bahasakita speech recognition service.

## **Table of Contents**
  - [General API Information](#general-api-information)
  - [Tech Stack](#tech-stack)
  - [Diagram Process](#diagram-process)
  - [Upload STT API](#upload-stt-api) 

## **General API Information**
  - The base endpoint is: 
    - [https://api.dikte.in](https://api.dikte.in) for [REST](https://restfulapi.net/)
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
  [https://api.dikte.in](https://api.dikte.in)

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
  | resume | Boolean | Resume process will also create if `True` |
  | wordcloud | Boolean | Wordcloud process will also create if `True` |

#### **Language List**
  | Code-Language | Language |
  | ------ | ------ |
  | detect | Language Detect |
  | id | Indonesia / Indonesian |
  | en | Inggris / English |
  | af | Afrikaans |
  | am | Amharik / Amharic |
  | ar | Arab / Arabic |
  | as | Assam |
  | az | Azerbaijani |
  | ba | Bashkir |
  | be | Belarusia / Belarusian |
  | bg | Bulgaria / Bulgarian |
  | bn | Benggala / Bengala |
  | bo | Tibet |
  | bs | Bosnia / Bosnian |
  | ca | Katalan / Catalan |
  | cs | Ceko / Czech |
  | cy | Welsh |
  | da | Dansk / Danish |
  | de | Jerman / German |
  | el | Yunani / Greek |
  | es | Spanyol / Spanish |
  | et | Estonia / Estonian |
  | eu | Basque |
  | fi | Finlandia / Finnish |
  | fo | Faroe / Faroese |
  | fr | Perancis / French |
  | gl | Galicia / Galician |
  | gu | Gujarat |
  | ha | Hausa |
  | he | Ibrani / Hebrew |
  | hi | India / Hindi |
  | hr | Kroasia / Croatian |
  | ht | Kreol Haiti / Haiti Creol|
  | hu | Hungaria / Hungarian |
  | hy | Armenia / Armenian |
  | is | Islandia / Icelandic |
  | it | Italia / Italian |
  | ja | Jepang / Japanese|
  | jw | Jawa / Javanese |
  | ka | Georgia / Georgian |
  | kk | Kazakh |
  | km | Khmer |
  | kn | Kanada / Kannada|
  | ko | Korea / Korean |
  | lb | Luksemburg / Luxembourgish |
  | ln | Lingala |
  | lo | Lao |
  | lt | Lithuania / Lithuanian|
  | lv | Latvia / Latvian |
  | mk | Makedonia / Macedonian |
  | ml | Malayalam |
  | mr | Marathi |
  | ms | Malay |
  | mt | Malta / Maltese|
  | my | Myanmar |
  | ne | Nepali |
  | nl | Belanda / Dutch |
  | nn | Nynorsk |
  | no | Norwegia / Norwegian |
  | oc | Occitan |
  | pa | Punjabi |
  | pl | Polandia / Polish |
  | ps | Pashto |
  | pt | Portugis / Portuguese |
  | ro | Romania / Romanian |
  | ru | Rusia / Russian |
  | sa | Sansekerta / Sanskrit |
  | sd | Sindhi |
  | si | Sinhala |
  | sk | Slovakia / Slovak |
  | sl | Slovenia / Slovenian |
  | sn | Shona |
  | so | Somalia / Somali |
  | sq | Albania / Albanian |
  | sr | Serbia / Serbian |
  | su | Sunda / Sundanese |
  | sv | Swedia / Swedish |
  | sw | Swahili |
  | ta | Tamil |
  | te | Telugu |
  | tg | Tajik |
  | th | Thai |
  | tk | Turkmenistan / Turkmen |
  | tl | Tagagalog |
  | tr | Turki / Turkish |
  | tt | Tatar |
  | uk | Ukraina / Ukraininan|
  | ur | Urdu |
  | uz | Uzbek |
  | vi | Vietnam / Vietnamese |
  | yi | Yiddish |
  | yo | Yoruba |
  | zh | Cina / Chinese |

#### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | source_language | String | Source language of audio |
  | target_language | String | Target language of transcribe |
  | diarization | Boolean | Including Process Diarization or Not |
  | subtitle_cc | Boolean | Creating subtitle file or Not |
  | resume | Boolean | Creating resume or Not |
  | wordcloud | Boolean | Creating wordcloud or Not |
  | uuid | String | Used for get the result of transcribe |
  | message_status | String | `'success'`, `'failed'`, `'inquery'` or `'inprogress'` message |
  | credits | Int | Your credits balance |

#### **Example Response :**
```json
{
    "bk": {
        "data": { 
            "source_language": <str>,
            "target_language": <str>,
            "diarization": <boolean>,
            "subtitle_cc": <boolean>,
            "resume": <boolean>,
            "wordcloud": <boolean>,
            "uuid": <str>,
            "credits": <int>
        },
        "message_status": <str>
    }
}
```

#### **Sample Post in Python:**
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
    parser.add_argument("-r", "--resume", dest="resume",
                default=False, help="create resume")
    parser.add_argument("-w", "--wordcloud", dest="wordcloud",
                default=False, help="create wordcloud")

    args = parser.parse_args()
    if not os.path.exists(args.filename) :
        parser.print_help()
        return

    url_post = "https://api.dikte.in/v2/prod/stt/async/upload"
    headers={"Authorization": "Bearer <your token>"}

    file = {
        "file": open(args.filename, "rb")
    }

    data = {
        "source_language": args.source_language,
        "target_language": args.target_language,
        "diarization": args.diarization,
        "subtitle_cc": args.subtitle_cc,
        "resume": args.resume,
        "wordcloud": args.wordcloud
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
  | message_status | String | status process |

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
  | resume | dict | Result resume, null if resume false, if true contains title, resume, key_points, action_plan, resume_date, status |
  | wordcloud | String (base64) | Result worcloud, null string if wordcloud false |
  | top_frequencies | dict | Top ten frequencies word |
  | subtitle_cc | String (base64) | Subtitle file in base64 format, you must decode it, information will be displayed if status is `success` |
  | credits | Int | Remaining credits info, information will be displayed if status is `success` |
  | message_status | String | `'success'`, `'inprogress'` message |
  | progress | Float | Percentage progress or null, information will be displayed if status is `inprogress` |

#### **Example Response (In-Progress) :**
```json
{
    "bk": {
        "uuid": <str>,
        "data": {
            "source_language": <str>,
            "target_language": <str>,
            "transcripts": [
                {
                    "text":"halo nama saya",
                    "start_time": <float>,
                    "end_time": <float>,
                    "speaker": "speaker_01"
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
            "uuid": <str>,
            "source_language": <str>,
            "target_language": <str>,
            "uuid": <str>,
            "duration": <int>,
            "total_segments": <int>, 
            "transcripts": [
                {
                    "text":"halo nama saya",
                    "start_time": <float>,
                    "end_time": <float>,
                    "speaker": "speaker_01"
                }
            ],
            "subtitle_cc": <str> (base64),
            "resume": {
                "title": <str>,
                "resume": <str>,
                "key_points": <list>,
                "action_plan": <list>,
                "resume_date": <str_date>,
                "status": <str> // inprogress | success
            },
            "wordcloud": <str_base64>,
            "top_frequencies": [
                {
                    "word": <str>,
                    "frequencies": <int>
                }
            ]
        },
        "credits": <int>,
        "message_status": "success"
    }
}
```

#### **Sample Get in Python:**
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
    
    headers={"Authorization": "Bearer <your token>"}
    uuid_code = args.uuid    
    url_get = f"https://api.dikte.in/v2/prod/stt/async/content/{uuid_code}"

    while True:
        response_get = requests.request("GET", url_post, headers=headers)
        if "bk" in response_get.json():
            if "data" in response_get.json()["bk"] and "message_status" in response_get.json()["bk"]:
                if args.resume == "true":
                    if response_get.json()["bk"]["message_status"] == "success" and response_get.json()["bk"]["data"]["resume"] != None \
                            and response_get.json()["bk"]["data"]["resume"]["status"] == "success":
                        print("TRANSCRIPT: ", json.dumps([transcript["speaker"] + ": " + transcript["text"] for transcript in response_get.json()["bk"]["data"]["transcripts"]], indent=4))
                        print("RESUME    : ", json.dumps(response_get.json()["bk"]["data"]["resume"], indent=4))
                        break
                    elif response_get.json()["bk"]["message_status"] == "inquery":
                        time.sleep(1.0)
                    elif response_get.json()["bk"]["message_status"] == "inprogress":
                        print("PROGRESS  : ", json.dumps(response_get.json()["bk"]["progress"]), end="\r")
                        time.sleep(1.0)
                    else:
                        break
                elif args.resume == "false":
                    if response_get.json()["bk"]["message_status"] == "success":
                        print("TRANSCRIPT: ", json.dumps([transcript["speaker"] + ": " + transcript["text"] for transcript in response_get.json()["bk"]["data"]["transcripts"]], indent=4))
                        break
                    elif response_get.json()["bk"]["message_status"] == "inquery":
                        time.sleep(1.0)
                    elif response_get.json()["bk"]["message_status"] == "inprogress":
                        print("PROGRESS  : ", json.dumps(response_get.json()["bk"]["progress"]), end="\r")
                        time.sleep(1.0)
                    else:
                        break
            else:
                print(json.dumps(response_get.json(), indent=4))
                if response_get.status_code >= 400:
                    break
                else:
                    time.sleep(1.0)
        else:
            print(response_get.text)
            break


if __name__ == "__main__":
    main()
```