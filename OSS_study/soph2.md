##2019.11.09
- readme.md 팀원 모임과 이후 프로젝트에 관한 내용 추가

##2019.11.25
- readme.md opencv관련 설치 명령문 추가

##2019.11.26
- gitkraken 프로그램을 이용하여 clone 다시 함

##2019.11.28

darkflow-master 폴더를 darkflow로 이름 변경하고 기존 darkflow를 darkflow222로 변경해서 실행하고 있음

--------------------

(base) C:\Users\LG\Documents\GitHub\MAiEye\Project\JanghooModule_RoIExtraction>python roi_multi_tracking_in_video.py -t CSRT -sp C:\Users\LG\Documents\GitHub\MAiEye\Project\ -ms 5 -v C:\Users\LG\Desktop\KakaoTalk_Video_20191126_0026_05_032.mp4 --label stump



run module separately



size is :  416 416


현재 저장된 boundary box 개수 : 0
Select a ROI and then press SPACE or ENTER button!
Cancel the selection process by pressing c button!



object 선택을 마치고 tracking 을 시작하려면 q를 누르세요.
다음 object 를 선택하려면 아무 키나 누르세요.


현재 저장된 boundary box 개수 : 1
Select a ROI and then press SPACE or ENTER button!
Cancel the selection process by pressing c button!



object 선택을 마치고 tracking 을 시작하려면 q를 누르세요.
다음 object 를 선택하려면 아무 키나 누르세요.


현재 저장된 boundary box 개수 : 2
Select a ROI and then press SPACE or ENTER button!
Cancel the selection process by pressing c button!



object 선택을 마치고 tracking 을 시작하려면 q를 누르세요.
다음 object 를 선택하려면 아무 키나 누르세요.


현재 저장된 boundary box 개수 : 3
Select a ROI and then press SPACE or ENTER button!
Cancel the selection process by pressing c button!



object 선택을 마치고 tracking 을 시작하려면 q를 누르세요.
다음 object 를 선택하려면 아무 키나 누르세요.
Selected bounding boxes : [(0, 0, 0, 0), (152, 132, 164, 163), (15, 303, 29, 29), (58, 301, 31, 36)]
Traceback (most recent call last):
  File "roi_multi_tracking_in_video.py", line 231, in <module>
    flow(videoPath, trackerType, labelName, savefolder, waitingTime)
  File "roi_multi_tracking_in_video.py", line 78, in flow
    multiTracker = cv2.MultiTracker_create()
AttributeError: module 'cv2.cv2' has no attribute 'MultiTracker_create'

--------------------
위의 AttributeError 오류 뜸
```
(base) C:\Users\LG\Documents\GitHub\MAiEye\Project\JanghooModule_RoIExtraction>pip install opencv-contrib-python
```
위의 명령어 사용하여 모듈 설치

##2019.11.30

--------------------------
(base) C:\Users\LG>python C:\Users\LG\Documents\GitHub\MAiEye\Project\runmodel.py
Traceback (most recent call last):
  File "C:\Users\LG\Documents\GitHub\MAiEye\Project\runmodel.py", line 16, in <module>
    filename = sct.shot(mon=-1, output='JanghooModule_RunWithMapleGUI/tmpfolder/fullscreen.png')
  File "C:\ProgramData\Anaconda3\lib\site-packages\mss\base.py", line 140, in shot
    return next(self.save(**kwargs))
  File "C:\ProgramData\Anaconda3\lib\site-packages\mss\base.py", line 130, in save
    to_png(sct.rgb, sct.size, level=self.compression_level, output=output)
  File "C:\ProgramData\Anaconda3\lib\site-packages\mss\tools.py", line 52, in to_png
    with open(output, "wb") as fileh:
FileNotFoundError: [Errno 2] No such file or directory: 'JanghooModule_RunWithMapleGUI/tmpfolder/fullscreen.png'
---------------------------
파일이 있는데 없다고 에러 뜸
"C:\Users\LG\Documents\GitHub\MAiEye\Project\JanghooModule_RunWithMapleGUI\tmpfolder\fullscreen.png"

