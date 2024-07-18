# **Speaker Recognition APIs**
The `Speaker Recognition APIs documentation` serve as a comprehensive guide for interacting with Bahasakita Cognitive AI services in  speaker recognition. Among these capabilities are speaker identification, speaker registration (enrollment), and speaker deletion.

#### **Table of Contents**
  - [General API Information](#general-api-information)
  - [Tech Stack](#tech-stack)
  - [Identification Speaker Recogniton API](#sid-identification-api) 
  - [Enrollment Speaker Recogniton API](#sid-enrollment-api) 
  - [Remove Speaker Recogniton APIs](#sid-remove-api) 

## **General API Information**
  - The base endpoint is: 
    - [https://api.bahasakita.co.id](https://api.bahasakita.co.id) for [REST](https://restfulapi.net/)
     - All endpoints return JSON object.

## **Tech Stack**
  - **[REST](https://restfulapi.net/)**  
 
## **How to Use: Step by Step**
  1. Obtain your access token using [Our API](./Auth-API.md) 
  2. Upload the audio file you want to be processed for speaker identification (enrollment or  identify) to the `endpoint-post`. Ensure the audio file meets the specified format and quality requirements.
  3. Wait for the response. This will return detailed information about the speaker identification process, including timestamps, message status and identified speakers.
   
## **Hostname :**
  [https://api.bahasakita.co.id](https://api.bahasakita.co.id)


## **SID Identification API**
  In order to use this API, you required to create account by registering yourself.

### **Endpoint :**
 `POST: /v2/prod/sid/identification`
  
### **Request**
#### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Content-Type | `multipart/form-data` |
  | Authorization | `Bearer token` |

#### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | file | File | Your audio file  |
  | participants | List of string | By specifying this parameter, the identification process will match the audio to the existing participants. If the parameter is not specified (`None`), the system will match the audio to all speakers previously registered by the user.  |
  | global | Boolean| This parameter is avalaible for on-premise system, Setting is `True` will match audio to all speakers in the database |

### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | type | string | `identification` |
  | decision | string | The identified speaker has a match.  |
  | total_candidates | int | The total number of possible candidates |
  | candidates | List | List of the detected candidates, The content includes the speaker's name (`string`) and the similarity score / likelihood (`float`)|
  | message_status | String | `'success'`, `'failed'`, `'inquery'` or `'inprogress'` message |
  | created | datetime | The datetime the process was created |
  | expired | datetime | The datetime when the expiration of the results will be available |
    
#### **Example Response :**
```json
{
   "bk": {
              "data" : {                        
                "type":"identification",
                "global":true,                                                 
                "results" :{
                      "decision" :"speakername1",
                      "total_candidates":2, 
                      "candidates":
                  [
                  {"speakerid":"speakername1","likelihood":25.00},
                  {"speakerid":"speakername2","likelihood": 75.00}
                  ]
              }, 
             "message_status": <string>,                    
             "created": datetime.isoformat(),
             "expired": datetime.isoformat()
   }}
}
```

### **Sample Post in Python:**

```python
import requests
from argparse import ArgumentParser


def main():
    parser = ArgumentParser()
    parser.add_argument("-f", "--file", dest="filename",
                help="write report to FILE", metavar="FILE")

    args = parser.parse_args()
    if not os.path.exists(args.filename) :
        parser.print_help()
        return

    url_post = "https://api.bahasakita.co.id/v2/prod/sid/identification/"
    headers={"Authorization": "Bearer <your token>"}
    
    file = {
        'file': open(args.filename,'rb')
    }

    form_data = {       
            "participants":"[\"admin\"]"
            "global": False
    }   
    
    post_response = requests.request("POST", url_post, headers=headers, files=file,data=form_data).json()
    print(post_response)
  

if __name__ == "__main__":
    main()
```

## **SID Enrollment API**
### **Endpoint :**
 `POST: /v2/prod/sid/enrollment`
  
### **Request**
#### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Content-Type | `multipart/form-data` |
  | Authorization | `Bearer token` |

#### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | file | File | Your audio file  |
  | speakerid | string| the name of speaker for enrollment process|

### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | type | string | `enrollment` |
  | speakerid | string| the name of speaker for enrollment process|
  | message_status | String | `'success'`, `'failed'`, `'inquery'` or `'inprogress'` message |
  | created | datetime | The datetime the process was created |
  | expired | datetime | The datetime when the expiration of the results will be available |
    
#### **Example Response :**

```json
{
  "bk":{
           "data" : {                                                          
                  "type":"enrollment",
                  "speakerid":"admin"
             }, 
             "message_status": <string>,                    
             "created": datetime.isoformat(),
             "expired": datetime.isoformat()
  }
}
```

### **Sample Post in Python:**
```python
import requests
from argparse import ArgumentParser


def main():
    parser = ArgumentParser()
    parser.add_argument("-f", "--file", dest="filename",
                help="write report to FILE", metavar="FILE")

    args = parser.parse_args()
    if not os.path.exists(args.filename) :
        parser.print_help()
        return

    url_post = "https://api.bahasakita.co.id/v2/prod/sid/enrollment"
    headers={"Authorization": "Bearer <your token>"}
    
    file = {
        'file': open(args.filename,'rb')
    }
    
    form_data = {       
            "speakerid":"admin"
    }  

    post_response = requests.request("POST", url_post, headers=headers, files=file,data=form_data).json()
    print(post_response)
  
if __name__ == "__main__":
    main()

```

## **SID Remove API**
### **Endpoint :**
 `DELETE: /v2/prod/sid/remove`
  
### **Request**
#### **Headers**
  | Name | Format |
  | ------ | ------ |
  | Content-Type | `multipart/form-data` |
  | Authorization | `Bearer token` |

#### **Body**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | speakerid | string| the name of speaker for enrollment process|

### **Response**
  | Field | Data Type | Description |
  | ------ | ------ | ------ |
  | type | string | `remove` |
  | speakerid | string| the name of speaker for enrollment process|
  | message_status | String | `'success'`, `'failed'`, `'inquery'` or `'inprogress'` message |
  | created | datetime | The datetime the process was created |
  | expired | datetime | The datetime when the expiration of the results will be available |
    
#### **Example Response :**
```json
{
  "bk":{
           "data" : {                                                          
                  "type":"remove",
                  "speakerid":"admin"
             }, 
             "message_status": <string>,                    
             "created": datetime.isoformat(),
             "expired": datetime.isoformat()
  }
}
```

### **Sample Post in Python:**

```python

import requests
from argparse import ArgumentParser

def main():
    parser = ArgumentParser()
    parser.add_argument("-f", "--file", dest="filename",
                help="write report to FILE", metavar="FILE")

    args = parser.parse_args()
    if not os.path.exists(args.filename) :
        parser.print_help()
        return

    url_post = "https://api.bahasakita.co.id/v2/prod/sid/remove"
    headers={"Authorization": "Bearer <your token>"}
    
    form_data = {       
            "speakerid":"admin"
    }  

    post_response = requests.request("DELETE", url_post, headers=headers,data=form_data).json()
    print(post_response)
  
if __name__ == "__main__":
    main()

```
