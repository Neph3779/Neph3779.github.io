---
layout: post
title: "Twitter Login API 사용방법"
categories: "Ruby_on_Rails"
tags:
  - Ruby on Rails
  - Tip
---



홈페이지 등에서의 `sign in Twitter` 와 같은 기능을 사용하기 위해선 트위터 홈페이지에서 나의 앱을 등록하여 API Key를 발급받을 필요가 있습니다.

![](/assets/images/posts/Ruby on Rails/Tips/Twitter Login API/E40C3C65-4E28-4A6D-ADE9-2C70F3080C77.png)
위 그림에서 상단의 `Apps`로 이동
![](/assets/images/posts/Ruby on Rails/Tips/Twitter Login API/9136D3AF-F930-4945-9E94-3BCD01F1C3AE.png)
현 상태에서는 아무것도 없음. `Create an app` 클릭하여 새로운 앱을 만들 것.
![](/assets/images/posts/Ruby on Rails/Tips/Twitter Login API/8DAE983B-BAFF-4984-8115-BB472FEF9A81.png)
일단 개발자 아이디로 등록을 하라네.
신청을 적당히 해줍니다.
일단 신청하는 이유를 200자 이상 영어로 서술해야합니다.

- - - -
다 신청하고 이메일들어가서 수락까지하면 왠만하면 자동으로 승인이 날껍니다.
다시 앱을 등록하러 가보자면,
![](/assets/images/posts/Ruby on Rails/Tips/Twitter Login API/F3B862AC-6024-4007-90D6-A76B91768D69.png)
자신의 앱이름과 설명을 적절하게 써주시고,
![](/assets/images/posts/Ruby on Rails/Tips/Twitter Login API/82D79A5E-B50D-4D50-A5F0-3D9A0171056D.png)
여기가 중요한데요, 자신의 Web Application의 주소를 적은다음,
로그인 처리 후 이동할 주소, `Callback`주소를 서술합니다.
저 같은 경우에는 홈페이지 주소 뒤에  `/auth/twitter/callback`을 기술하였습니다.

이 이후에 `heroku restart`를 통해 heroku 서버를 재부팅 후 다시 한 번 로그인 해보니 제대로 로그인이 됩니다.


