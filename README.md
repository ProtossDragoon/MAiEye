# MAiEye

**May I** give<br>
**Maplestory** <br>
**Ai** <br>
and <br>
**Eye?** <br>

....

<br>

이 프로젝트는 https://github.com/yangjae-ai-school/Individual-Project (현재는 폐쇄됨) 에서 옮겨진 것으로, <br>
저장소가 완전히 꼬여 고장나버렸기 때문에 부득이 이사하게 되었음을 알립니다. <br>


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


### Command

<br>

**Start! data generator**

data generator 은 매우 간단한 두 개의 모듈로 이루어져 있습니다. 첫째는 Multi-RoI(Region of Interest)Extraction 모듈 - Image Detection 모델에 사용되는 데이터를 모으는 역할의 모듈이고, 둘째는 MapCapture 모듈 - Image Classification 모델에 사용되는 데이터를 모으는 역할의 모듈입니다.<br>
커맨드 실행창을 키고, 해당 모듈이 있는 경로로 이동해주세요. git 저장소에서 다운로드받았다면, MAiEye\Project 폴더 안에 있을 겁니다.

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

**Start! classification model trainig**

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

**Before Final Starting**

우선, darkflow 에서 제공하는 cython utils 이라는 것이 전역에 설치가 되어 있어야 합니다. <br>

https://github.com/thtrieu/darkflow/archive/master.zip 다운로드 하고 압축풀기를 합니다. <br>

```
(base) C: [your own path] \darkflow-master> pip install -e .
```

<br>

이제 준비를 마쳤습니다!

<br>
<br>

**Run Final Model!**

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

## Folder - Study

 - 프로젝트에 필요한 공부를 한 내용들을 정리해 담았습니다. (저장소가 변경되면서 그냥 포기했습니다.)
 
| Title | Subject | 학습경로 | 정리여부 | 학습 성취도 |
|---|:---:|:---|:---:|:---:|
| Basics of Data Analysis | ML | School / Sejong Univ (pf. Jaewook Song) | x | 100% |
| Deep Leraning Zero to All 1 | AI | Youtube (pf. Sung kim) | x | 100% |
| 양재 AI School Summer Camp | AI | Yangjae | o | 이수 |
| CS231N | CV | Youtube / Stanford Univ (pf. Fei-Fei Li) | o | 20% |
| Computer Vision | CV | ... | o | ? |
| Django Tutorial | Web Server | Django Official Hompage | o | 20% |
| 생활코딩 Database | Web Server | Opentutorials.org | x | 30% |
| RL Zero to All | RL | Youtube (pf. Sung kim) | x | 40% |
| 강화학습 기초 | RL | Youtube (pf. David Silver, with 팡요랩) | x | 10% |

**다양한 CV 모델들과 RL 이론을 이해하기 위하여, 대학에서 수학공부와 알고리즘 공부를 하고 있습니다.**


<br>

### Computer Vision Study Source


- **image detection, segmentation 모델에 대한 개념은 아래 내용을 참고했다.**
- Article, step1 : A Step-by-Step Introduction to the Basic Object Detection Algorithms (Part 1)
- Article, step1 : A Practical Implementation of the Faster R-CNN Algorithm for Object Detection (Part 2 – with Python codes)
- Article, step2 : Computer Vision Tutorial: Implementing Mask R-CNN for Image Segmentation (with Python Code)
- Blog, step1 : From R-CNN to Mask R-CNN
- Youtube : Faster R-CNN, Towards Real-Time Object Detection with Region Proposal Networks

> 1. https://www.analyticsvidhya.com/blog/2019/04/introduction-image-segmentation-techniques-python/
> 2. https://www.analyticsvidhya.com/blog/2018/10/a-step-by-step-introduction-to-the-basic-object-detection-algorithms-part-1/?utm_source=blog&utm_medium=image-segmentation-article
> 3. https://www.analyticsvidhya.com/blog/2019/07/computer-vision-implementing-mask-r-cnn-image-segmentation/?utm_source=blog&utm_medium=introduction-image-segmentation-techniques-python
> 4. https://tensorflow.blog/2017/06/05/from-r-cnn-to-mask-r-cnn/
> 5. https://youtu.be/kcPAGIgBGRs

- finetuning 은 다음을 참고했다.
> 1. Github, 'inception v3 finetuning repo' : https://github.com/solaris33/deep-learning-tensorflow-book-code/tree/master/Ch13-Fine-Tuning/Inceptionv3_retraining
> 2. Blog, 'inception v3, finetuning 설명' : https://post.naver.com/viewer/postView.nhn?volumeNo=17107116&memberNo=1085064
> 3. Github, 'Training Fast R-CNN on Right Whale Recognition Dataset' : https://github.com/coldmanck/fast-rcnn/blob/master/README.md
> 4. Article, 'yolo v3, Train Custom Data' : https://github.com/ultralytics/yolov3/wiki/Train-Custom-Data
> 5. Github, 'What does "normalized xywh" mean?' : https://github.com/ultralytics/yolov3/issues/341
> 6. Blog, '적은 데이터셋으로 강력한 모델 학습시키기' : https://keraskorea.github.io/posts/2018-10-24-little_data_powerful_model/

