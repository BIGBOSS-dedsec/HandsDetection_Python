# Introduction

## Project renderings
*The number of video capture frames is stable at (25-30)*
![在这里插入图片描述](https://img-blog.csdnimg.cn/9add66a4fd054d41b733bfec5feede8c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQklHQk9TU3lpZmk=,size_18,color_FFFFFF,t_70,g_se,x_16)
# Mediapipe

[Mediapipe Dev](https://mediapipe.dev/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/634b06ee5f7d4295a388e651e77f2b22.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQklHQk9TU3lpZmk=,size_20,color_FFFFFF,t_70,g_se,x_16)

Python install **Mediapipe**
```python
pip install mediapipe==0.8.9.1
```
Or you can use **setup.py** to install
[https://github.com/google/mediapipe](https://github.com/google/mediapipe)

# Environment
**Python			3.7
Mediapipe     0.8.9.1
Numpy		1.21.6
OpenCV-Python 		4.5.5.64
OpenCV-contrib-Python		4.5.5.64**
![在这里插入图片描述](https://img-blog.csdnimg.cn/e49c1c0fa21c4df08ee58e57ce57f8b4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQklHQk9TU3lpZmk=,size_20,color_FFFFFF,t_70,g_se,x_16)

# Code
## CoreCode
**OpenCV Capture**：
```python
import cv2

cap = cv2.VideoCapture(0)       #OpenCV摄像头调用：0=内置摄像头（笔记本）   1=USB摄像头-1  2=USB摄像头-2

while True:
    success, img = cap.read()
    imgRGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)       #cv2图像初始化
    cv2.imshow("HandsImage", img)       #CV2窗体
    cv2.waitKey(1)      #关闭窗体
```
**mediapipe HandDetect**

```python
#定义并引用mediapipe中的hands模块
mpHands = mp.solutions.hands
hands = mpHands.Hands()
mpDraw = mp.solutions.drawing_utils

while True:
    success, img = cap.read()
    imgRGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)       #cv2图像初始化
    results = hands.process(imgRGB)
    # print(results.multi_hand_landmarks)
    
    if results.multi_hand_landmarks:
        for handLms in results.multi_hand_landmarks:
            for id, lm in enumerate(handLms.landmark):
                # print(id, lm)
                h, w, c = img.shape
                cx, cy = int(lm.x * w), int(lm.y * h)
                print(id, cx, cy)
                # if id == 4:
                cv2.circle(img, (cx, cy), 15, (255, 0, 255), cv2.FILLED)
            
            #绘制手部特征点：
            mpDraw.draw_landmarks(img, handLms, mpHands.HAND_CONNECTIONS)
```
### FPS

```python
import time

#帧率时间计算
pTime = 0
cTime = 0

while True
cTime = time.time()
    fps = 1 / (cTime - pTime)
    pTime = cTime

    cv2.putText(img, str(int(fps)), (10, 70), cv2.FONT_HERSHEY_PLAIN, 3,
                (255, 0, 255), 3)       #FPS的字号，颜色等设置
```

## Final Code

```python
# Coding BIGBOSSyifi
# Datatime:2022/4/24 21:41
# Filename:HandsDetector.py
# Toolby: PyCharm

import cv2
import mediapipe as mp
import time

cap = cv2.VideoCapture(0)       #OpenCV摄像头调用：0=内置摄像头（笔记本）   1=USB摄像头-1  2=USB摄像头-2

#定义并引用mediapipe中的hands模块
mpHands = mp.solutions.hands
hands = mpHands.Hands()
mpDraw = mp.solutions.drawing_utils

#帧率时间计算
pTime = 0
cTime = 0

while True:
    success, img = cap.read()
    imgRGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)       #cv2图像初始化
    results = hands.process(imgRGB)
    # print(results.multi_hand_landmarks)
    
    if results.multi_hand_landmarks:
        for handLms in results.multi_hand_landmarks:
            for id, lm in enumerate(handLms.landmark):
                # print(id, lm)
                h, w, c = img.shape
                cx, cy = int(lm.x * w), int(lm.y * h)
                print(id, cx, cy)
                # if id == 4:
                cv2.circle(img, (cx, cy), 15, (255, 0, 255), cv2.FILLED)
            
            #绘制手部特征点：
            mpDraw.draw_landmarks(img, handLms, mpHands.HAND_CONNECTIONS)
    '''''
    视频FPS计算
       '''
    cTime = time.time()
    fps = 1 / (cTime - pTime)
    pTime = cTime

    cv2.putText(img, str(int(fps)), (10, 70), cv2.FONT_HERSHEY_PLAIN, 3,
                (255, 0, 255), 3)       #FPS的字号，颜色等设置

    cv2.imshow("HandsImage", img)       #CV2窗体
    cv2.waitKey(1)      #关闭窗体
```
### Project effect
![在这里插入图片描述](https://img-blog.csdnimg.cn/50e4d32e5f0a444c87cfb92ea3747363.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQklHQk9TU3lpZmk=,size_20,color_FFFFFF,t_70,g_se,x_16)

