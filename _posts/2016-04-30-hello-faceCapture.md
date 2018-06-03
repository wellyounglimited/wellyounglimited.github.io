---
layout:     post
title:      "vs+opencv人脸动态识别"
subtitle:   "人脸动态识别"
date:       2016-04-30 12:00:00
author:     "wellyoung"
header-img: "img/home-bg-face2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - vs
    - opencv
    - 人脸识别
---


![人脸动态识别](http://upload-images.jianshu.io/upload_images/2484273-6ef51c75477b139a.gif?imageMogr2/auto-orient/strip)

上节我们讲了vs+OpenCV的配置以及人脸静态识别，这次我们讲讲人脸动态识别，因为在实际应用场景中人脸动态识别（通过摄像头捕捉实时画面）是较常用的。

## 一、环境搭建
### 1.前期的环境搭建可以参考[上一篇文章](https://wellyounglimited.github.io/2016/04/20/hello-face/)
### 2.新建一个项目
- 这里我们直接在原来的项目上新建一个文件，就不用再配与OpenCV的环境了，并把自己给为main，先前的改为main1（或者其他只要不是main）

### 3.人脸动态识别代码


```
#include <iostream>  
#include <opencv2\opencv.hpp>
#include <E:\openvc\build\include\opencv2\core\core.hpp>
#include <opencv2\highgui\highgui.hpp>
#include <opencv2\imgproc/imgproc.hpp>
#include <opencv2\objdetect/objdetect.hpp>

#include <string>  

using namespace cv;
using namespace std;

	String face_cascade_name = "E:\\openvc\\sources\\data\\haarcascades\\haarcascade_frontalface_alt.xml";
	String eyes_cascade_name = "E:\\openvc\\sources\\data\\haarcascades\\haarcascade_eye.xml";
	CascadeClassifier face_cascade, eyes_cascade;//定义两个级联分类器 分别用于检测faces eyes；
	string window_name = "Capture-Facs detection";
	void detectAndDisplay(Mat &frame) {

		vector<Rect> faces;
		Mat frame_gray;
		cvtColor(frame, frame_gray, COLOR_BGR2GRAY);

		equalizeHist(frame_gray, frame_gray);
		face_cascade.detectMultiScale(frame_gray, faces, 1.21, 2, 0 | CV_HAAR_SCALE_IMAGE, Size(30, 30));
		for (size_t i = 0; i < faces.size(); i++) {
			Point lefttop(faces[i].x , faces[i].y);
			Point reightbotomm(faces[i].x + faces[i].width, faces[i].y + faces[i].height);

		//	ellipse(frame, center, Size(faces[i].width / 2, faces[i].height / 1.5), 0, 0, 360, Scalar(0, 255, 0), 2, 8, 0);
			rectangle(frame, lefttop, reightbotomm,  Scalar(0, 255, 0), 1, 16, 0);
			putText(frame,"face",lefttop,1,1, Scalar(255, 0, 0),1,16);

			for (size_t i = 0; i < faces.size(); i++) {

				Mat faceROI = frame_gray(faces[i]);

		
				eyes_cascade.load(eyes_cascade_name);
				vector<Rect> eyes;
				//eyes_cascade.detectMultiScale(faceROI, eyes, 1.1, 2, 0 | CV_HAAR_SCALE_IMAGE, Size(30, 30));
				eyes_cascade.detectMultiScale(faceROI, eyes, 1.1, 3, 0 | CV_HAAR_SCALE_IMAGE, Size(30, 30), Size(100, 100));
				for (size_t j = 0; j < eyes.size(); j++)
				{
					Point eyes_center(faces[i].x + eyes[j].x + eyes[j].width / 2, faces[i].y + eyes[j].y + eyes[j].height / 2);
					Point eyes_title(faces[i].x + eyes[j].x, faces[i].y + eyes[j].y);
					int r = cvRound((eyes[j].width + eyes[j].height)*0.25);
					putText(frame, "eyes", eyes_title, 1, 1, Scalar(255, 0, 0), 1, 16);
					circle(frame, eyes_center, r, Scalar(255, 0, 0), 1, 16, 0);
				}
			}
		}
		imshow(window_name, frame);
	}

int main() {
	VideoCapture capture;//定义摄像头
	Mat frame;//用于保存涉嫌昂投诉获取的每一帧画面
	//--1 用xml文件初始化级联分类器
	if (!face_cascade.load(face_cascade_name)) {
		printf("error 1oad file");
		return -1;
	}
	if (!eyes_cascade.load(eyes_cascade_name)) {
		printf("error 1oad file");
		return -1;
	}

	namedWindow(window_name,0);
	resizeWindow(window_name,1280,720);
	//打开摄像头
/*
存储视频
*/
	//string outputfile = "E://out.flv";
	//int w = static_cast<int>(capture.get(CV_CAP_PROP_FRAME_WIDTH));
	//int h = static_cast<int>(capture.get(CV_CAP_PROP_FRAME_HEIGHT));
	//Size S(w, h);
	//double r = capture.get(CV_CAP_PROP_FPS);

	//VideoWriter write;

	//write.open(outputfile,-1,r,S,true);
	capture.open(0);
	if (capture.isOpened()) {
		for (;;) {
			//capture >> frame;//frame将帧画面赋值给frame
			 capture.read(frame);
			//imshow(window_name, frame);

			if (!frame.empty()) {

				detectAndDisplay(frame);
			//	write.write(frame);
			}
			else {
				printf("no frame");
			}
			int c=waitKey(10);
			if ((char)c == 'c') {
			
				break;
			}
		}
	}
	capture.release();
	//write.release();
	cvDestroyAllWindows();
	return 0;
}

```

### 4.可调参数
- 由于电脑硬件差距可能会导致运行后卡死，这时我们可以选择调整参数，让其正常运行

```
//int c=waitKey(10);
int c=waitKey(100);
//10代表10毫秒，1秒=1000毫秒，所以每秒刷新100次,也就是100帧，相对于这个程序每秒运行100次并把输出（人脸实时绘画出来），对计算性能要求较高，我们把参数改为100，每秒刷新10次，但会有少许卡顿，选择最适合自己的参数即可

```

### 5.实现原理
- 实现原理其实和静态的一样，动态的利用摄像头捕捉到图像让后以每秒多少次的速度向工程输入图片，让后程序运行后输出实时标记出人脸，这样看起来就是连贯的画出人脸了。

### 6.拓展
- 用摄像头捕捉到的视频流可以实现人脸动态识别，那如果换成一段录好的视频是否可以实现呢

![人脸动态识别](https://upload-images.jianshu.io/upload_images/2484273-f87301bde86719dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
