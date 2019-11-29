# MAiEye

**May I** give<br>
**Maplestory** <br>
**Ai** <br>
and <br>
**Eye?** <br>

....

<br>


![1](./readmeImage/1_1.png)

<br>
<br>


![1](./readmeImage/1_2.png)


<br>
<br>


![1](./readmeImage/1_3.png)


<br>
<br>


![1](./readmeImage/2_1.png)
![1](./readmeImage/2_2.png)

<br>
<br>

## How to Run? (Windows Only)


<br>

**Dependencies**

CV2 모듈이 설치되어 있지 않다면 아래의 명령어를 통하여 opencv-contrib 을 설치하셔야 합니다.
```
(base) C: [your own path] \MAiEye\Project> pip install opencv-contrib-python
```

### Start! data generator

data generator 은 매우 간단한 두 개의 모듈로 이루어져 있습니다.
1. Multi-RoI(Region of Interest)Extraction 모듈 - Image Detection 모델에 사용되는 데이터를 모으는 역할의 모듈
2. MapCapture 모듈 - Image Classification 모델에 사용되는 데이터를 모으는 역할의 모듈입니다.


<br>

커맨드 실행창을 키고, 해당 모듈들의 상위 디렉토리로 이동해주세요. git 저장소에서 다운로드받았다면, MAiEye\Project 폴더일 것입니다.

<br>

```
(base) C: [your own path] \MAiEye\Project> python startdatagenerator.py
```

<br>

아차, 단순히 python startdatagenerator.py 로 실행시킨다고 바로 뚝딱뚝딱 실행되는 것이 아니고, 추가적인 명령을 내려 주셔야 합니다. 아래에서 천천히 알아보도록 해요.

<br>

**Argument Example**

```
python startdatagenerator.py
--labeltype map
-v C: [your own root] \perion\move
-sp C: [your own root] \perion
-ms 1
--label perion
--imagesize 80
```

<br>

우선 아래는 실제로 모듈 안에 작성된 python 코드입니다. 하나씩 속성을 살펴보도록 하겠습니다.

<br>

```python

ap = argparse.ArgumentParser()
ap.add_argument("-v", "--video", type=str, help="[string] : path to input video file")
ap.add_argument("-s", "--labeltype", type=str, help="[string] : type of video. Is it map of mob?")
ap.add_argument("-l", "--label", type=str, help="[string] : label name")
ap.add_argument("-t", "--tracker", type=str, help="[string] : BOOSTING, MIL, KCF, TLD, MEDIANFLOW, GOTURN, MOSSE, CSRT")
ap.add_argument("-ms", "--milliseconds", type=int, help="[int] : save image per n milliseconds")
ap.add_argument("-sp", "--savepath", type=str, help="[string] : saving path (absolute path , 절대경로), 경로가 존재하지 않으면 저장되지 않을 수 있어요!")
ap.add_argument("-is", "--imagesize", type=int, help="[int] : map 옵션 을 선택할 때에만 지정. N * N 으로 사진이 저장됨")
args = vars(ap.parse_args())

```

<br>

**Arguments**

1. -v : Source 가 될 video file 의 절대 경로를 지정합니다. 제가 상대경로 규칙을 잘 모르기도 하고, 모듈 외부에 있는 파일을 쉽게 불러오기 위하여 절대경로 규칙을 사용하여 지정해 주는 것을 권장합니다.

2. -s : Label type 에 대한 속성입니다. 이 속성에 대하여 설명하기 위해 간단히 덧붙이자면, 이 프로젝트에서 Maplestory 의 마을 판단 은 Image Classification (이미지 분류) 기술을, 몬스터 식별 과 지형 식별 등의 경우에는 Object Detection (이미지 감지) 기술을 사용합니다. 만약 이 속성이 Map 으로 되어 있다면, 별도의 Annotation XML 파일을 생성하지 않습니다.

3. -l : Label 에 대한 속성입니다. -s 가 map 일 때에는, -l 에 지정된 문자열로, savepath 내부에 경로를 만들고, 그 안에 이미지를 저장합니다. 만약 -s 가 map 이고, -sp 가 C:/folder 이고, -l 이 example 이라면, C:/folder/example 에 이미지를 저장합니다. 이렇게 설계한 이유는 classification 모델들의 학습이 보통 이런 디렉토리 구조를 선호하기 때문입니다.

