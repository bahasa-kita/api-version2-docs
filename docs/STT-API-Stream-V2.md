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
  `wss://api.bahasakita.co.id/v2/prod/stream2?token=xxxxxxxxxx`

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
  | language | String | `id` language id code for transcription |
  | type | String | `AudioConn` |

##### **Data Structure**
```json
{
    "bk": {
        "service": "stt", 
        "language": "id",
        "type": "audioConn"            
    }
}
```

#### **Response**
##### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service |
  | language | String | `id` language id code for transcription |
  | type | String | `AudioConn` |
  | status | Int | `200` is `success` or `400` is `failed` |
  | message_status | String | detail status information |

##### **Data Structure**
```json
{
    "bk": {
        "service": "stt",
        "type": "audioConn",
        "language": "id",
        "status": 200,
        "message_status": "message status",
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
  | transcript | List | Result of transcribe conatains [text `<str>`, start time `<float>`, end time `<float>`, speaker `<str>`] |

##### **Data Structure**
```json
{
    "bk": {
        "service": "stt",
        "type": "audioStream",
        "data" : {
            "transcript": [
                {
                    "text": "halo nama saya",
                    "text_confidence": 0.9,
                    "start_time": <float>,
                    "end_time": <float>,
                    "speaker": "speaker_01"
                }
            ],
        },
        "text_type": "partial",
        "status": 200,
        "message_status": "message status",
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
            "audio": None
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
            "audio": None
        },
        "status": 200,
        "message_status": "message status"
    }
}
```

#### **Response - Final Transcript**
##### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | service | String | `stt` for speech recognition service|
  | type | String | `AudioStop` |
  | final | None | Final None |
  | status | Int | `200` is `success` or `400` is `failed` |
  | message_status | String | status information |
  
##### **Data Structure**
```json
{
    "bk":{
        "service": "stt",
        "type": "audioStop",
        "data": {
            "final": None
        }
        "status": 200,
        "message_status": "message status"
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

queue: asyncio.Queue = None


def ask_exit(signame, stream):
    stream.stop_stream()    
    print("got signal %s: exit" % signame)
    
async def main():
    # Websocket URL, changed with your token
    url = f"wss://api.bahasakita.co.id/v2/prod/stream2?token={AUTH_TOKEN}"

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
                "language": "id"
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
        #time.sleep(0.1)

def pyaudio_callback(in_data, frame_count, time_info, status):
    queue.put_nowait((in_data, status))

    return in_data, pyaudio.paContinue

async def receive_message(ws, stream):
    print("Waiting Response....")
    while True:
        try:
            reply =  await asyncio.wait_for(ws.recv(), timeout=0.01) ## Receive message
            if not reply is None:
                reply = json.loads(reply)
                if reply["bk"]["service"] == "stt":
                    if "type" in reply["bk"] and "audioStop" == reply["bk"]["type"]:
                        print("End of Stream Audio")
                    if "data" in reply["bk"] and "transcript" in reply["bk"]["data"]:
                        for transcript in reply["bk"]["data"]["transcript"]:
                            print("Transcript :", transcript["text"])
                    if "data" in reply["bk"] and "final" in reply["bk"]["data"]:
                        print("Final Transcripts :", reply["bk"]["data"]["final"])
                        stream.stop_stream()
                        break
        except asyncio.TimeoutError:
            pass
    await ws.close()


if __name__ == "__main__":
    asyncio.run(main())

```