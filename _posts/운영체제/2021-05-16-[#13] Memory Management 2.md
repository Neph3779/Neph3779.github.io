---
layout: post
title: "[#13] 메모리 관리-2"
image:
categories: 운영체제
tags: 
  - 운영체제
  - OS
sitemap:
  changefreq: daily
  priority: 1.0
---

## MultiLevel Paging and Performance

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516123827.png" alt="image-20210516123811067" style="zoom:50%;" />

페이지 테이블은 2단계로만 쓸 수 있는게 아닌 여러 단계의 페이지 테이블을 쓰는게 가능하다.

다단계 페이지 테이블은 페이지 테이블이 사용하는 공간을 더 많이 줄일 수 있지만 변환작업을 여러번 거쳐야 하고, 주소 변환을 위해 메모리에 여러번 접근해야하기 때문에 오버헤드를 커진다.

하지만 TLB라는 주소변환을 전담하는 일종의 cash 메모리 덕분에 대부분의 주소변환은 TLB를 통해 이루어지기 때문에 다단계 페이지 테이블을 사용하더라도 오버헤드가 크지 않다.



## Valid (v) / Invalid (i) Bit in a Page Table

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516124142.png" alt="image-20210516124138507" style="zoom:50%;" />

논리주소의 페이지 개수만큼 페이지 테이블의 엔트리가 존재하고 페이지가 물리적 메모리의 어떤 페이지 frame에 올라가 있는지 주소변환 정보가 들어있다고 배웠었다.

하지만 페이지 테이블에는 이 외에도 부가적인 비트가 엔트리마다 저장되고 있는데 이 중 하나가 valid-invalid bit이다.

메모리에 의미있는 값을 집어넣든 않든간에 6,7 등의 0은 의미있는 값으로 해석될 것이다. 프로그램이 가질 수 있는 maximum size 만큼 페이지 테이블의 엔트리가 생겨야하는데 (페이지 테이블의 특성상 위에서부터 순서대로 적어야하기 때문에 필요) 여기서  사용하지 않는 영역을 invalid로 표시한다. (32bit 운영체제라면 최대 1Mb짜리 페이지 테이블이 필요하다. )



## Memory Protection

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516124543.png" alt="image-20210516124540476" style="zoom:50%;" />

### Protection Bit

여기서는 접근해서 안될 페이지를 막아주는 protection의 의미가 아니다.

페이지 테이블을 통해 이동한 것이므로 자기 자신의 페이지에만 접근할 수 있으므로 애초에 다른 프로그램의 공간에는 접근할 수 없다.

여기서의 protection의 의미는 어떤 연산에 대한 접근 권한이다. 프로세스의 영역에는 code만 담고 있는 부분도 있을 것이고 (read only로 있어야 하는 부분) data 영역이나 stack 영역 같은 곳은 read&write 권리를 모두 가지고 있어야 하므로 이를 지정해주는 bit가 필요하다.

### Valid-Invalid Bit

프로세스가 아예 주소를 사용하지 않거나 해당 페이지가 backing store에 가있는 경우 invalid로 표시된다.



## Inverted Page Table

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516125524.png" alt="image-20210516125520349" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516125629.png" alt="image-20210516125626602" style="zoom:50%;" />



Page table은 너무 많은 메모리 공간을 차지한다는 문제가 있다. 프로세스마다 각각 주소변환이 이루어져야 하기 때문에 공간 오버헤드가 큰데 이를 해결하기 위해 존재하는 것이 inverted page table이다. 

원래 페이지 테이블을 통한 주소의 변환을 거꾸로 뒤집어 놓은 inverted page table은 기존의 프로세스마다 필요했던 page table이 아닌 시스템 안에 page table이 딱 하나만 존재하며 page table의 엔트리가 프로세스의 page 개수만큼이 아닌 물리적인 메모리의 page 개수만큼 존재한다. page table의 첫 엔트리는 physical memory의 첫 엔트리를 나타내고 마지막 엔트리는 physical memory의 마지막 엔트리를 담당하는 식이다. 

기존의 page table은 주소변환을 위해 page 번호를 보고 위에서부터 page 번호만큼 떨어진 엔트리에 가서 주소변환을 하는 방법을 썼었는데 inverted의 경우 그것이 불가능하다. 원래는 논리적인 page 번호가 있으면 해당하는 엔트리에 가서 페이지 테이블에 가면 페이지 frame 번호가 몇번이지 나왔는데 역방향의 경우에는 page frame 개수만큼 엔트리가 존재하며 첫번째 entry에는 첫번째 page frame에 들어가는 논리적 page 번호가 들어있고 두번재 entry에는 두번째 page entry에 들어가는 논리적 번호가 들어있다. page frame의 f번째 entry에 가면 논리적인 page table의 번호가 나오게 되어있는게 inverted page table architecture의 구조이다.

pid : 어떤 프로세스의 page인지를 저장하는 공간 (논리적 주소는 프로그램마다 별도로 존재하므로 f번째에 올라가있는 page가 어떤 프로그램의 p번째 page인지를 나타내야 함)



## Shared Page

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516131502.png" alt="image-20210516131459581" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516131521.png" alt="image-20210516131518876" style="zoom:50%;" />



서로 다른 프로세스가 같은 코드를 가지고 프로그램을 돌린다 하면 프로그램의 코드 부분은 같은 내용이니 shared code를 이용할 수 있다. 이런 경우에는 각각의 코드를 모두 메모리에 올리는 것이 하나만 read only state로 올려서 이를 공유할 수 있다.

shared code는 모든 프로세스에서 동일한 logical address상에 있어야 한다.

shared code는 re-entrant code, pure code라고도 부른다.



:exclamation:shared code는 공유 데이터와는 다른 개념이다. (공유 데이터와 다르게 shared code는 Read-Only이기 때문)



## Segmentation

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516134426.png" alt="image-20210516134423619" style="zoom:50%;" />

segment는 프로그램을 의미가 있는 단위로 자른 것이다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516134506.png" alt="image-20210516134504076" style="zoom:50%;" />

세그먼트별로 주소변환이 이루어져야하기 때문에 segmentation table이 있으며, segment table의 길이가 limit register에 들어가게 된다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516134558.png" alt="image-20210516134555965" style="zoom:50%;" />

s는 세그먼트의 번호, d는 떨어진 위치에 해당하는 offset이다.

paging과 다르게 물리적인 메모리상의 시작위치 외에도 limit이라는 segment의 길이를 가지고 있는데 page와 다르게 segment마다의 길이가 다를 수 있기 때문에 필요한 내용이다. 

- 프로그램이 가지고 있는 segment 개수보다 큰 segment를 요청하면 trap
- segment의 길이보다 segment에서 떨어진 offset값이 크면 trap



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516135020.png" alt="image-20210516135016652" style="zoom:50%;" />

segmentation 기법은 page 기법과 다르게 크기가 균일하지 않기 때문에 생기는 문제점들을 가지고 있음

하지만 의미 단위의 일을 하기 위해서는 매우 효과적 (실행권리 권한부여의 구역을 나누기 쉽기 때문)



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516134940.png" alt="image-20210516134937476" style="zoom:50%;" />



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516135044.png" alt="image-20210516135041217" style="zoom:50%;" />

segment와 page를 비교했을 때 table에 의한 공간 낭비가 큰 쪽은 paging 기법쪽이다.

  

## Segmentation with Paging

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210516202942.png" alt="image-20210516202939549" style="zoom:50%;" />

[정리]

>  물리적인 메모리를 나누는 방법으로 크게 연속 할당과 불연속 할당 기법이 있었다. 이 중 불연속 할당 기법에 이용할 수 있는 방법으로는 paging, segmentation, paged segmentation이 있다. 



Paged Segmentation 기법을 사용하면 segment가 page로 쪼개져서 올라가기 때문에 segmentation의 조각 문제가 발생하지 않는다. 

자세한 작동방식은 글로 풀어쓰기보다는 위의 순서도를 이해해보는게 나을 것 같다.