4. -t : tracker 의 속성을 지정합니다. 이 속성은 -s 가 mob 또는 land 로 지정되어 있을 때에만 사용하면 됩니다. tracker 은 움직이는 영상에서도 메이플스토리에서 어떤 몬스터 또는 지형을 졸졸 따라다니면서 해당 영역을 지정해 줍니다. 이 tracker 은 고전적인 알고리즘들로 구성되어 있습니다. CSRT 가 기본적으로 사용됩니다.

5. -ms : milli second 의 약자로, 동영상을 몇 밀리초마다 캡쳐를 해낼 것인지 지정합니다.

6. -is : image size 의 약자로, 어느 크기로 영상을 캡쳐할지 지정합니다. 이 속성은 -s 가 map 일 때에만 지정해 주면 됩니다. mob 이나 land 와 같은 경우, object detection model 들 중 yolo 모델을 통과합니다. 즉, yolo 모델에서 권장하는 size 가 있는 것 같다는 판단 하에 위 입력과 상관없이 고정된 크기로 잘라내기 때문입니다.

7. -sp : saving path 의 약자로, 동영상을 저장할 위치를 의미합니다.

<br>

**Arguments Summary**

| -s type | -s : map | -s : mob | -s : land (아직 구현안됨) |
|:-------:|:--------:|:--------:|:---------:|
| **call module** | map capture module | RoI Extraction module | RoI Extraction module |
| -v | Need(경로지정) | Need(영상지정) | Need(영상지정) |
| -l | Need | Need | Need |
| -t | x | Need | Need |
| -ms | Need | Need | Need |
| -sp | Need | Need | Need |
| -is | Need | x | x |


<br>
<br>

### Start! classification model trainig

아직 모듈화가 이루어지지 않았습니다.

- 아직 학습이 중단되었을 때 다시 로드하는 것이 원활하지 않습니다.
- 아직 Class 의 개수를 유동적으로 조절하지 못합니다.
- 아직 label 을 기준으로 test, train, validation set 를 자동생성해내지 못합니다.
- 추후에는 이 Classification 모델 학습을 모듈로 만들 예정입니다.
- 아직 test, train, validation set 의 경로를 하드코딩해주어야 합니다.

<br>

아래 파일의 소스코드를 본인의 환경에 맞게 수정해 주세요. <br>
startclassificationmodeltraining.py
```python
    train_directory = 'C: [your own path] /train'
    valid_directory = 'C: [your own path] /valid'
    test_directory  = 'C: [your own path] /test'
    checkpoint_path = "trainingcheckpoint/cp.ckpt"
```

<br>

실행명령은 다음과 같습니다.

```
(base) C: [your own path] \MAiEye\Project\JanghooModule_RunWithMapleGUI\Janghoo_Model> python startclassificationmodeltraining.py
```

<br>
<br>

### Before Final Starting

우선, 저희는 학습을 위해 darkflow 라는 open source 를 사용합니다. <br>

https://github.com/thtrieu/darkflow/ 에 원본이 존재하지만, MAiEye 에는 기본적으로 설치되어 있습니다. <br>

```
(base) C: [your own path] \darkflow-master> pip install -e .
```

<br>

Darkflow 는 Tensorflow 설치를 필요로 합니다. Tensorflow 1.4 이하가 설치되어 있으신가요? <br>


**설치되어 있는지 확인하기**

1. python 실행
```
(base) C: [path]> python
```

<br>

2. tensorflow import 해보기
```
>>> import tensorflow   
```
여기서 오류가 나면 설치되어있지 않은 것입니다. 설치부터 진행해 주세요.

<br>

3. tensorflow version 확인하기
```
(base) C: [path]> python
>>> import tensorflow
>>> tensorflow.__version__ 
```
이것을 했을 때 2.0 이상의 결과가 출력되면, 실행되지 않습니다! 제거 후 다시 설치해야 하니, 제거부터 진행해 주세요.

<br>

**기존에 설치되어 있던 Tensorflow 2.0 이상 제거하기**
```
(base) C: [path]> pip uninstall tensorflow
(base) C: [path]> pip uninstall tensorflow-gpu
```

<br>

**Tensorflow 새로 설치하기**
```
(base) C: [path]> pip install tensorflow==1.4.0
```


