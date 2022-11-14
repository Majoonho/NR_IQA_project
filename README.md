![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=200&text=Welcome!%20&fontSize=60&fontAlignY=40&desc=I'm%20joonho)


##### 팀원 : 노치현, 마준호, 임건희 <br/>프로젝트 수행기간 : 2달<br/>Time Flow : 1주(기획) -> 2주(FR_IQA구현) -> 2주(NR_IQA 구현) -> 1주(Data EDA, Image crop) -> 모델학습 -> 1주(발표 PPT 준비)<br/>my part : 기획참여, Image crop 구현, NR_IQA Metric 구현, ppt 제작   <br/><br/><br/>


# NR_IQA (No Reference Image Quality Assessment)
  * Task 목적 : 기업현장에서 이미지 합성(생성)을 할 경우 발생하는 distortion 에 대해<br/> 이미지 퀄리티평가를 자동으로 하는 것입니다.
  * 기업에서 원하는 사항 : 이미지 합성(생성)시 원본사진이 없으므로 이미지퀄리티 평가가 불가능<br/> --> NR Metric으로 해결하길 원함
  * 성능평가 : ROCC/LCC
  * 문제점 : MOS 지표를 가지고 있는 얼굴 데이터셋을 찾기 힘들며,<br/> 단일 Metric 으로 좋은 결과를 내기 어려움<br/><br/>
  
  * 가설 : FR/NR Metric 을 fusion 하면 앙상블 모델처럼 성능이 향상될 수 있다.
  * 결과 : Metric 간의 단점을 서로 보완해주어, 실제로도 더 좋은 결과를 만들어 내었음 <br/> --> 세밀한 모델을 만든다면, 더 좋은 결과가 예상되어집니다.
  
  
  
  
<img src="https://user-images.githubusercontent.com/103080228/201522891-07de2fba-fe95-4a6d-b346-de6a190af8af.JPG"  width="500" height="300">   
<!--<p align="center"><img src="https://user-images.githubusercontent.com/103080228/201522891-07de2fba-fe95-4a6d-b346-de6a190af8af.JPG"  width="500" height="300"></p>-->
<br/>

### 기획
 * FR-IQA 를 MOS 에 regression 모델을 통해 학습을 하고,  <br/> 그 결과값을 NR-IQA score(input) 와 new_FR_score(target) 으로 학습을 합니다.
 
<p align="left"><img src="https://user-images.githubusercontent.com/103080228/201523838-6b34be14-b833-4b50-a6ef-8dba442dfdab.JPG"  width="370" height="300"><img src="https://user-images.githubusercontent.com/103080228/201523842-bd70920e-0ded-4584-8a11-903710db14e3.JPG"  width="370" height="300"></p>

<초기 Flow-Chart>
<br/><br/>

 * 하지만 학습 데이터의 분포와 테스트 데이터의 분포의 차이로 결과가 안좋게나와<br/>이 부분을 고민하고, 다시 간단하게 수정이 되는 과정을 거칩니다.<br/>(FR-IQA Metric을 제외함)
<img src="https://user-images.githubusercontent.com/103080228/201523828-b078f137-5f45-435b-b12e-da2c6f17fda4.JPG"  width="500" height="300">
<최종 Flow-Chart>
<br/><br/>

### FR, NR-IQA Metric 구현
 * FR-IQA 구현
   * MSE/RMSE, PSNR/PSNR-B, UQI, SSIM/MS-SSIM, ERGAS, SCC, SAM ,VIFP, WaDIQaM/DIQaM 등의<br/> Metric 구현 - 노치현, 임건희
 * NR-IQA 구현  
   * IL-NIQE, CNN, BRISQUE, CPBD, NIQE, WaDIQaM/DIQaM, HYPER - 마준호
   * 구현하면서 어려운 점 
     - 옛날 논문을 기반으로 만들어져서 버전이 대부분 다름
     - 사라진 함수의 경우 최신 doc를 참고해서 다시 만들어야함 <br/> --> 구글링을 통해 해결, 오랜 시간 고민하면서 문제해결에 자신감이 생김 
<br/>   

### 데이터 이상치 탐색

 * NR_Metric correlation/ROCC,LCC
   * kadid dataset 의 MOS 지표와 상관관계를 보면, <br/> 뚜렷한 특징이 있는 것과 아닌 것을 확인할 수 있습니다.
 
 <p align="left"><img src="https://user-images.githubusercontent.com/103080228/201550805-790e4fdc-9d4c-4e2a-81bd-863371fd26a4.JPG"  width="370" height="300"><img src="https://user-images.githubusercontent.com/103080228/201550810-d814112a-ea86-49b3-9ce1-6c12a175a8ca.JPG"  width="370" height="300"></p>
  
* Boxplot 탐색
  * 다른 NR_Metric 에 비해 NIQE 계열만이 이상치가 확인됩니다. <br/> --> 이상치 데이터를 확인하고 Metric 신뢰여부 판단
<p align="left"><img src="https://user-images.githubusercontent.com/103080228/201550811-8c68f232-0f0d-4915-ab54-9f7e6ed4dfc0.JPG"  width="370" height="300"><img src="https://user-images.githubusercontent.com/103080228/201550813-c24c8198-be52-4d2d-bcc9-f0c40eda0425.JPG" width="370" height="300"></p>

 * KADID-10k score Distribution for each NR-IQAs 
 <br/>
 
### How to choose NR_Metric


 * Feature Importance
 * D-test, P-test, L-test
<p align="left"><img src="https://user-images.githubusercontent.com/103080228/201574856-40892445-cce5-4dc1-97ed-50bd1ed517a0.JPG"  width="370" height="300"><img src="https://user-images.githubusercontent.com/103080228/201574863-fc029afd-b554-41c6-866b-2ae47a77ebb6.JPG" width="370" height="300"></p>
  
 * Feature Importace, D,P,L-test, Data EDA를 통해 다음과 같은 Metric 선별.
<img src="https://user-images.githubusercontent.com/103080228/201575299-95020762-f6e2-427f-b38e-a037f6eec1e3.JPG"  width="500" height="300">
<br/>
 

### 학습 결과
 * MSE : train 0.01 val 0.18 <br/> ROCC 0.922 LCC 0.941
 <img src="https://user-images.githubusercontent.com/103080228/201575304-c31d07c2-5612-49f5-aac5-5ebc75427c47.JPG"  width="500" height="300">
 
 * Test dataset 결과
  * Test dataset 에 분류되어 있는 이미지 퀄리티(high, middle, low)에 따라 분포가 되어 있음
  * 다만 Test dataset 의 label 이 과제출제자의 개인 의견이므로 신뢰성은 높지 않음 
  * 처음 과제 distortion(blur만 하기로 함) 과 맞지 않아 더 정확한 결과를 못낸것이 아쉬움
 <img src="https://user-images.githubusercontent.com/103080228/201576447-b1b218fe-b2a2-497e-bcaf-aeab76e5f02d.JPG"  width="500" height="300">
 <br/>
 
 
### Discussion
 * Task 의 목적을 기업에서의 활용에 초점을 준 만큼, 성능, 범용성, 유지관리 및 비용이라는 <br/> 측면에서 더 생각해보려고 노력하였습니다.
 <p align="left"><img src="https://user-images.githubusercontent.com/103080228/201578452-8deec17f-8efa-4f07-8522-fb48d270af18.JPG"  width="370" height="300"><img src="https://user-images.githubusercontent.com/103080228/201578459-5c495558-bdc3-44f5-8796-f81be98096e2.JPG"  width="370" height="300"></p>
 <br/>
 
### To Do
 * 더 많은 NR-IQAs 학습
 * JPEG 압축 등의 다양한 distortion으로 학습
 * LGBM 외 다른 Regression 모델을 사용한 Fusion 실험
 * ML 모델이 아닌 DL 모델 학습에서의 성능평가
 * 더 정밀한 Fusion을 위한 실험체계 구축
 
 -----
 * 이번 project 는 적은 인원으로 시작한만큼 해야할 것도 많았습니다. (다른 팀의 절반)
 * 모델은 최대한 간단하게 돌려보아서 어려움은 없었습니다. 하지만 처음 접하는 FR/NR_Metric 에 대한 이해와<br/> 버전문제, 없어진 함수 등을 해결하는데 매우 많은 시간이 걸렸습니다.
 * mat 학습파일의 경우에는 python으로 불러오면 매우 느리다는 것도 배웠고, <br/>.py 를 .ipynb 로 바꾸는 것도 늘었습니다.
 * 이상치파일을 걸러내는 함수를 만드는 경험도 재미있어서 배울 것이 많았던 project 입니다.
 
 
 
 

