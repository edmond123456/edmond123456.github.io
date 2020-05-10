---
layout:     post
title:      text message to slack
subtitle:    by pressing button on mbed
date:       2019-03-19
author:     Junjie Hua
header-img: img/post-bg-keybord.jpg
catalog: true
tags:
    - Program
---

## Goal
text message to slack by pressing button on mbed


python file
```

"""
 https://www.youtube.com/watch?v=cuKQ9w9nr1o
https://api.slack.com/methods/chat.postMessage/test
https://api.slack.com/apps/AH25TSLQ4/general

"""

import requests
import json
import time
import serial
ser = serial.Serial('COM4', 115200)  # open serial port

web_hook_url = 'XXXXXXXXXXXXXXXXXXXXXXXXXX'

slack_msg ={'text': ' test from python'}
while True:
    if ser.isOpen():
        #data = ser.read()
        data = ser.read(3)
        if data == b'\x00\x00\x00':
            #print("yes")
            requests.post(web_hook_url,data=json.dumps(slack_msg))
        time.sleep(1)
    else:
        break

```

mbed c++file
```
#include "mbed.h"
 
Serial pc(USBTX, USBRX); // tx, rx
 
int main() {
 
    pc.printf("a");
}

```

## VIDEO
- https://youtu.be/KHhHRMBsovQ