이제 준비를 마쳤습니다!

<br>
<br>

### Run Final Model!

아직 모듈화가 이루어지지 않았습니다. <br>
하지만 데모 버젼을 실행해 보는 것은 어렵지 않습니다. <br>


- 아직 학습이 적절히 이루어지지 않았습니다.
- Yolo 모델은 특정 유닛에 한해서 작동합니다.
- argument 를 통한 조절이 불가능합니다.


주의 : Github 용량 제한 때문에, 학습된 classification 모델 파일이 존재하지 않으므로, 사용 이전에 직접 제작해야 합니다. <br>
학습시킨 모델 (.h5 파일) 은, 지정된 이름으로 \MAiEye\Project\JanghooModule_RunWithMapleGUI\Janghoo_Model] 에 넣어 주면 됩니다.

실행명령은 다음과 같습니다.

```
(base) C: [your own path] \MAiEye\Project> python runmodel.py
```

<br>
<br>
<br>

### 모듈만 단독적으로 사용하기

<br>

관심영역 추출 모듈 단독으로 사용하기

```
C: [your own path] \MAiEye\Project\JanghooModule_RoIExtraction> python roi_multi_tracking_in_video.py 
-t CSRT -sp C: [your own path] -ms 5 -v C: [your own path]\[your own video name].mp4 --label stump
```

<br>
<br>

동영상화면 캡쳐 모듈 단독으로 사용하기

```
C: [your own path] \MAiEye\Project\JanghooModule_MapCapture> python capture_map.py 
-sp C: [your own path] -v C:[your own path]\[your own video name or folder]  
--label henesis --imagesize 80 --labeltype map -ms 2
```

<br>

```python
    ap = argparse.ArgumentParser()
    ap.add_argument("-v", "--video", type=str, help="[string] : path to input video file")
    ap.add_argument("-t", "--tracker", type=str,
                    help="[string] : BOOSTING, MIL, KCF, TLD, MEDIANFLOW, GOTURN, MOSSE, CSRT")
    ap.add_argument("-l", "--label", type=str, help="[string] : label name")
    ap.add_argument("-ms", "--milliseconds", type=int, help="[int] : save image per n milliseconds")
    ap.add_argument("-sp", "--savepath", type=str, help="[string] : saving path (absolute path , 절대경로)")
    args = vars(ap.parse_args())
```

<br>
<br>

### 첨언

- 왜 굳이 모듈화?
> 이 프로젝트의 최종 목표는 강화학습입니다. 아타리게임의 강화학습은 비전 정보를 기반으로 하였습니다. 이에 영감을 얻어, 우리가 두 눈으로 보는 정보를 디지털로 바꾸어 주는 중간다리 역할을 할 프로그램이 있어야 하지 않을까 하는 생각이 들었습니다. 다양한 강화학습을 시도해보기 위해, 비전 데이터셋을 모으는 것은 정말 힘든 일입니다. 하지만 우리는 게임 환경에서, 동영상이라는 좋은 비전 데이터가 존재합니다. <br> 모듈화가 되어있지 않다면, 이것은 MAiEye 프로젝트 안에서 호출되어야만 합니다. 하지만 저는 이것이 메이플스토리에만 쓰이길 바라지는 않습니다. 대부분정말 쉬운 코드이고, 한정적인 상황에서만 사용 가능하며, 누구나 만들 수 있는 모듈이긴 하지만 조금이라도 개발 시간이 단축된다면 기쁠 것 같습니다.

<br>
<br>
<br>

## 지금 이 프로젝트를 위해 하고 있는 일

1. 모델 내부에 대한 보다 정량적인 이해를 위해 선형대수 공부 중.
2. 오픈소스의 라이선스와 깃에 대한 이해도를 높이기 위해 오픈소스 수업 듣는 중.
3. 동아리에서 RL / CV / GAN 스터디 할 예정.
4. 파이썬 모듈을 만드는 약간의 틀이 있다는 것을 알게 됨. 이 형식에 걸맞게 재구성해볼 예정
> https://python-guide-kr.readthedocs.io/ko/latest/writing/structure.html


- OSS 조별 과제로 몬스터 뿐만 아니라 광석과 식물을 탐지시키는 기능 추가 예정

- 2019/10/6
> - OSS팀원 모임


##2019.11.05 오픈소스 팀원 참여
