---
layout: post
title: "[#12] 메모리 관리-1"
image:
categories: 운영체제
tags: 
  - 운영체제
  - OS
sitemap:
  changefreq: daily
  priority: 1.0
---

## Logical Address vs Physical Address

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509140605.png" alt="image-20210509140602320" style="zoom:50%;" />

Logical address는 프로세스마다 독립적으로 가지는 주소공간이다. 어떤 프로세스가 메모리상에서 어느 지점에 위치하는지와 무관하게 논리적으로 각 프로세스가 0번지부터 가지고 있는 주소값을 Logical address(virtual address)라고 부른다.

반면 Physical address는 메모리에 실제로 올라가있는 위치를 나타낸다.

어떤 프로그램이 물리적인 메모리의 어느 지점으로 올라갈지에 대해 결정하는 것을 주소 바인딩이라 한다. 주소 바인딩의 시점은 크게 3가지로 분류할 수 있다.

❗️Symbolic Address란 우리가 흔히 사용하는 포인터라 볼 수 있다. 프로그래밍을 할 때 논리적인 주소값을 실제로 사용하는 경우는 거의 없고 Symbolic한 공간에 담아서 사용하는데 이런 것을 Symbolic Address라 한다. 컴파일 시에 이런 Symbolic Address는 Logical Address로 바뀌게 된다.



## 주소 바인딩의 3가지 시점

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509141342.png" alt="image-20210509141341673" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509141431.png" alt="image-20210509141428970" style="zoom:50%;" />

주소 바인딩이 이루어지는 시점은 크게 다음 3가지로 분류할 수 있다.

1. Compile time binding

   컴파일 타임에 Physical address가 결정됨 (사실상 Logical address가 Physical address와 같음)

   컴파일 타임에 메모리 어느위치에 올라가야 할지가 결정되어야 하므로 비효율적

2. Load time binding

   프로그램이 메모리에 올라갈 때 Logical address를 Physical address에 올려준다.

3. Execution time binding (= Run time binding)

   Load time binding과 기본적으로 같지만 Physical address가 실행되는 도중에 바뀔 수 있다. (메모리에서 쫓겨났다가 다시 메모리에 올라갈 수 있으므로)

   하드웨어적 지원이 필요한 방식이며 MMU라는 하드웨어가 이를 담당한다.



❗️ instruction code내부의 Logical Address는 메모리에 올라가더라도 Logical Address의 상태를 유지하고 있으므로 CPU가 보는 주소는 Logical Address라는 것에 유의



## Memory Management Unit

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509173608.png" alt="image-20210509173605022" style="zoom:50%;" />



주소변환을 위한 하드웨어 장치인 MMU는 base register(relocation register)의 값을 통해 user program과 CPU가 Logical address만을 가지고 일할 수 있도록 도와준다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509173742.png" alt="image-20210509173739212" style="zoom:50%;" />



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509173920.png" alt="image-20210509173916868" style="zoom:50%;" />

가장 간단한 형태의 MMU는 Relocation register와 Limit register 이 두개의 레지스터를 통해 주소바인딩을 진행한다. 

그림의 예제에서는 Logical Address 346번지를 보려하고 있다. 이런 상황에서 프로세스 p1의 시작위치(relocation register) + 346에 위치한 메모리에 올라가있는 값을 읽어 반환한다. 

Limit register는 프로그램의 크기를 담는 값으로 범위를 벗어난 주소로의 잘못된 접근을 막아준다. (다른 프로그램이 존재하는 위치의 값을 참조할 수 없음)





## 몇가지 용어 정리

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509174433.png" alt="image-20210509174430642" style="zoom:50%;" />



### Dynamic Loading (Loading: 메모리로 올리는 것)

- 프로세스 전체를 메모리에 미리 올리는 것이 아닌 해당 루틴이 불려질 때 메모리에 load하는 것
- 메모리 이용률 향상
- 가끔식 사용되는 코드들이 많은 경우에 유용 (오류 처리 루틴은 가끔씩만 사용됨)
- 운영체제의 특별한 지원 없이 프로그램 자체에서 구현 가능 
  - OS가 라이브러리를 통해 Dynamic Loading을 쉽게 할 수 있도록 지원)



