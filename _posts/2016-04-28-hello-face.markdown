---
layout:     post
title:      "vs+opencv人脸识别"
subtitle:   "人脸识别"
date:       2016-04-20 12:00:00
author:     "wellyoung"
header-img: "img/home-bg-face.jpg"
header-mask: 0.3
catalog:    true
tags:
    - vs
    - opencv
    - 人脸识别
---


![人脸识别](http://upload-images.jianshu.io/upload_images/2484273-0691290095754281?imageMogr2/auto-orient/strip)

曾经有一些问题是关于如何确认本人的笑话，派出所要求一个小伙证明就是本人，证明你妈是你妈。。这种奇葩问题，但是许多陌生场合也有这种尴尬，你如果没有带证件，警察无法看到你的照片，如何确认你就是XX就是之前经常出现的执法矛盾；如果一个人把身份证弄丢了，外面风雪交加，如何给这类人办理酒店入住手续？

在AI技术炙手可热的今天，人脸识别作为确认用户身份的一项技术，从苹果舍弃指纹识别采用人脸识别技术可以发现，人脸识别技术也得到了新的发展。人脸识别的技术实现方式有很多种，本文对Visual Studio配合opencv这种实现方式进行简要概述。

## 一、环境搭建

### 1.Windows10 + vs2015专业版 + OpenCV
- Visual Studio的软件自行百度下载,值得一提的是请针对下载的版本再搜索一次密钥，不然的话只能试用一个月哦。
- OpenCV可进入[官网](https://opencv.org/)直接下载，或想使用[历史版本](https://opencv.org/releases.html)，安装方式直接解压。
![选择Windows包.png](https://upload-images.jianshu.io/upload_images/2484273-851168ae2816bf88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.环境变量
- 快捷键win+e 选择此电脑(计算机)
![右键属性](https://upload-images.jianshu.io/upload_images/2484273-93bdf35a1615b67f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 右键属性->高级系统设置->环境变量->系统变量->找到Path->在变量值中添加相应路径,我的路径是（请根据自己文件路径修改） 
E:\Program Files\opencv\build\x64\vc14\bin 
E:\Program Files\opencv\build\x64\vc15\bin
这次更新发现一直存在的x86文件夹已经删除了，也就是说不支持vs2015的x86编译了，这个问题之后也会强调。另外如果你是vs2013请选择vc12文件夹，如果你是其他更老的vs版本，建议选择其他版本的opencv。 
![添加环境变量](https://upload-images.jianshu.io/upload_images/2484273-be9b68d6a8ac40ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<br>

### 3.打开vs建立一个项目
![新建项目](https://upload-images.jianshu.io/upload_images/2484273-12a95b8413f70148.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![新建项目.png](https://upload-images.jianshu.io/upload_images/2484273-10f44750a554ea70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![新建项.png](https://upload-images.jianshu.io/upload_images/2484273-43da7f7ef2e38867.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![main.cpp.png](https://upload-images.jianshu.io/upload_images/2484273-cd3878b7fec63b72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4.在vs里添加opencv进去
点击视图，在视图下找到属性管理器，点击打开
![视图](https://upload-images.jianshu.io/upload_images/2484273-b40dc943b5ef1c44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后便会有一个属性管理器的窗口了，接下来点开工程文件test，下边会有一个Debug|x64的文件夹，点开，下有名为Microsoft.Cpp.x64.user的文件，右键属性 
![属性管理器](https://upload-images.jianshu.io/upload_images/2484273-178de8a44db57e30.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后选择通用属性下的VC++目录，右边会有包含目录和库目录，点击包含目录，添加以下三条路径，其实这些都是刚才OpenCV相关解压文件所在的目录
E:\Program Files\opencv\build\include
E:\Program Files\opencv\build\include\opencv
E:\Program Files\opencv\build\include\opencv2
这三条路径要依据自己解压OpenCV的路径进行修改
![编辑包含目录](https://upload-images.jianshu.io/upload_images/2484273-eaea6c167b8d4bd8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![自己的路径.png](https://upload-images.jianshu.io/upload_images/2484273-26c00f9fba0bafba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
再点击库目录添加下面一条路径
E:\Program Files\opencv\build\x64\vc14\lib
![库目录点击编辑](https://upload-images.jianshu.io/upload_images/2484273-cee526b054c05ec9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![库目录添加lib](https://upload-images.jianshu.io/upload_images/2484273-33f5550e8bc209c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
点击链接器，选择输入，会在右侧看到附加依赖项，添加下面文件
opencv_world341d.lib  （如果你是3.4.0就改为opencv_world340d.lib以此类推）
说明：这里小编添加的是Debug模式（测试）的，会看到文件的结尾有d， 
假如要添加Release模式（正式上线）的，将d去掉即可 
即opencv_world341.lib
![添加lib](https://upload-images.jianshu.io/upload_images/2484273-cb01cd28edca35e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 5.测试环境是否搭建成功

```
 #include<opencv2\opencv.hpp>
 #include<opencv2\highgui.hpp>
 #include<opencv2\imgproc\imgproc.hpp>

using namespace cv;

int main()
{
	Mat picture = imread("yeah.jpg");//图片必须添加到工程目录下 ,也就是和 main.cpp文件放在一个文件夹下！！！
	imshow("测试程序", picture);
	Mat element = getStructuringElement(MORPH_RECT, Size(15, 15));
	Mat dstimage;
	erode(picture, dstimage, element);
	imshow("腐蚀操作", dstimage);
	waitKey(0);
}
```
按F5或则点击本地Windows调试器运行程序，你会发现报错，需要在debug模式下选择x64，因为我们现在的pc进本都是64位的，x84是32位系统下的
![x64](https://upload-images.jianshu.io/upload_images/2484273-2ecd291023e3e8bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
测试图片的路径
![图片路径](https://upload-images.jianshu.io/upload_images/2484273-dfd0167c4590049d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
喝杯咖啡让程序自己下载
![第一次运行会加载很多库](https://upload-images.jianshu.io/upload_images/2484273-5cf3939a55c668f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果加载不成功可进行以下方式进行调试
1、点调试。

2、然后选项和设置。

3、右边勾上启用源服务器支持。

4、左边点符号。

5、把微软符号服务器勾。

6、运行的时候等一下。
![启用源服务器支持](https://upload-images.jianshu.io/upload_images/2484273-fec24e0c841c5f93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![勾选Microsoft符号服务器](https://upload-images.jianshu.io/upload_images/2484273-2fce0af4e333ebe4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这是程序正常运行的结果，可以看出右边腐蚀后的图片与原图有何区别吗？
![图片对比](https://upload-images.jianshu.io/upload_images/2484273-a2a3ff6d6c574a0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6.人脸识别项目
把刚刚的测试代码删掉注入人脸识别代码

```
#include <iostream>  
#include <opencv2\opencv.hpp>
#include<E:\Program Files\opencv\build\include\opencv2\core\core.hpp>//填写自己路径
#include <opencv2\highgui\highgui.hpp>
#include <opencv2\imgproc/imgproc.hpp>
#include <opencv2\objdetect/objdetect.hpp>

#include <string>  

using namespace cv;
using namespace std;

//#pragma comment(lib, "opencv_world330.lib")

vector<Rect> detectFaces(Mat img_gray) {
	vector<Rect> faces;
	CascadeClassifier faces_cascade;
	
	string xmlPath = "E:\\Program Files\\opencv\\sources\\data\\haarcascades_cuda\\haarcascade_frontalface_alt.xml";//填写自己路径
	if (!faces_cascade.load(xmlPath)) {
		cout << "不能加载指定的xml文件" << endl;
		return faces;
	}

	faces_cascade.detectMultiScale(img_gray, faces, 1.21, 3, 0 | CV_HAAR_SCALE_IMAGE, Size(50, 50), Size(100, 100));
	//faces_cascade.detectMultiScale(img_gray, faces, 1.1, 3, 0,Size(30,30),Size(50,50));

	//faces_cascade.detectMultiScale(img_gray, faces, 1.1, 3, 0, Size(10, 10), Size(100, 100));
	//cout << "222" << endl;
	return faces;
}

void drawFaces(Mat img, vector<Rect> faces) {

	namedWindow("draw faces");
	for (size_t i = 0; i < faces.size(); i++) {
		Point center(faces[i].x + faces[i].width / 2, faces[i].y + faces[i].height / 2);
		ellipse(img, center, Size(faces[i].width / 2, faces[i].height / 1.5), 0, 0, 360, Scalar(0, 255, 0), 2, 8, 0);

	}
	imshow("draw faces", img);

}
void saveFaces(Mat img, Mat img_gray) {
	vector<Rect> faces = detectFaces(img_gray);
	for (size_t i = 0; i < faces.size(); i++)
	{

		stringstream buffer;
		buffer << i;
		string saveName = "facesicon\\" + buffer.str() + ".jpg";
		Rect roi = faces[i];
		imwrite(saveName, img(roi));
	}



}

void detectDrawEyes(Mat img, Mat img_gray) {
	vector<Rect> faces = detectFaces(img_gray);
	CascadeClassifier faces_cascade;
	
	string xmlPath = "E:\\Program Files\\opencv\\sources\\data\\haarcascades_cuda\\haarcascade_eye.xml";//填写自己路径

	for (size_t i = 0; i < faces.size(); i++) {

		Mat faceROI = img_gray(faces[i]);

		CascadeClassifier eyes_cascade;
		eyes_cascade.load(xmlPath);
		vector<Rect> eyes;
		eyes_cascade.detectMultiScale(faceROI, eyes, 1.1, 2, 0 | CV_HAAR_SCALE_IMAGE, Size(30, 30));

		for (size_t j = 0; j < eyes.size(); j++)
		{
			Point eyes_center(faces[i].x + eyes[j].x + eyes[j].width / 2, faces[i].y + eyes[j].y + eyes[j].height / 2);
			int r = cvRound((eyes[j].width + eyes[j].height)*0.25);
			circle(img, eyes_center, r, Scalar(255, 0, 0), 1, 8, 0);
		}


	}
	namedWindow("detectDrawEyes");

	Mat temImage = img, dstImage1;
	resize(temImage, dstImage1, Size(temImage.cols / 2, temImage.rows / 2), 0, 0, INTER_LINEAR);
	imshow("detectDrawEyes", dstImage1);

}
int main() {

	/*


	// 读入一张图片（游戏原画）
	Mat img = imread("pic.jpg");
	// 创建一个名为 "游戏原画"窗口
	namedWindow("游戏原画");
	// 在窗口中显示游戏原画
	imshow("游戏原画", img);
	// 等待6000 ms后窗口自动关闭
	waitKey(6000);
	int a;
	cin >> a;
	return 0;
	*/


	Mat img = imread("xj.jpg");//找一张有人脸的图片（毕业照之类的）
	Mat img_gray;
	namedWindow("原画");
	imshow("原画", img);


	cvtColor(img, img_gray, COLOR_BGR2GRAY);



	equalizeHist(img_gray, img_gray);


	namedWindow("灰度图");
	imshow("灰度图", img_gray);

	vector<Rect> faces = detectFaces(img_gray);
	saveFaces(img, img_gray);

	drawFaces(img, faces);

	detectDrawEyes(img, img_gray);
	waitKey(0);
	return 0;
}
```
![人脸识别结果](https://upload-images.jianshu.io/upload_images/2484273-9e85d69d178300b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

换一张图也许就是另外一回事了

![图片样本不好](https://upload-images.jianshu.io/upload_images/2484273-a5e876141b34983c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)