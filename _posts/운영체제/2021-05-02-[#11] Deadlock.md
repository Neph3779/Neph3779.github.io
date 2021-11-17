---
layout: post
title: "[#11] Deadlock(데드락)"
image:
categories: 운영체제
tags: 
  - 운영체제
  - OS
sitemap:
  changefreq: daily
  priority: 1.0
---

## 교착상태(deadlock)

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502181946.png" alt="2" style="zoom:50%;" />



어느 누구도 양보를 하지 않으면 더이상 진행되지 않는 상태를 deadlock(교착상태)라고 한다.

Resource는 하드웨어 자원과 소프트웨어 자원을 모두 칭하는 개념으로 자신의 자원을 내놓지 않고 상대방의 자원을 서로 차지하려는 시도에 의해 deadlock 상태가 발생할 수 있다.

프로세스가 자원을 사용하는 절차는 다음과 같이 이루어진다.

1. 요청 (Request)
2. 획득 (Allocate)
3. 사용 (Use)
4. 반납 (Release)



## Deadlock 발생의 4가지 조건

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502182005.png" alt="3" style="zoom:50%;" />

데드락은 다음 4가지 조건에 의해 발생한다.

- Mutual Exclusion (상호 배제)

  어떤 자원을 가지고 있는 동안에 그 자원을 독점적으로 쓴다.

- No preemption (비선점)

  자원을 강제로 빼앗을 수 없다.

- Hold and wait (보유 대기)

  자원을 가지고 있는 프로세스가 다른 자원을 기다릴 때 자신의 자원을 내놓지 않는다.

- Circular wait (순환 대기)

  자원을 기다리는 프로세스간의 cycle이 형성된다.



## 자원 할당 그래프

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502182013.png" alt="4" style="zoom:50%;" />

Process에서 나가는 화살표는 Resource를 요청하고 있는 상태를 나타낸다.

Resource에서 나가는 화살표는 해당 Resource를 Process가 보유하고 있음을 나타낸다.

Resource는 하나의 Block에 여러개가 있을 수 있다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502182019.png" alt="5" style="zoom:50%;" />



자원 할당 그래프(Resource Allocation Graph)를 통해 deadlock 상태인지 아닌지를 판가름할 수 있다.

그래프에 cycle이 없다면 모든 경우에 deadlock이 아니다.

그래프에 cycle이 있고 모든 Resouce Block에 자원이 하나씩만 있다면 반드시 deadlock 상태이다.

그래프에 cycle이 있지만 모든 Resoucr Block의 자원이 하나씩만 있는 것이 아니라면 (여러개가 있는 Block이 존재한다면) deadlock일 수도 있고 아닐수도 있다. (추가적 검증 필요)



참고)

ppt의 왼쪽 그림은 deadlock 상태이다.

ppt의 오른쪽 그림은 cycle이 형성되어있지만 deadlock 상태가 아니다.



## Deadlock의 처리 방법

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502182024.png" alt="6" style="zoom:50%;" />

Deadlock을 해결하는 방법에는 크게 4가지가 있다. 가장 강한 처리 방법부터 약한 처리방법을 순차적으로 나열하였다.

1. Deadlock Prevention

   Deadlock의 4가지 조건 중 만족하지 않는 조건이 있도록 만드는 방법

   

2. Deadlock Avoidance

   Deadlock 발생 조건을 따져본 뒤 발생 가능성이 없는 경우에만 자원을 할당

   

3. Deadlock Detection and recovery 

   Deadlock이 발생하면 해당 지점을 파악하여 recover하는 방식

   

4. Deadlock Ignorance 

   OS는 deadlock에 대해 관여하지 않음. deadlock은 빈번히 발생하는 현상이 아니기 때문에 deadlock 처리 방법의 overhead를 처리하는 cost가 더 높다 판단하여 현대의 OS들은 대부분 Deadlock Ignorance를 채택하고 있다.



## Deadlock Prevention

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502182036.png" alt="7" style="zoom:50%;" />

위의 4가지 조건 중 하나를 차단하면 deadlock 발생을 막을 수 있다.

Mutual Exclusion은 사실상 직접 차단하는 것이 어렵다. 

Hold and Wait은 두 가지 방법으로 차단할 수 있는데 방법 1의 경우에는 자원의 비효율성이 높아지므로 잘 사용하지 않는다.

반면 방법2는 보유 자원을 모두 내려놓은 상태에서만 자원 요청을 할 수 있게 만드는 방법이므로 자원 비효율성이 높지 않다.

No Preemption의 경우 자원을 뺏었을 때 문제가 없는 구조여야만 차단할 수 있다. CPU 스케쥴링과 다르게 자원을 빼았긴 후 다시 되찾았을 때 이전 작업을 이어서 하기가 어렵기 때문이다.

Circular Wait는 자원마다 번호를 매겨서 정해진 순서대로만 자원을 획득할 수 있도록 처리하면 쌍방향으로의 자원 요구가 이루어질 수 없으므로 Cycle이 생길 우려가 사라진다.