- 모델은 다음을 가져왔다.
> 1. Github, 'yolo v3 with tensorflow (darkflow)' : https://github.com/thtrieu/darkflow
> 2. Github, blog, 'original yolo v3 for window (darknet)' : https://darkpgmr.tistory.com/170 && https://github.com/AlexeyAB/darknet/
> 3. AlexNet

- 위에서 나오는 기본적인 tensorflow 내용은 다음을 참고했다.
> 1. Blog, 'operator' : https://tensorflowkorea.gitbooks.io/tensorflow-kr/content/g3doc/get_started/basic_usage.html

- 위에서 나오는 기본적인 Python 문법들은 다음을 참고했다.
> 1. Official, 'parser' : https://docs.python.org/ko/3/howto/argparse.html#id1


- 기타
> 1. 김태영 블로그 : https://tykimos.github.io/2017/03/25/Fit_Talk/
> 2. 김태영 블로그, 컨볼루션 신경망 모델 만들어보기 : https://tykimos.github.io/2017/03/08/CNN_Getting_Started/

- 나중에 참고할 만한 자료
> 1. Youtube, 'CS231N official spring 2017' : https://youtu.be/vT1JzLTH4G4
> 2. Blog, 'CS231N 강의노트 한글번역 project' : http://aikorea.org/cs231n/


<br>
<br>
<br>

## Process and Issues...

- 정말 깜깜이로 주섬주섬

<br>

- 2019/8/13
> - 일단 darkflow 의 demo 를 돌려 봄. 작동은 한다는 것을 확인함. 하지만 성능이 굉장히 좋지 않음. 좀 불안함.
> - 메이플스토리 소스 파일들 다 꺼내서 하나의 폴더로 이름 정리해서 모으고 폴더 정리했음.
> - 2007 파스칼 데이터셋 형식에 맞춰서 tiny yolo 에 들어갈 xml 파일을 위한 annotating 프로그램 제작 중.
> - tiny yolo 가 내가 학습시킨 데이터셋에서 작동하지 않을 시, normal yolo v3 을 시도해야 하는데, darkflow 에서는 실행이 안돼서 darknet for window source 를 찾음.

<br>

- 2019/8/14
> - xml 파일 완성해서 학습 버튼 누르는것. 데이터셋에 들어있는 몬스터가 존재하는 맵에 가서 스크린샷 detect 돌려보고 안되면 tiny yolo 버리고 다른 것으로 갈아타기 위해 새로운 annotating 프로그램 만들어 학습 버튼 누르는 것이 (xml 또는 json 형식이겠지..) 오늘의 목표.
> - 속도 느리면 피시방 가거나 최유경교수님 연구실 찾아가거나.. 학교 컴퓨터에 몰래 돌려놓고 나와야지
> - 하지만 목표 달성 실패, parsing 하고 이런저런 오류 잡다가 끝남.

<br>

- 2019/8/15
> - 일단 오류 하나 잡음. 소스를 까보니, pascal voc 형식도 조금씩 다 성격이 달랐다는 걸 알게 되었음.
> - 그리고 다음 오류가 발생. 소스를 까보니, pascal voc 형식을 모두 읽어들였지만, training 이 안됨
> - 구글링 결과 https://github.com/thtrieu/darkflow/issues/265 에서 지적한 것처럼, filename 이라는 노드에 확장자가 같이 들어가 있는 경우 나와 같은 오류가 발생한다는 것을 발견함. 하지만 나는 해당 사항이 아니었음. 그리고 https://github.com/thtrieu/darkflow/commit/80f5798d7dcce94969577b585cd26aa0f0c74602 를 찾음.
> - 하지만 나의 이슈는 그것이 아니었고, 오히려 소스 내 path 의 형식에 확장자를 잘 붙여 주어야 한다는 것을 알게 됨. linux 기반의 프로그램을 윈도우에서 돌리다 보니 발생한 문져였던 것 같음. python 에서 os.join() 함수는 예를 들어 "\" 를 사용하는데 윈도우에서는 "/" 를 사용하는.. 이런 문제와, linux 에서는 .확장자 를 명명하지 않아도 되는데 windows 터미널에서는 .확장자 를 명명해야 한다는.

<br>

- 2019/8/16
> - 하루종일 학습 돌렸는데 안 끝남. 동아리 2박 3일

<br>