해결!! 현재 위치를 project에 있어야함 runmodel.py:16에서 사용하는 mss().shot이 상대주소를 사용하여 움직이기 때문에 찾지 못한 거였음

---------------------------------
(base) C:\Users\LG>python C:\Users\LG\Documents\GitHub\MAiEye\Project\runmodel.py
Traceback (most recent call last):
  File "C:\Users\LG\Documents\GitHub\MAiEye\Project\runmodel.py", line 16, in <module>
    filename = sct.shot(mon=-1, output='JanghooModule_RunWithMapleGUI/tmpfolder/fullscreen.png')
  File "C:\ProgramData\Anaconda3\lib\site-packages\mss\base.py", line 140, in shot
    return next(self.save(**kwargs))
  File "C:\ProgramData\Anaconda3\lib\site-packages\mss\base.py", line 130, in save
    to_png(sct.rgb, sct.size, level=self.compression_level, output=output)
  File "C:\ProgramData\Anaconda3\lib\site-packages\mss\tools.py", line 52, in to_png
    with open(output, "wb") as fileh:
FileNotFoundError: [Errno 2] No such file or directory: 'JanghooModule_RunWithMapleGUI/tmpfolder/fullscreen.png'

(base) C:\Users\LG>cd C:\Users\LG\Documents\GitHub\MAiEye\Project

(base) C:\Users\LG\Documents\GitHub\MAiEye\Project>python runmodel.py
JanghooModule_RunWithMapleGUI/tmpfolder/fullscreen.png
960 540
1920 1080
Select a ROI and then press SPACE or ENTER button!
Cancel the selection process by pressing c button!
Selected bounding box : xmin / ymin / width / height : (401, 205, 315, 211)
Using TensorFlow backend.
now model loading
WARNING:tensorflow:From C:\ProgramData\Anaconda3\lib\site-packages\tensorflow\python\ops\init_ops.py:97: calling GlorotUniform.__init__ (from tensorflow.python.ops.init_ops) with dtype is deprecated and will be removed in a future version.
Instructions for updating:
Call initializer instance with the dtype argument instead of passing it to the constructor
WARNING:tensorflow:From C:\ProgramData\Anaconda3\lib\site-packages\tensorflow\python\ops\init_ops.py:1251: calling VarianceScaling.__init__ (from tensorflow.python.ops.init_ops) with dtype is deprecated and will be removed in a future version.
Instructions for updating:
Call initializer instance with the dtype argument instead of passing it to the constructor
WARNING:tensorflow:From C:\ProgramData\Anaconda3\lib\site-packages\tensorflow\python\ops\init_ops.py:97: calling Zeros.__init__ (from tensorflow.python.ops.init_ops) with dtype is deprecated and will be removed in a future version.
Instructions for updating:
Call initializer instance with the dtype argument instead of passing it to the constructor
2019-11-30 00:42:10.089498: I tensorflow/core/platform/cpu_feature_guard.cc:142] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2
2019-11-30 00:42:10.236066: W tensorflow/core/framework/allocator.cc:107] Allocation of 47316992 exceeds 10% of system memory.
2019-11-30 00:42:10.416873: W tensorflow/core/framework/allocator.cc:107] Allocation of 47316992 exceeds 10% of system memory.
2019-11-30 00:42:10.965332: W tensorflow/core/framework/allocator.cc:107] Allocation of 47316992 exceeds 10% of system memory.
2019-11-30 00:42:11.015302: W tensorflow/core/framework/allocator.cc:107] Allocation of 47316992 exceeds 10% of system memory.
2019-11-30 00:42:11.067258: W tensorflow/core/framework/allocator.cc:107] Allocation of 47316992 exceeds 10% of system memory.
path before import TFNet :  C:\Users\LG\Documents\GitHub\MAiEye\Project
 ['C:\\Users\\LG\\Documents\\GitHub\\MAiEye\\Project', 'C:\\ProgramData\\Anaconda3\\python37.zip', 'C:\\ProgramData\\Anaconda3\\DLLs', 'C:\\ProgramData\\Anaconda3\\lib', 'C:\\ProgramData\\Anaconda3', 'C:\\ProgramData\\Anaconda3\\lib\\site-packages', 'c:\\users\\lg\\documents\\github\\darkflow-master\\darkflow-master', 'C:\\ProgramData\\Anaconda3\\lib\\site-packages\\win32', 'C:\\ProgramData\\Anaconda3\\lib\\site-packages\\win32\\lib', 'C:\\ProgramData\\Anaconda3\\lib\\site-packages\\Pythonwin', 'C:\\Users\\LG\\Documents\\GitHub\\MAiEye\\Project\\Darkflow']