### Dynamic Linking

- Linking을 실행 시간(execution time)까지 미루는 기법
- Static linking
  - 라이브러리가 프로그램의 실행 파일 코드에 포함됨
  - 실행 파일의 크기가 커짐
  - 동일한 라이브러리를 각각의 프로세스가 메모리에 올리므로 메모리 낭비 
- Dynamic linking
  - 라이브러리가 실행시에 연결(link)됨
  - 라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 stub이라는 작은 코드를 둠
  - 라이브러리가 이미 메모리에 있으면 그 루틴의 주소로 가고, 없으면  디스크에서 읽어옴
  - 운영체제의 도움이 필요



### Overlays

- 메모리에 프로세스의 부분 중 실제 필요한 정보만을 올림
- 프로세스의 크기가 메모리보다 클 때 유용
- 운영체제의 지원없이 사용자에 의해 구현
- 작은 공간의 메모리를 사용하던 초창기 시스테메서 수작업으로 프로그래머가 구현
  - Manual Overlay
  - 프로그래밍이 매우 복잡함



### Swapping

- Swapping : 프로세스를 일시적으로 메모리에서 backing store로(하드디스크 등) 쫓아내는 것
- Backing store (= swap area)
  - 디스크 : 많은 사용자의 프로세스 이미지를 담
- Swap in / Swap out
  - 일반적으로 중기 스케쥴러 (swapper)에 의해 swap out 시킬 프로세스 선정
  - priority based CPU scheduling algorithm
    - priority가 낮은 프로세스를 swapped out
    - priority가 높은 프로세스를 메모리에 올려놓음
  - Compile time 혹은  load time binding 에서는 원래 메모리 위치로 swap in 해야함
  - Run time binding에서는 추후 빈 메모리 영역 아무 곳에나 올릴 수 있음
  - swap time은 대부분 transfer time (swap되는 양에 비례하는 시간)임



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509180641.png" alt="image-20210509180638441" style="zoom:50%;" />

프로세스를 메모리에서 backing store로 swap out하고

backing store에서 메모리로 다시 swap in 하는 과정



## Allocation of Physical Memory

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509183936.png" alt="image-20210509183932669" style="zoom:50%;" />

물리 메모리는 낮은 주소공간에 OS가, 높은 주소공간에 사용자 프로세스가 올라간다.

사용자 프로세스를 할당하는 방법에는 연속 할당과 불연속 할당이 있다.

불연속 할당의 예시로는 잘 알려진 Paging 기법이 있다.



## 연속 할당

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509184148.png" alt="image-20210509184146679" style="zoom:50%;" />

연속 할당은 고정 분할 방식과 가변분할 방식으로 구분된다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509184248.png" alt="image-20210509184246161" style="zoom:50%;" />

고정 분할 방식은 사용자의 프로그램을 미리 파티션(분할)으로 나누는 것이고

가변 분할 방식은 사용자 프로그램이 들어가 있는 메모리 영역을 미리 나누지 않는 것이다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509184231.png" alt="image-20210509184229095" style="zoom:50%;" />

외부 조각이란 프로그램의 크기보다 분할의 크기가 작은 경우에 발생 (다른 프로그램이 들어가기엔 너무 작은 공간)

내부 조각이란 프로그램이 들어가고도 남아버린 공간



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509190019.png" alt="image-20210509190016473" style="zoom:50%;" />

Hole : 가용 메모리 공간



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509190145.png" alt="image-20210509190141506" style="zoom:50%;" />

연속 할당을 사용하면 Hole이 생기는데 이 Hole의 어느 위치에 프로그램을 넣을 것인지에 대한 문제가 Dynamic Storage Allocation Problem이다.

이 문제를 해결하는 알고리즘은 대표적으로 다음 세가지가 있다.

- First - fit 
- Best - fit
- Worst - fit



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509190404.png" alt="image-20210509190401752" style="zoom:50%;" />

외부조각 문제를 해결하는 방법중엔 compaction 이라는 방법이 있는데 사용중인 메모리 영역을 한군데로 몰아서 사용 가능한 넓은 공간을 얻어내는 방법이다. compaction시 최소한의 메모리 이동 방법을 구하기 매우 어려우며 비용이 많이 든다.



