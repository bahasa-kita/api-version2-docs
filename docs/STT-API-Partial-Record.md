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
    - `wss://api.bahasakita.co.id` used WEBSOCKET RFC 6455.
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
  `wss://stagapi.bahasakita.co.id/v2/prod/stt/record?token=xxxxxxxxxx`

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
##### **Body**
  | Field |Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service |
  | type | String | `AudioConn` |
  | source_language | String | source language id code for transcription |
  | target_language | String | target language id code for transcription |
  | diarization | Boolean | diarization feature |
  | subtitle_cc | Boolean | subtitle and close caption feature |
  | check_summary | Boolean | summirize feature |
  | max_sentence_summary | Int | maximum number of sentences for the summary |
  | title | String | title of sentences for the summary |

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
        "check_summary": false,
        "max_sentence_summary": 3,
        "title": "title"          
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
        "check_summary": false,
        "max_sentence_summary": 3,
        "title": "title",
        "status": 200,
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

#### **Response**
##### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service|
  | type | String | `AudioStream` |
  | text_type | String | `partial` text or `final` text |
  | status | Int | `200` is `success` or `400` is `failed` |
  | message_status | String | status information |
  | transcripts | List | result of transcribe conatains {text: `<str>`, start: `<float>`, end: `<float>`, speaker: `<str>`} |
  | quota | Int | your quota balance |

##### **Data Structure**
```json
{
    "bk": {
        "service": "stt",
        "type": "audioStream",
        "data" : {
            "transcripts": [
                {
                    "text": "halo nama saya",
                    "start_time": <float>,
                    "end_time": <float>,
                    "speaker": "SPEAKER_00"
                }
            ],
            "quota": <int>
        },
        "text_type": "final",
        "status": 200,
        "message_status": "Transcript Success",
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
        "message_status": "Audio Stop"
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
  | total_segments | Integer | Information total segment of transcribe result, information will be displayed if status is `success` |
  | transcripts | List | result of transcribe conatains {text: `<str>`, start: `<float>`, end: `<float>`, speaker: `<str>`} |
  | summary | List | Result summary, null string if summary false, data will be displayed if status is `success` |
  | subtitle_cc | String (base64) | Subtitle file in base64 format, you must decode it, information will be displayed if status is `success` |
  | final | None | Final None |
  | duration | Int | total duration |
  | quota | Int | your quota balance |
  | status | Int | `200` is `success` or `400` is `failed` |
  | message_status | String | status information |
  
##### **Data Structure**
```json
{
    "bk": {
        "service": "stt",
        "type": "audioStop",
        "data" : {
            "uuid": "xxxxxxxxxxxxxxxxxxxxx",
            "source_language": "en",
            "target_language": "en",
            "total_segments": <int> or null,
            "transcripts": [
                {
                    "text": "halo nama saya",
                    "start": <float>,
                    "end": <float>,
                    "speaker": "SPEAKER_00"
                }
            ],
            "subtitle_cc": <string base64>,
            "summary": null,
            "final": null,
            "duration": <int>,
            "quota": <int>
        },
        "status_quota": true,
        "message_status": "success"
    }
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
SUMMARY = False
MAX_SENTENCE = 3
TITLE = "string title"

queue: asyncio.Queue = None


def ask_exit(signame, stream):
    stream.stop_stream()    
    print("got signal %s: exit" % signame)
    
async def main():
    # Websocket URL, changed with your token
    url = f"wss://stagapi.bahasakita.co.id/v2/prod/stt/record?token={AUTH_TOKEN}"

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
                "check_summary": SUMMARY,
                "max_sentence_summary": MAX_SENTENCE,
                "title": TITLE
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
    while True:
        try:
            reply =  await asyncio.wait_for(ws.recv(), timeout=0.01) ## Receive message
            if not reply is None:
                reply = json.loads(reply)
                if reply["bk"]["service"] == "stt":
                    if "type" in reply["bk"] and "audioStop" == reply["bk"]["type"]:
                        print("End of Stream Audio")
                        stop_count += 1
                    if "data" in reply["bk"] and "transcript" in reply["bk"]["data"]:
                        for transcript in reply["bk"]["data"]["transcript"]:
                            print("Transcript :", transcript["text"])
                    if "data" in reply["bk"] and "final" in reply["bk"]["data"]:
                        print("Final Transcripts :", str(reply["bk"]["data"]["final"]))
                        stream.stop_stream()
                        if "data" in reply["bk"] and "subtitle_cc" in reply["bk"]["data"] and reply["bk"]["data"]["subtitle_cc"] != None:
                            print("Subtitle: \n", base64.decodebytes(str.encode(reply["bk"]["data"]["subtitle_cc"])).decode("utf-8"))
                        if "data" in reply["bk"] and "summary" in reply["bk"]["data"]:
                            print("Summary: ",  reply["bk"]["data"]["summary"])
                            break
                if stop_count > 20:
                    stream.stop_stream()
                    break
        except asyncio.TimeoutError:
            pass
    await ws.close()


if __name__ == "__main__":
    asyncio.run(main())
```