Traceback (most recent call last):
  File "runmodel.py", line 61, in <module>
    from darkflow.net.build import TFNet
ModuleNotFoundError: No module named 'darkflow.net'
-----------------------------------
darkflow.net 오류 뜸 -> 인터넷에서  protobuf==3.5.2을 설치하라 나옴
-----------------------------------
(base) C:\Users\LG\Documents\GitHub\MAiEye\Project>pip install protobuf==3.5.2
Collecting protobuf==3.5.2
  Downloading https://files.pythonhosted.org/packages/c0/bc/c538b14c738366284faffd276ee5cdc053e4a999490a62c74bbc265a7635/protobuf-3.5.2-py2.py3-none-any.whl (388kB)
     |████████████████████████████████| 389kB 435kB/s
Requirement already satisfied: six>=1.9 in c:\programdata\anaconda3\lib\site-packages (from protobuf==3.5.2) (1.12.0)
Requirement already satisfied: setuptools in c:\programdata\anaconda3\lib\site-packages (from protobuf==3.5.2) (41.4.0)
ERROR: tensorflow 1.14.0 has requirement protobuf>=3.6.1, but you'll have protobuf 3.5.2 which is incompatible.
ERROR: tensorboard 1.14.0 has requirement protobuf>=3.6.0, but you'll have protobuf 3.5.2 which is incompatible.
Installing collected packages: protobufr
  Found existing installation: protobuf 3.10.0
    Uninstalling protobuf-3.10.0:
      Successfully uninstalled protobuf-3.10.0
Successfully installed protobuf-3.5.2
-------------------------------------
저렇게 하면 
-------------------------------------
TypeError: __init__() got an unexpected keyword argument 'serialized_options'
-------------------------------------
위와 같은 에러뜸
근데 또 인터넷 찾아보면 protobuf버전문제라고 함,,

!!!!!!!!그냥 다시 protobuf부분 버전 3.10.0으로 올리고 project에 있는 darkflow폴더이름을 Darkflow로 바꿔주니 해결

##2019.12.02
"C:\Users\LG\Documents\GitHub\MAiEye\Project\Darkflow\labels-maple.txt" 광물이랑 식물 label추가

python flow --model cfg\tiny-yolo.cfg --load bin\yolov2.weights
실행 했더니 성공한거같음!!

python flow --imgdir sample_img\ --model cfg\tiny-yolo.cfg --load bin\yolo.weights --gpu 1.0
했더니 sample_img\out에 사진 생김
python flow --imgdir sample_img\ --model cfg\tiny-yolo.cfg --load bin\yolo.weights --json
했더니 json생성됨

flow --model cfg/yolo-new.cfg 명령어에서 yolo-new는 새롭게 만들어야하는 cfg파일인거같음
일단 저 명령어에서는 
FileNotFoundError: [Errno 2] No such file or directory: 'cfg\\yolo-new.cfg'
이런 오류메세지뜸

flow --model cfg\yolo.cfg 로 대체해서 실행해봄
example 실행 위해 label.txt에서 boat지움
loader.py:121 self.offset=16->20으로 수정함

