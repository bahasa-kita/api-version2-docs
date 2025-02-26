# **Speech to Text API**
API STT Documentation is guidance for communicate with bahasakita speech recognition service with the [WebSocket protocol as defined in RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)

## **Table of Contents**
  - [General API Information](#general-api-information)
  - [Diagram Activity](#diagram-activity)
  - [API](#list-of-states)
    - [Audio Connection](#state-1-audioconn)
    - [Audio Stream](#state-2-audiostream)
    - [Audio Stop](#state-3-audiostop)

### **General API Information**
  - The base endpoint is: 
    - `wss://api.dikte.in` used WEBSOCKET RFC 6455.
    - All endpoints return JSON object.

### **Diagram Activity**
  ![Diagram Activity](/asset/stt-streaming.png "Diagram Activity")

### **List of States**
  | Name | State Types | Description |
  | ------ | ------ | ------ |
  | [start stream audio](#state-1-audioconn) |  type : AudioConn    | initialize stream before send audio |
  | [send stream audio](#state-2-audiostream) | type : AudioStream  | send audio after stream initialized |
  | [stop stream audio](#state-3-audiostop) | type : AudioStop    | end stream. you need to request [start stream audio](#start-stream-audio) if you want to send audio again after [stop stream audio](#stop-stream-audio) |

### **URL ACCESS**
  `wss://api.dikte.in/v2/prod/stt/record?token=xxxxxxxxxx`

### **"How to Use" Flow**
  1. get token with access [Our API](./Auth-API.md)
  2. Send request message [AudioConn state](#state-1-audioconn).
  3. Read bytes of the audio from your device computer ( microphone or file).
  4. Please send  `raw audio` format `signed integer` chunks size of `3200 bytes` with `samplerate 16000` and `mono channel`.
  5. Speech recognition service will return transcript response
  6. Send request message [AudioStop state](#state-3-audiostop) to end process.


### **STATE 1 AudioConn**
  AudioConn is the first step to start sending the audio bytes you want to transcribe.

#### **Request**
##### **Language List**
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

##### **Body**
  | Field |Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service |
  | type | String | `AudioConn` |
  | source_language | String | source language id code for transcription |
  | target_language | String | target language id code for transcription |
  | diarization | Boolean | diarization feature |
  | subtitle_cc | Boolean | subtitle and close caption feature |
  | resume | Boolean | resume feature |
  | wordcloud | Boolean | wordcloud feature |

##### **Data Structure**
```json
{
    "bk": {
        "service": "stt",
        "type": "audioConn",
        "source_language": "id",
        "target_language": "id",
        "diarization": false,
        "subtitle_cc": true,
        "resume": false,
        "wordcloud": false          
    }
}
```

#### **Response**
##### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service |
  | type | String | `AudioConn` |
  | uuid | String | used for get the transcription results |
  | source_language | String | source language id code for transcription |
  | target_language | String | target language id code for transcription |
  | diarization | Boolean | diarization feature |
  | subtitle_cc | Boolean | subtitle and close caption feature |
  | check_summary | Boolean | summirize feature |
  | max_sentence_summary | Int | maximum number of sentences for the summary |
  | title | String | title of sentences for the summary |
  | status | Int | `200` is `success` or `400` is `failed` |
  | message_status | String | detail status information |

##### **Data Structure**
```json
{
    "bk": {
        "service": "stt",
        "type": "audioConn",
        "uuid": "xxxxxxxxxxxxxx",
        "source_language": "id",
        "target_language": "id",
        "diarization": false,
        "subtitle_cc": true,
        "status": 200,
        "resume": false,
        "worcloud": false,
        "message_status": "Koneksi Telah Siap",
    }
}
```

### **STATE 2 AudioStream**
#### **Request**
##### **Body**
  | Field |Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service|
  | type | String | `AudioStream` |
  | audio | String | bytes audio with base64 encode |

##### **Data Structure**
```json
{
    "bk": {
        "service": "stt",
        "type": "audioStream",            
        "data": {
            "audio": <base64(bytes)>
        }
    }
}
```

#### **Response -- Partial Transcripts**
##### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service|
  | type | String | `AudioStream` |
  | text_type | String | `partial` text or `final` text |
  | status | Int | `200` is `success` or `400` is `failed` |
  | message_status | String | status information |
  | words | List | partial transcript is list of words |
  | credits | Int | your credits balance |

##### **Data Structure**
```json
{
    "bk": {
        "service": "stt",
        "type": "audioStream",
        "data": {
            "words": <list>,
            "credits": <int>
        },
        "text_type": "partial",
        "status": 200,
        "message_status": "success"
    }
}
```

#### **Response -- Final Trancripts**
##### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service|
  | type | String | `AudioStream` |
  | text_type | String | `partial` text or `final` text |
  | status | Int | `200` is `success` or `400` is `failed` |
  | message_status | String | status information |
  | source_language | String | code language source |
  | target_language | String | code language target |
  | transcripts | List | result of transcribe conatains {text: `<str>`, start: `<float>`, end: `<float>`} |
  | credits | Int | your credits balance |

##### **Data Structure**
```json
{
    "bk": {
        "service": "stt",
        "type": "audioStream",
        "data": {
            "source_language": <str>,
            "target_language": <str>,
            "transcripts": {
                "text": <str>,
                "start": <float>,
                "end": <float>
            },
            "credits": <int>
         },
         "text_type": "final",
         "message_status": "success"
    }
}
```

### **STATE 3 AudioStop**
#### **Request**
##### **Body**
  | Field |Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service|
  | type | String | `AudioStop` |
  | audio | None | Audio None |

##### **Data Structure**
```json
{
    "bk":{
        "service": "stt",
        "type": "audioStop",
        "data": {
            "audio": null
        }
    }
}
```

#### **Response**
##### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service|
  | type | String | `AudioStop` |
  | audio | None | Audio None |
  | status | Int | `200` is `success` or `400` is `failed` |
  | message_status | String | status information |
  
##### **Data Structure**
```json
{
    "bk":{
        "service": "stt",
        "type": "audioStop",
        "data": {
            "audio": null
        },
        "status": 200,
        "message_status": "success"
    }
}
```

#### **Response - Final Transcript**
##### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service|
  | type | String | `AudioStop` |
  | uuid | String | used for get the transcription results |
  | source_language | String | source language id code for transcription |
  | target_language | String | target language id code for transcription |
  | duration | Int | Information duration audio (second), information will be displayed if status is `success` |
  | total_segments | Int | Information total segment of transcribe result, information will be displayed if status is `success` |
  | transcripts | List | result of transcribe conatains {text: `<str>`, start: `<float>`, end: `<float>`, speaker: `<str>`} |
  | resume | dict | Result resume, null string if resume false, if resume true, result of resume contains {title: `<str>`, outline: `<list>`, resume: `<str>`, key_points: `<list>`, action_plan: `<list>`, resume_date: `<str-date>`, status `<str>`} |
  | subtitle_cc | String (base64) | Subtitle file in base64 format, you must decode it, information will be displayed if status is `success` |
  | wordcloud | String (base64) | image wordcloud in base64 format |
  | top_frequencies | dict | top ten frequencies contains {word: `<str>`, frequencies": `<int>`}  |
  | duration | Int | total duration |
  | credits | Int | your credits balance |
  | status | Int | `200` is `success` or `400` is `failed` |
  | message_status | String | status information |
  
##### **Data Structure**
```json
{
    "bk": {
        "service": "stt",
        "type": "audioStop",
        "data": {
            "uuid": <str>,
            "source_language": <str>,
            "target_language": <str>,
            "total_segments": <int>,
            "transcripts": <list>,
            "subtitle_cc": <str_base64>,
            "wordcloud": <str_base64>,
            "top_frequencies": <list>,        // { "word": <str>, "frequencies": <int> }
            "resume": <null> or <dict>,       // { "title": <str>, "resume": <str>, "key_points": <list>, "action_plan": <list>, "resume_date": <str-date>, "status" <str> // "inprogress", "success" }
            "duration": <int>,
            "credits": <int>,
            "finish-state": True
        },
        "credits": <int>,
        "status_quota": True,
        "status": 200,
        "message_status": "success"
}
```


### **Sample Call in Python**
#### **Requeiremets**
- `python3.6` or later
- `pyaudio`
- `websockets10.x` or later

#### **Sample Code**
```python
import asyncio
import base64
import json
import functools
import signal
import pyaudio
import websockets


FORMAT = pyaudio.paInt16
CHANNELS = 1
RATE = 16000
CHUNK = int(RATE / 10) # 3200 bytes / 100ms

SOURCE_LANG = "id"
TARGET_LANG = "id"
DIARIZATION = False
SUBTITLE_CC = True
RESUME = False
WORDCLOUD = False

queue: asyncio.Queue = None


def ask_exit(signame, stream):
    stream.stop_stream()    
    print("got signal %s: exit" % signame)
    
async def main():
    # Websocket URL, changed with your token
    url = f"wss://api.dikte.in/v2/prod/stt/record?token={AUTH_TOKEN}"

    global queue
    queue = asyncio.Queue()
    audio = pyaudio.PyAudio()
    stream = audio.open(
        format=FORMAT,
        channels=CHANNELS,
        rate=RATE,
        input=True,
        frames_per_buffer=CHUNK,
        stream_callback=pyaudio_callback,
    )

    loop = asyncio.get_running_loop()
    for signame in {'SIGINT', 'SIGTERM'}:
        loop.add_signal_handler(
        getattr(signal, signame),
        functools.partial(ask_exit, signame, stream))

    async with websockets.connect(url, ping_interval=None) as ws:
        # sending "audioConn type"
        msg = {
            "bk": {
                "service": "stt",
                "type": "audioConn",
                "source_language": SOURCE_LANG,
                "target_language": TARGET_LANG,
                "diarization": DIARIZATION,
                "subtitle_cc": SUBTITLE_CC,
                "resume": RESUME,
                "wordcloud": WORDCLOUD
            }
        }
        await ws.send(json.dumps(msg))
        reply = await ws.recv()  # Receive message

        print("Response :", reply)
        reply = json.loads(reply)
        if reply["bk"]["status"] == 200:
            recieve_task = asyncio.create_task(receive_message(ws,stream))
            stream_task = asyncio.create_task(stream_mic(ws,stream))
            await asyncio.gather(
                recieve_task,
                stream_task               
            )
            stream.close()
        else:
            print(reply["bk"]["message_status"])

        print("Finished...")

async def stream_mic(ws, stream):
    print("Streaming MIC .... ")
    while not stream.is_stopped():
        data,status = await queue.get()  
        bschunk = base64.b64encode(data)

        # sending "audioStream type"
        msg = {
            "bk": {
                "service": "stt",
                "type": "audioStream",
                "data": {
                    "audio": bschunk.decode("utf-8")
                },
            }
        }
        msg = json.dumps(msg)
        await ws.send(msg)
    
    if stream.is_stopped():
        # sending "audioStop type"
        msg = {
            "bk": {
                "service": "stt", 
                "type": "audioStop",
                "data": {
                    "audio": None
                }
            }
        }
        msg = json.dumps(msg)
        await ws.send(msg)

def pyaudio_callback(in_data, frame_count, time_info, status):
    queue.put_nowait((in_data, status))

    return in_data, pyaudio.paContinue

async def receive_message(ws, stream):
    print("Waiting Response....")
    stop_count = 0
    def check_resume_wordcloud(reply, resume, wordcloud):
        if resume == "true":
            if "data" in reply["bk"] and "resume" in reply["bk"]["data"] and reply["bk"]["data"]["resume"] != None \
                    and "status" in reply["bk"]["data"]["resume"] and reply["bk"]["data"]["resume"]["status"] == "success":
                if wordcloud == "true":
                    if "top_frequencies" in reply["bk"]["data"]:
                        print("WORDCLOUD ", json.dumps(reply["bk"]["data"]["top_frequencies"], indent=4))
                print("TITLE        : ", reply["bk"]["data"]["resume"]["title"], "\n", sep="")
                print("KEY POINTS   : ", json.dumps(reply["bk"]["data"]["resume"]["key_points"], indent=4), "\n", sep="")
                print("RESUME       : ", reply["bk"]["data"]["resume"]["resume"], "\n", sep="")
                print("ACTION PLAN  : ", json.dumps(reply["bk"]["data"]["resume"]["action_plan"], indent=4), "\n", sep="")
                print("RESUME DATE  : ", reply["bk"]["data"]["resume"]["resume_date"], sep="")
                print("STATUS       : ", reply["bk"]["data"]["resume"]["status"], sep="")
                break_status = True
            else:
                if wordcloud == "true":
                    if "top_frequencies" in reply["bk"]["data"]:
                        print("WORDCLOUD ", json.dumps(reply["bk"]["data"]["top_frequencies"], indent=4))
                break_status = False
        else:
            if wordcloud == "true":
                if "top_frequencies" in reply["bk"]["data"]:
                    print("WORDCLOUD ", json.dumps(reply["bk"]["data"]["top_frequencies"], indent=4))
            break_status = True
        return break_status

    while True:
        try:
            reply =  await asyncio.wait_for(ws.recv(), timeout=0.01) ## Receive message
            if not reply is None:
                reply = json.loads(reply)
                if reply["bk"]["service"] == "stt":
                    if "type" in reply["bk"] and "audioStop" == reply["bk"]["type"] and "audio" in reply["bk"]["data"] and reply["bk"]["data"]["audio"]== None:
                        print("End of Stream Audio")
                    if "type" in reply["bk"] and "audioStop" == reply["bk"]["type"] and "final-state" in reply["bk"]["data"] and reply["bk"]["data"]["final-state"]:
                        if diarization == "true":
                            text = [transcript["text"] for transcript in reply["bk"]["data"]["transcripts"]]
                            print("All-Transcript :", " ". join(text))
                            if "speaker" in reply["bk"]["data"]["transcripts"][0] and reply["bk"]["data"]["transcripts"][0]["speaker"] != None \
                                    and reply["bk"]["data"]["transcripts"][0]["speaker"] != "":
                                break_status = check_resume_wordcloud(reply, resume, wordcloud)
                                if break_status:
                                    break
                                else:
                                    continue
                            else:
                                continue
                        else:
                            break_status = check_resume_wordcloud(reply, resume, wordcloud)
                            if break_status:
                                break
                            else:
                                continue
                    if "type" in reply["bk"] and "audioStream" == reply["bk"]["type"]:
                        if "words" in reply["bk"]["data"]:
                            utterance = " ".join(reply["bk"]["data"]["words"])
                            print("Partial-Transcript :", utterance, end="\r", flush=True)
                        if "transcripts" in reply["bk"]["data"] and reply["bk"]["data"]["transcripts"] != []:
                            text = [transcript["text"] for transcript in reply["bk"]["data"]["transcripts"]]
                            print("Final-Transcript :", " ". join(text))
                if stop_count > 20:
                    stream.stop_stream()
                    break
        except asyncio.TimeoutError:
            pass
    await ws.close()


if __name__ == "__main__":
    asyncio.run(main())
```