하지만 위의 방법들은 자원 이용률 저하, starvation 현상 등이 발생한다.



## Deadlock Avoidance

 <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502183735.png" alt="image-20210502183734195" style="zoom:50%;" />

Process가 시작되어 종료될 때까지 써야하는 최대의 자원량을 알고 있다고 가정한다.

모든 경우의 수를 따져보고 자원 할당으로부터 안전한 경우에만 자원을 할당해준다. (자원의 여분이 있음에도)



 <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502184158.png" alt="image-20210502184152926" style="zoom:50%;" />



자원당 하나의 instance를 가지고 있다면 자원 할당 그래프 알고리즘(deadlock 발생 여부를 찾아줌)을 이용해 막아줄 수 있고

자원당 여러개의 instance를 가지고 있는 경우에는 Banker's 알고리즘을 통해 deadlock을 막아줄 수 있다.



## Resource Allocation Graph algorithm

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502190017.png" alt="image-20210502190015537" style="zoom:50%;" />

자원당 instance가 하나만 있는 상황에서 deadlock을 피하는 방법



자원에서 프로세스로 뻗어있는 화살표는 프로세스가 해당 자원을 보유하고 있다는 의미

프로세스에서 자원으로 뻗어있는 화살표는 프로세스가 해당 자원을 요청했다는 의미

점선으로 이루어진 화살표는 프로세스가 평생에 거쳐 해당 자원을 요청할 가능성이 있음을 나타냄



deadlock의 위험성이 있으면 자원을 주지 않고 막는다.



## Banker's Algorithm

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502191103.png" alt="image-20210502191101848" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502191140.png" alt="image-20210502191138729" style="zoom:50%;" />



자원당 instance가 여러개 있는 경우 Banker's Algorithm을 적용할 수 있다.

ppt의 예시는 Process가 5개 있고

Resource A,B,C는 instance를 각각 10개, 5개, 7개 가지고 있다. 



Allocation은 현재 할당된 instance를 나타내는 것이고

Max는 최대로 요청할 수 있는 (프로세스 평생에 걸쳐) instance의 수를 나타낸 것이다.

현재 남아있는 자원은 Available section에 보이는 것처럼 A 3개, B 3개, C 2개인 상황에서

프로세스들이 자원을 요청했을 때 처리하는 방식에 대해 알아보자



현재 가용자원만 가지고도(Available) 어떤 프로세스의 Max를 충족할 수 있다면 자원을 할당해준다

하지만 이것이 불가능한 경우에는 deadlock을 피하기 위해 자원을 할당해주지 않는다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502193527.png" alt="image-20210502193525439" style="zoom:50%;" />



프로세스의 Need (현재 상태에서 최대로 요청할 수 있는 resource instance의 양)보다 Available한 자원이 더 많은 경우에만 자원을 할당해주면 deadlock을 막을 수 있다. 

이 때 요청한 자원의 양과 Need는 서로 다른 개념임을 기억하자.

적어도 Need 이상의 양을 요청하지 않는다는 전제 때문에 가능한 상황이다. 



## Deadlock Detection

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502194954.png" alt="image-20210502194952896" style="zoom:50%;" />

Deadlock Detection은 Deadlock Avoidance와 마찬가지로 모든 Resource의 instance가 하나인 경우에는 자원 할당 그래프를 사용해 deadlock을 탐지할 수 있다.

instance가 여러개인 경우에는 Banker's algorithm과 유사한 방법으로 처리할 수 있다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502195232.png" alt="image-20210502195223354" style="zoom:50%;" />

프로세스에서 프로세스로 뻗어나간 화살표는 자원 할당 그래프를 더 간편하게 그리기 위한 방안으로 화살표의 목적지에 해당하는 프로세스가 가진 자원을 화살표의 시작지에 해당하는 프로세스가 요청하고 있음을 나타낸다.



process가 n개일 때 wait for graph에서 cycle을 찾는 overhead는 O(n^2)에 해당한다.

node가 n개일 때 edge의 maximum은 n(n-1)개이므로 이 edge들을 모두 다 따라가보면 O(n^2)의 시간복잡도가 걸림을 알 수 있다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502200422.png" alt="image-20210502200419205" style="zoom:50%;" />

위의 상황은 자원당 instance가 여러개인 경우를 나타낸 것이다.



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502200728.png" alt="image-20210502200726350" style="zoom:50%;" />



## Deadlock Recovery

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/master/img/20210502200920.png" alt="image-20210502200918555" style="zoom:50%;" />

Deadlock을 recover하는 방법에는 크게 두가지가 있다.

1. Deadlock에 연루된 process들을 없애기 (한꺼번에 kill)
2. Deadlock에 연루된 process들을 하나씩 없애보기 (Deadlock이 없어질때까지)

위와 같은 방식으로 safe state가 되면 다시 process를 시작하면 된다.



동일한 프로세스가 계속해서 victim(process가 deadlock에 연루되어서 죽는 경우)이 되는 경우

starvation 문제를 야기할 수 있으므로 이 부분도 같이 고려를 해야한다.