## Paging 기법

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509190553.png" alt="image-20210509190550691" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509191337.png" alt="image-20210509191333969" style="zoom:50%;" />

각각의 page로 나누어 비어있는 공간이 있다면 page를 물리적인 메모리(page frame)에 할당해준다.

logical page frame이 physical memory의 어떤 frame에 올라가 있는지에 대해 다루는 것이 page table이다.

page table의 내용에는 frame의 번호가 담겨있다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509193903.png" alt="image-20210509193900472" style="zoom:50%;" />

위의 그림은 paging 기법을 나타낸 그림이다.

CPU가 논리적인 주소를 주면 물리적인 주소로 바꿔주는 작업을 page table을 거쳐서 진행한다.

p는 page의 번호가 되고 d는 page 내에서 얼만큼 떨어져 있는지를 나타내는 offset이 된다. 

물리적인 주소로 바꾸기 위해 page의 번호를 frame의 번호로 바꾸는 작업이 page table에서 진행된다.

page table의 용량이 매우 크기 때문에 page table은 물리적 메모리에 들어가게 된다.



## TLB & Associative Register

### Page Table의 구현

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509194641.png" alt="image-20210509194638320" style="zoom:50%;" />

메모리에 접근하기 위해서는 주소 변환을 해야하며

page table이 메모리에 존재하기 때문에

page table에 접근하는 과정에서 메모리 접근이 이루어지고

page table을 통해 변환된 주소로 이동하는 과정에서 또 한번 메모리 접근이 이루어진다.



MMU는 base register와 limit register가 존재했는데 Page Table에서는 이것이 Page-table base register(PTBR)와 Page-table length register(PTLR) 두가지로 이용된다. 

PTBR은 page table을 가리키며 (메모리에 존재하므로 page table에 접근하기 위해 필요)

PTLR은 테이블의 크기를 보관한다. (limit의 용도)



이렇듯 두 번의 메모리 접근은 비용이 크기 때문에 속도 향상을 위해 별도의 하드웨어 자원을 두기도 하는데 associative register 혹은 Translation Look-aside Buffer(TLB)라 불리는 캐시를 사용한다.



### TLB

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509195201.png" alt="image-20210509195158476" style="zoom:50%;" />

> 캐시 메모리 : 메인 메모리 윗단에 존재하는 메인 메모리에서 빈번히 사용되는 메모리를 더 빨리 접근할 수 있게 해주는 메모리

TLB는 page table에서 빈번히 이용되는 table 값을 캐싱하고 있음 (메인 메모리보다 접근 속도가 빠름)

CPU가 메모리에 접근할때는 먼저 TLB를 검색해보고 TLB에 없는 경우 (TLB miss)에는 page table를 통해 접근한다.

page 번호와 frame 번호 쌍을 가지고 있으며 TLB 전체를 검색해봐야 있는지 없는지를 확인할 수 있다.



###  Associative Register

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509195711.png" alt="image-20210509195707543" style="zoom:50%;" />

TLB 전체를 항상 검색해봐야한다는 특성 때문에 병렬적 탐색이 가능한 Associative register를 사용한다.

TLB와 page table은 프로세스마다 존재해야하므로 문맥교환이 일어날때마다 flush 작업을 해줘서 비워줘야 한다.



### 실제 메모리 접근 시간

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509200005.png" alt="image-20210509200001687" style="zoom:50%;" />



## Two-Level Page Table

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509200119.png" alt="image-20210509200117200" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509202817.png" alt="image-20210509202814597" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509203420.png" alt="image-20210509203416523" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210509203524.png" alt="image-20210509203521191" style="zoom:50%;" />

일반 Page Table은 공간의 낭비가 크므로 이를 개선하기 위해 있는 것이 Two-Level Paging Table

inner table의 entry 하나당 크기는 4bytes인데 이는 inner table의 크기가 실제 page의 크기와 같기 때문이다.

2^10 = 1K이므로 1K만큼의 크기를 가진 page와 매칭이 가능하다.

