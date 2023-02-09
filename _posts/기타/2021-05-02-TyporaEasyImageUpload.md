---
layout: post
title: "Typora 이미지 쉽게 업로드 하기 for mac"
image:
categories: [기타]
tags: 
  - Typora
  - Picgo
sitemap:
  changefreq: daily
  priority: 1.0
---

## 사건의 발단

Typora를 통해 깃허브 블로그 글을 쓰다보면 이미지 첨부하는 작업이 매우 귀찮다고 느끼시는 분들이 많으실 것 같아요. 이미지의 상대경로를 지정해주고 깃헙 블로그 레포의 어떤 폴더에 캡쳐한 스크린샷을 담는 과정은 상당히 귀찮은 작업이죠.. 

이러한 문제를 해결할 수 있는 방안(이미지를 링크로 변환하여 업로드)을 오늘 알려드리겠습니다.

 

#### 내가 찍은 스크린샷이 weibo에 익명으로 올라가는 것을 참을 수 있어!

하시는 분들은 밑의 내용을 읽으실 필요 없이

Typora의 이미지 설정 부분에 가셔서 Image Uploader을 upic으로 설정해주신 뒤

`brew install upic` 커맨드를 통해 upic을 다운받아주시기만 하면 간단하게 해결할 수 있습니다





## 그래서 어떻게 하면 되는데?

### 1. Typora의 언어 중국어로 변경하기

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502223128.png" alt="image-20210502223124947" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502223414.png" alt="image-20210502223410704" style="zoom:50%;" />

우선 Typora의 언어를 중국어로 바꾸는 것부터 시작합니다

???:   🧐 웬 뜬금없이 중국어?

하시는 분들이 많으실텐데 이번 포스트의 핵심이 될 Picgo가 중국어만을 지원하기 때문에 진행해야하는 작업입니다!

과정이 끝난 뒤에는 다시 한국어로 설정해도 전혀 문제가 없으니 걱정하지 마세요! 😆

중국어로 설정해주신 뒤 command + q를 이용해 typora를 완전 종료해줬다가 다시 켜주세요.

<br/> 

### 2. PicGo.app 다운로드 하기

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502223553.png" alt="image-20210502223551901" style="zoom:50%;" />

중국어로 바꿔주셨다면 이미지 업로더의 종류에 `PicGo.app`이 추가된 것을 보실 수 있으실거에요

PicGo.app은 중국어만 지원하는 앱이기 때문에 언어를 중국어로 설정했을 경우에만 노출되도록 Typora가 지정해놨습니다.

이제 PicGo.app 다운로드하기 버튼을 누르셔서 (아래 하 + 어려운 한자 버튼이 다운로드 버튼입니다.) 

다운로드를 진행해주세요 (꼭 저 버튼을 통해 하실 필요는 없고 PicGo 깃헙으로 가셔서 dmg 파일을 받으셔도 됩니다.)

<br/> 

### 3. Github에 이미지가 저장될 Repository 만들기, 토큰 받아오기

이 부분의 스크린샷은 생략하도록 하겠습니다!

Repo를 만들어 주시고 토큰을 받아와주세요

토큰을 받아오는 과정은 [이 블로그 글](https://taeuk-gang.github.io/wiki/Typora%20%EC%8B%A0%EA%B8%B0%EB%8A%A5%20-%20%EC%9D%B4%EB%AF%B8%EC%A7%80%20%EC%9E%90%EB%8F%99%20%EC%97%85%EB%A1%9C%EB%93%9C/)에 잘 설명되어있으니 참고하시면 좋을 것 같습니다!

<br/> 



### 4. PicGo.app 설정하기

이제부터는 제가 보여드리는 스크린샷을 그대로 따라해주시면 됩니다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502223852.png" alt="image-20210502223850730" style="zoom:50%;" />



Custom에 불이 들어오도록 버튼을 눌러주세요!



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210503101050.png" alt="image-20210503101048818" style="zoom:50%;" />

톱니바퀴 모양 cell을 눌러서 이 화면에 들어와 주신 뒤

제 설정대로 stepper을 On/Off 해주세요

다만! 위에서 4번째에 있는 stepper을 On 해주면 이미지를 업로드 할때 매번 업로드될 이름을 정해줄 수 있어요!

<br/> 

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210502225855.png" alt="20210502225608" style="zoom:50%;" />

왼쪽 ScrollView에 있는 GitHub뭐시기 버튼을 눌러 이 화면에 와주신 뒤 TextField를 채워주시면 됩니다!

<br/> 

첫 칸에는 본인의 깃허브 닉네임/레포이름 을 적어주세요!

두번째 칸에는 저장할 레포의 어떤 브랜치에 이미지를 업로드할지 설정해주는 작업인데, 여기서 master branch가 아닌 다른 브랜치로 설정해주셔야 무분별한 commit들이 쌓이지 않습니다! 

사용하기로 한 깃헙 레포에 추가적인 브랜치를 만들어주세요! 저의 경우에는 master branch가 아닌 forUpload라는 이름의 브랜치에 이미지가 업로드 되도록 한 상황입니다! 

세번째 칸에는 아까 받아온 토큰을 넣어주시면 됩니다.

네번째 칸에는 이미지의 경로를 적어주시면 되는데 repo내의 폴더라고 보시면 될 것 같아요

(제가 설정한 것은 Neph3779라는 유저의 Blog-Image라는 레포의 img 폴더에 내가 찍은 스크린샷을 저장해! 가 되겠네요)

이 작업이 끝나셨다면 밑의 파란색 버튼과 초록색 버튼을 눌러주시면 됩니다 (초록 버튼은 눌러지지 않을 수 있는데 문제 없어요!)

<br/> 

그럼 이제 드디어 타이포라에 와서 이미지를 업로드 해볼까요?

(타이포라의 언어 설정은 한국어로 다시 바꿔주셔도 됩니다!)

<br/> 

### 5. Typora를 통해 이미지 업로드 하기

찍은 스크린샷을 단순히 command + v로 타이포라에 붙여넣기 하면 아래의 사진처럼 내 맥북에 저장된 경로를 통해 이미지가 업로드 됩니다



![image-20210502224715666](https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502224745.png)



 이 상태에서 사진을 우클릭 하신 뒤 Upload Image라는 버튼을 누르시면



![image-20210502224818354](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210502225842.png)

짜잔 보이는것처럼 나의 깃헙 레포와 연동된 링크를 통해 이미지가 들어가는 것을 볼 수 있어요!



개인적으로 이 작업을 해놓고 나니 사진을 업로드 하는게 너무나도 편해졌어요!

게다가 upic은 weibo에 나의 소중한 스크린샷들이 익명으로 올라가는게 마음에 걸렸었는데

이 방법을 쓰면 내 깃허브 레포에만 저장되고 이미지가 유출되지 않으니 좋은 것 같습니다

Mac 사용자를 위한 튜토리얼이 없길래 글을 써봤는데 도움이 되셨으면 좋겠어요!
