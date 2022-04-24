@[TOC](Python+OpenCV手势识别Mediapipe（新手入门）)
# 前言
本篇文章适合刚入门OpenCV的同学们。文章将介绍如何使用**Python**利用**OpenCV图像捕捉**，配合强大的**Mediapipe**库来实现**手势检测与识别**；本系列后续还会**继续更新Mediapipe手势的各种衍生项目**，还请多多关注！
## 项目效果图
*视频捕捉帧数稳定在（25-30）*
![在这里插入图片描述](https://img-blog.csdnimg.cn/9add66a4fd054d41b733bfec5feede8c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQklHQk9TU3lpZmk=,size_18,color_FFFFFF,t_70,g_se,x_16)
# 认识Mediapipe
项目的实现，核心是强大的**Mediapipe** ，它是**google**的一个**开源**项目：
| 功能 |详细  |
|--|--|
|人脸检测 FaceMesh |从图像/视频中重建出人脸的3D Mesh  |
| 人像分离 |从图像/视频中把人分离出来  |
| 手势跟踪 |21个关键点的3D坐标  |
|人体3D识别  |33个关键点的3D坐标  |
| 物体颜色识别 |可以把头发检测出来，并图上颜色  |

[Mediapipe Dev](https://mediapipe.dev/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/634b06ee5f7d4295a388e651e77f2b22.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQklHQk9TU3lpZmk=,size_20,color_FFFFFF,t_70,g_se,x_16)
以上是**Mediapipe**的几个常用功能   ，*这几个功能我们会在后续一一讲解实现*
Python安装**Mediapipe**
```python
pip install mediapipe==0.8.9.1
```
也可以用 **setup.py** 安装
[https://github.com/google/mediapipe](https://github.com/google/mediapipe)

# 项目环境
**Python			3.7
Mediapipe     0.8.9.1
Numpy		1.21.6
OpenCV-Python 		4.5.5.64
OpenCV-contrib-Python		4.5.5.64**
![在这里插入图片描述](https://img-blog.csdnimg.cn/e49c1c0fa21c4df08ee58e57ce57f8b4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQklHQk9TU3lpZmk=,size_20,color_FFFFFF,t_70,g_se,x_16)
*实测也支持Python3.8-3.9*
# 代码
## 核心代码
**OpenCV摄像头捕捉部分**：
```python
import cv2

cap = cv2.VideoCapture(0)       #OpenCV摄像头调用：0=内置摄像头（笔记本）   1=USB摄像头-1  2=USB摄像头-2

while True:
    success, img = cap.read()
    imgRGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)       #cv2图像初始化
    cv2.imshow("HandsImage", img)       #CV2窗体
    cv2.waitKey(1)      #关闭窗体
```
**mediapipe 手势识别与绘制**

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
### 视频帧率计算

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

## 完整代码

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
### 项目输出
![在这里插入图片描述](https://img-blog.csdnimg.cn/50e4d32e5f0a444c87cfb92ea3747363.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQklHQk9TU3lpZmk=,size_20,color_FFFFFF,t_70,g_se,x_16)
# 结语
*以此篇文章技术为基础，后续会更新利用此篇基础技术实现的*《**手势控制：音量，鼠标**》
