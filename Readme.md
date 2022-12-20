# Official Documentation for the BahasaKita APIs and Streams.

* Streams, endpoints, parameters, payloads, etc. described in the documents in this repository are considered **official** and **supported**.
* The use of any other streams, endpoints, parameters, or payloads, etc. is **not supported**; **use them at your own risk and with no guarantees.**


Items       | Sub-Items | Description |
------------        |------------ | ------------ |
[Auth API](./docs/Auth-API.md)       | | API for get token authorization |
 Speech To Text API       |[STT API Streaming](./docs/STT-API-Stream-V2.md) | Speech to Text API v2 Documentation for streaming process |
 |      |[STT API Upload](./docs/STT-API-Upload-Transcription.md) | Speech to Text API v2 Documentation for uploading process |
 |      |[STT API Diarization](./docs/STT-API-Upload-Diarization.md) | Speech to Text API Diarization Documentation |
 |      |[STT API Monitoring](./docs/STT-API-Monitoring.md) | Speech to Text API User Monitoring Documentation |
 |      |[STT API Change Priority](./docs/STT-API-Change-Priority.md) | Speech to Text API Change Priority Documentation |
 Text To Speech API        |[Sync TTS API ](./docs/TTS-API-Sync.md) |Synchronous Text-to-Speech API Documentation is guidance for communicate with bahasakita speech synthesizer service, wait for the processing to complete |
 |      |[Async TTS API](./docs/TTS-API-Async.md) | Asynchronous Text-to-Speech API Documentation is guidance for communicate with bahasakita speech synthesizer service, without having to wait for synthesis processing to complete and return an audio url path result |