- 2019/8/19
> - CNN 학습에 대한 조교님의 조언을 들음. "CNN 은 특징을 추출하는데, 배경이 모두 같은 색상이면, 배경 색을 기준으로 CNN이 학습해 버린다. 그러다가 어떠한 형체가 나타나면 그 물체로 인식하는 것.. 이러한 방식으로 학습되어 버린다." 따라서, RoI 에 대한 시도가 필수적임을 알게 됨.
> - Local 에 돌리지 않고 Colab 에 돌리는 것을 추천해 주심. 몬스터의 형상에, 지형을 합성해 보는 것은 어떠냐고 제안해 주셨지만, 내가 가지고 있는 데이터를 생각해 볼 때 그렇지 못함.
> - 그러므로, 이제 다른 방법으로 데이터를 수집하기 위해 **RoI Box 를 그리기** 에 다시 시간을 엄청 투자해야 할 듯. 
> - 단순한 RoI box 그리기, 영상 저장하기 (opencv에서 제공하는 함수를 통한) 구현
> - 몬스터가 뒤를 돌 때 RoI box 가 쫓아가지 못한다는 문제 확인. 다양한 알고리즘으로 테스팅해볼 필요성을 확인
> - multi-roi box 그리기 코드 확인
> - 2차점검 PPT 준비

<br>

- 2019/8/20
> - multi-roi box 를 다양한 알고리즘으로 그려보고 성능 평가해서 가장 괜찮은 것 채택
> - multi-roi box 를 그리고, 그 좌표들을 모두 따서 xml 파일로 만드는 코드 구현0

<br>

- 2019/8/22
> - 이력서 준비

<br>

- 2019/8/23
> - 깃허브 저장소 증발.. 새로운 저장소 만들기
> - python BBox 정보를 저장할 클래스 생성 - 참고 : https://codeday.me/ko/qa/20190604/710379.html

<br>

- 2019/8/24
> - 클래스에 데이터 삽입, 파일로 추출하는 함수 완성
> - 로컬에서 돌려보고 결과 잘 나오면, colab 알아볼것
> - colab 에서 구현하기 성공시킬 것. - 참고 : https://blog.nerdfactory.ai/2019/04/25/learn-bert-with-colab.html
> - 이력서 작성
> - 100 에폭 돌려놓고 취침

<br>

- 2019/8/25
> - image detection 모델 : 50에폭정도로 한번 test 해봤는데 전혀 검출을 못해냄. 정확도 0%
> - 정방형이 아니고 가로로 길쭉한 영상은 제대로 object detection 을 못 할수도 있겠구나 하고 생각하여, input shape 을 바꿔보기로 함
> - input shape 에 대한 연구
> - zfnet 이나 alexnet 
> - 데이터셋 다시 모으기 (정방형으로), 그리고 move 데이터셋 class 4개로 늘릴것. (커닝시티, 엘리니아, 헤네시스, 페리온)
> - image classification 모델 : zfnet 채택 https://github.com/amir-saniyan/ZFNet 안되면 alexnet 개량해서 사용하기로 결정
> - classification 모델 성공시키는 것이 목표
> - classification 모델 조사해보니 죄다 CiFar-10 데이터셋을 사용하는데, 그 데이터셋을 바꾸는 방법을 도저히 모르겠음.
> - 구글링 결과 비슷한 아픔을 느낀 사람 발견 https://github.com/hohoins/ml/tree/master/ImageBinaryGenerator

<br>

- 2019/8/27
> - https://tykimos.github.io/2017/03/08/CNN_Getting_Started/

<br>

- 2019/8/28
> - 나와 완전히 똑같은 문제를 겪는 사람들. 작은 learning rate 을 이용해서 overfit 을 시도해 보아도 아예 수렴을 잘 안하기도 하고, 훈련 후에 bbox 를 아예 그려내지를 못함. https://github.com/thtrieu/darkflow/issues/80


For example, I had changed the line annotations = glob.glob(str(annotations)+'*.xml') to annotations = glob.glob('*.xml') in the darkflow\utils\pascal_voc_clean_xml.py file. But I might have been mistaken about the format of the annotations directory and so might have passed the argument in incorrectly.


@GioLomia When you first start training on custom data, you should overfit the network on a subset of images (3 - 5). keep training until you get perfect bounding boxes on those images with 0.9 confidence or greater. After that, begin training on the entire dataset.
I had a similar issue and this fixed it for me.

<br>

- 2019/9/18
> - 결과물 만들어내는 데 급급해서 겨우겨우 완성을 시키고 발표를 마친 지 거의 한 달, 개강해서 대학공부 하느라 반쯤 손을 놓고 있었는데, 최종 발표 준비하고 소스코드 제출하라고 메일 받고 부랴부랴 모듈화 손보는 중.

<br>

- 2019/9/26
> - 코드 작성된 곳까지 readme 작성 완료


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
