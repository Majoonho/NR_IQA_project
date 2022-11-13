![header](https://capsule-render.vercel.app/api?type=waving&color=auto&height=200&text=Welcome!%20&fontSize=60&fontAlignY=40&desc=I'm%20joonho)


##### 팀원 : 노치현, 마준호, 임건희 <br/>프로젝트 수행기간 : 2달<br/>Time Flow : 1주(기획) -> 1주(Data eda, Image crop) -> 2주(FR_IQA구현) -> 4주(NR_IQA 구현) -> 1주(발표 PPT 준비)<br/>my part : 기획참여, Image crop 구현, NR_IQA Metric 구현, ppt 제작   <br/><br/><br/>


### NR_IQA (No Reference Image Quality Assessment)
  * Task 목적 : 기업현장에서 이미지 합성(생성)을 할 경우 발생하는 distortion 에 대해<br/> 이미지 퀄리티평가를 자동으로 하는 것입니다.
  * 기업에서 원하는 사항 : 이미지 합성(생성)시 원본사진이 없으므로 이미지퀄리티 평가가 불가능<br/> --> NR Metric으로 해결하길 원함
  * 문제점 : MOS 지표를 가지고 있는 얼굴 데이터셋을 찾기 힘들며,<br/> 단일 Metric 으로 좋은 결과를 내기 어려움
  
<img src="https://user-images.githubusercontent.com/103080228/201522891-07de2fba-fe95-4a6d-b346-de6a190af8af.JPG"  width="500" height="300">   
<!--<p align="center"><img src="https://user-images.githubusercontent.com/103080228/201522891-07de2fba-fe95-4a6d-b346-de6a190af8af.JPG"  width="500" height="300"></p>-->

### 기획
 * FR-IQA 를 MOS 에 regression 모델을 통해 학습을 하고,  <br/> 그 결과값을 NR-IQA score(input) 와 new_FR_score(target) 으로 학습을 합니다.
 
<p align="left"><img src="https://user-images.githubusercontent.com/103080228/201523838-6b34be14-b833-4b50-a6ef-8dba442dfdab.JPG"  width="370" height="300"><img src="https://user-images.githubusercontent.com/103080228/201523842-bd70920e-0ded-4584-8a11-903710db14e3.JPG"  width="370" height="300"></p>

<초기 Flow-Chart>
<br/><br/>

 * 하지만 학습 데이터의 분포와 테스트 데이터의 분포의 차이로 결과가 안좋게나와<br/>이 부분을 고민하고, 다시 간단하게 수정이 되는 과정을 거칩니다.
<img src="https://user-images.githubusercontent.com/103080228/201523828-b078f137-5f45-435b-b12e-da2c6f17fda4.JPG"  width="500" height="300">
<최종 Flow-Chart>


