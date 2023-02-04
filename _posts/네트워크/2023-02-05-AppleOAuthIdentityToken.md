---
layout: post
title: "(서버 임시 전달용) 애플 identity token 발급 및 확인법"
image:
categories: [네트워크]
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

1. 터미널에서 git clone -b feature/#120 --single-branch https://github.com/YAPP-Github/21st-ALL-Rounder-Team-1-iOS.git 명령어 통해 120번 브랜치만 클론

2. 파인더에서 RefillStation.xcodeproj를 더블클릭해서 프로젝트 오픈

3. 시뮬레이터 기기를 iPhone 기종 중 하나로 변경
   <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230205003006.png" alt="image-20230205003006783" style="zoom: 25%;" />

4. settings 버튼 눌러서 세팅으로 이동후 로그인 진행 (iCloud 병합 여부를 묻는 경우 병합 안 함 선택해도 무관)

   | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230205003038.png" alt="simulator_screenshot_9A7CB082-2DAB-4C46-8734-975544266354" style="zoom:25%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230205003202.png" alt="simulator_screenshot_5696CE43-2497-43B3-931B-00F0E7D41FD9" style="zoom:25%;" /> |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |

5. 암호 및 보안에 들어가서 "이중 인증"이 켜짐 상태인지 확인
   만약 이중 인증이 켜져있지 않은 경우 애플 아이디 이중 인증 설정을 해주어야 함

   | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230205003404.png" alt="Simulator Screen Shot - iPhone 14 Plus - 2023-02-05 at 00.33.20" style="zoom:25%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230205003359.png" alt="Simulator Screen Shot - iPhone 14 Plus - 2023-02-05 at 00.33.37" style="zoom:25%;" /> |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |

6. cmd + q로 시뮬레이터 종료 후, 다시 xcode에서 cmd + r로 코드 실행

7. 시뮬레이터상에서 애플로 로그인 진행

8. 콘솔창의 identityToken 값 확인

   <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20230205003713.png" alt="image-20230205003713089" style="zoom: 33%;" />