##2019.12.03
cfg파일에 region-classes수를 아까 label.txt파일에 있는 클래스 수에 맞게 바꿔줌
&&	convolutional-filter를 수에 맞게 바꿔줌 (([convolutional] layer (the second to last layer) to num * (classes + 5)))

annotation은 xml파일
load -> weights파일로 학습된 결과가 저장됨, 학습시키다가 중간에 그만 두고 다시 그 중지된 그때부터 다시 학습시킬 수 있음

```
python flow --model cfg/mineral.cfg --train --annotation cleandata_oneclass_annotation --dataset cleandata_oneclass_image --save 500
```
명령어 사용해서 학습

##2019.12.04
push failed 문제로 다시 clone함
startclassificationmodeltraining.py에서 
train_directory = 'C:/Users/LG/Documents/GitHub/MAiEye/Project/MAiEye/Data/maplestory_map_source/train'로 변경

새폴더 annotation이랑 img만들고 flow darkflow로 수정
darkflow 설치 명령어 추가해야함(readme.md에)(darkflow에서 pip install . e)

```
(base) C:\Users\LG\Documents\GitHub\MAiEye\Project>python startdatagenerator.py -s mob -t CSRT -sp C:\Users\LG\Desktop\2019_1\오픈소스및개론\xml -ms 3 -v C:\Users\LG\Desktop\2019_1\오픈소스및개론\video_source\heart_mineral_2.mp4 --label heart_mineral
```
이 명령어를 사용하여 datagenerate함

그러면 이걸로 데이터 만들고 darkflow로 학습?

//(base) C:\Users\LG\Documents\GitHub\MAiEye\Project\darkflow>python flow --model cfg/yolo-voc-maple.cfg --demo image/heart_mineral_1.mp4 --saveVideo 이걸로 데모 가능?

- Darkflow 바꾸기, .h5 파일 깃허브에서 받기

AttributeError: 'NoneType' object has no attribute 'shape' 	//xml파일이랑 jpg파일 수랑 안맞아서! 
원래 datagenerator하면 jpg랑 xml둘다 저장되어야함 -> 근데 왜인지 모르게 저거start명령어는 xml만 저장됨..
```
python startdatagenerator.py -s mob -t CSRT -sp C:\Users\LG\ex\video_saving_path -ms 3 -v C:\Users\LG\ex\video_source\heart_mineral_1.mp4 --label heart_mineral
```
이걸로 하니까 됨


최종정리!!!
0) cfg파일에서 class수(labels.txt에 적혀져있는 수)랑 filter수 알맞게 수정하기
1) datagenerator로 데이터 수집(xml,jpg저장) 후 -> darkflow로 학습시키기
((labels.txt에 있는 클래스이름이랑 데이터 저거모을때 이름이랑 똑같아야 됨! startdatagenerator.py --label 이름!!
```
python startdatagenerator.py -s mob -t CSRT -sp C:\Users\LG\ex\video_saving_path[생성될 xml,jpg파일 위치] -ms 3 -v C:\Users\LG\ex\video_source\heart_mineral_1.mp4[데이터 생성할 동영상] --label heart_mineral[클래스 이름]
```
1-1)생성된 xml파일을 darkflow/annotation 폴더로 이동하기
1-2)생성된 jpg파일을 darflow/image 폴더로 이동하기
2) 생성된 이미지파일과 xml파일을 이용하여 darkflow로 학습시키기
```
python flow --model cfg\maple.cfg --train --annotation annotation --dataset image --save 30 --lr 0.0001 --load -1
```
--lr learning rate 0.0001로 작게하면 금방금방 loss가 줄어들음 --load -1은 weights 파일 저장시키기 위해서
3) pb파일 생성하기
```
## Saving the lastest checkpoint to protobuf file
flow --model cfg/maple.cfg --load -1 --savepb
```
4) .h5파일인가 그거 사용해서 runmodel.py 실행하기
google drive에서 h5파일 다운받기


!!TO DO LIST!!
h5파일 어떻게 변형하는지 알아보기, 