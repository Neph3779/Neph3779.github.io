---
layout: post
title: "[#16] 파일시스템-2"
image:
categories: 운영체제
tags: 
  - 운영체제
  - OS
  sitemap:
    changefreq: daily
    priority: 1.0

---


## 페이지 캐시, 버퍼 캐시

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210606195907.png" alt="image-20210606195905344" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210606195939.png" alt="image-20210606195936242" style="zoom:50%;" />

### 페이지 캐시

스왑 영역에 있는가? 아닌가? 를 기준으로 캐싱 됨/안됨을 구분하는 방식

페이지 캐시의 단위: 페이지의 크기 (4Kb)

### 버퍼 캐시

파일 데이터가 파일 저장소에 저장되어있는가? 아니면 운영체제의 버퍼 캐시에 올라와 있는가? 를 기준으로 캐싱 됨/안됨을 구분하는 방식

버퍼 캐시의 단위: 논리적인 블록 (섹터)의 크기 (512b, disk I/O 단위)

### 유니파이드 버퍼 캐시

페이지 캐시와 버퍼 캐시의 구분 없이 똑같이 페이지 단위로 관리하면서

필요에 따라 페이지 캐시의 용도, 버퍼 캐시의 용도로 바꿔가며 사용하는 방식

### Memory-Mapped I/O

프로세스의 주소 공간 중 일부를 파일에 mapping 해두어서 파일 입출력을 메모리 접근 연산을 통해 실행할 수 있는 방식



## Unified Buffer Cache 전과 후

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210606200600.png" alt="image-20210606200557293" style="zoom:50%;" />

### 기존 방식

버퍼 캐시에 요청한 파일이 있으면 복사를 통해 전달, 없으면 read/write 시스템 콜을 이용해 디스크 파일 시스템으로부터 읽어와서 사용자 프로그램에 전달

memory mapped 

자신의 주소공간 중 일부를 파일에 mapping해놓음 

디스크에 있는 파일을 버퍼 캐시에 읽어오고 그 내용을 페이지 캐시(메모리 영역)에 복사함

이 이후에는 운영체제 간섭 없이 메모리 접근만을 통해 읽고 쓰기가 가능해짐



### 유니파이드 버퍼캐시

read write 시스템 콜을 하는 것은 기존과 같음

요청한게 이미 메모리에 있으면 그냥 전달해주고 없으면 읽어와서 전달함

주소 영역 중 일부를 파일에다 매핑하는 것 이후로는 사용자 프로그램의 주소 영역에 페이지 캐시가 매핑이 됨

(페이지 캐시 자체가 사용자 프로세스의 주소 영역에 있음)



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210606201758.png" alt="image-20210606201755182" style="zoom:50%;" />

코드 부분은 read only이기 때문에 메모리에 올라갔다가 쫓겨날때는 swap 영역에 가지 않고 파일 시스템에 파일 형태로만 존재함 

code영역에 접근하다 page fault 발생시 swap area로 내려가는게 아니라 file system에 가서 가져와야 함



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210606202650.png" alt="image-20210606202647047" style="zoom:50%;" />

위와 같이 데이터를 자신의 주소공간 중 일부에 mapping하여 운영체제의 도움없이 바로바로 사용할 수도 있다.

여기서 memory mapped I/O를 사용하면 메모리 상에서 읽고 쓰는 것도 가능해진다.



## Disk Structure & Management

### logical block

- 디스크의 외부에서 보는 디스크의 단위 정보 공간
- 주소를 가진 1차원 배열처럼 취급
- 정보를 전송하는 최소 단위



### Sector

- Logical block이 물리적 디스크에 매핑된 위치
- Sector 0은 최외곽 실린더의 첫 트랙에 있는 첫 번째 sector (부팅 정보가 들어있음)



### Disk Management

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210606203117.png" alt="image-20210606203114765" style="zoom:50%;" />

**physical formatting** : 디스크를 섹터 단위로 나누는 과정

**header, trailer** : error correction code, 주소 매핑을 위한 sector번호 등의 metadata의 역할을 하는 것들이 저장되는 곳

**partitioning** : 물리적 디스크를 논리적 디스크 여러개로 나누는 작업

**logical formatting** : 파일 시스템을 만드는 과정

**booting** : 메모리 영역 중 전원이 나가도 내용이 유지되는 ROM에 loader라는 부팅을 위한 내용이 저장해놓음, 이후 boot block을 이용해 boot

## Disk Scheduling

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210606203651.png" alt="image-20210606203638344" style="zoom:50%;" />

### 접근시간의 구성

- 탐색시간 (이것을 최소화 하는 것이 목적. 회전지연시간, 전송시간은 작을뿐더러 운영체제가 개입하기 어려움)
- 회전지연시간
- 전송시간 

**Disk bandwidth** : 단위 시간 당 전송되는 데이터의 양



### 알고리즘의 종류

- FCFS

  말그대로 First come first served

- SSTF

  Shortest seek time first로 가장 빨리 갈 수 있는 위치부터 차례대로 탐색

- SCAN

  한쪽 끝까지 탐색 후 다른쪽 끝까지 탐색하는 방법

- C-SCAN

  한쪽 끝까지 탐색 후 아무 탐색 없이 다시 원점으로 돌아온 뒤 다시 탐색을 시작하는 방법 (탐색시간의 편차를 줄여줌)

- N-SCAN

  기본적으로는 scan 방식, arm이 움직이기 시작하면 그 이후에 도착한 일들은 나중에 처리하는 방식

- LOOK

  한쪽으로 쭉 가다가 더 가도 탐색할게 없으면 유턴해오는 방식

- C-LOOK

  LOOK 방식인데 C-SCAN 처럼 마지막 스캔 위치까지 갔다가 다시 원점으로 와서 다시 반복하는 방식



###  디스크 스케쥴링 알고리즘의 결정

현대에는 스캔 기반의 알고리즘을 많이 쓰고 있음

file을 어떻게 할당하느냐에 따라 디스크 스케쥴링 성능에 영향을 끼침

디스크 스케쥴링 알고리즘은 필요할 경우 변경하기 편하도록 OS와 별도의 모듈로 작성되는 것이 좋음



## Swap Space Management

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210606204602.png" alt="image-20210606204600001" style="zoom:50%;" />

### 디스크를 사용하는 이유

- 메모리의 휘발성
- 메모리 공간의 부족

### 스왑 영역

스왑 영역은 공간 효율성보다 시간 효율성이 중요한데 

큰 단위로 데이터를 올리고 내리고 반복해줌



### RAID

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210606205107.png" alt="image-20210606205104641" style="zoom:50%;" />

### 

디스크 여러개를 묶어서 같이 사용하는 방식

여러개의 디스크에 데이터를 얼마나 중복저장, 분산저장할 것인가를 결정해야함

**장점 **

- 디스크 처리 속도가 향상됨
- 중복 저장으로 인해 신뢰성이 향상됨(failure 발생해도 다른 디스크에서 읽어올 수 있음)