## 01 스케쥴링의 개요

### 1. 레스토랑 관리자의 스케쥴링

레스토랑 관리자  == CPU 스케쥴러
 - 요리사가 요리를 하다가 직접 주문을 받거나 대기자를 관리할 수 없기 때문에 그러한 일을 대신해서 한다.
 - 예약, 좌석, 주문, 조리 순서, 손님 요청 관리 등
 - 전체 손님의 식사 진행 상황에 따라 조리 순서를 조절하여 요리사에게 알려준다.
 - 손님의 개별적인 요구에도 대응.

프로세서 스케쥴러라고도 한다. 여러 프로세스의 상황을 고려하여 CPU와 시스템 자원을 어떻게 배정할지 결정함.

### 2. CPU 스케쥴링

레스트랑 관리자는 주문과 관란하여 큰 틀의 관리와 작은 틀의 관리를 함.<br>
큰 틀 : 하루에 손님과 예약을 몇명 받을지 등<br>
작은틀 : 자리를 배정받은 손님을 대상으로 이루어지는 관리. 디테일. 손님들의 다양한 상황을 고려하여 요리가 나가는 순서와 속도를 관리함.

CPU 스케쥴러도 관리의 범주를 나누어 고수준/중간수준/저수준 스케쥴링으로 구분하여 관리한다.

- 작업대기 ~ 보류 프로세스 : 고수준 
- 보류 프로세스 ~ 활성 프로세스 : 중간 수준 - 보류, 활성화가 이루어짐
- 활성 프로세스 ~ 실행 프로세스 : 저수준 - 대기 또는 타임아웃, 프로세스 선정이 이루어짐.

#### 고수준 스케쥴링
- 많은 작업을 동시에 하면 과부하가 걸려 작업이 원활하게 이루어지지 않는다. 
- 시스템 내의 전체 작업 수를 조절하는 것.
- 작업은 OS에서 다루는 일의 가장 큰 단위로, 1개 또는 여러 개의 프로세스로 이루어진다. 
- 작업이 시작되면 시스템 자원을 사용하기 때문에 기존 작업에 영향을 미친다. 
  - 때문에 작업 요청이 오면 스케쥴러가 시스템의 상황을 고려하여 작업을 승인할지, 거부할지를 결정한다. 
  - 승인(admission) 스케쥴러라고도 한다. 
- 시스템 내에서 동시에 실행 가능한 프로세스의 총 개수가 정해진다. 
- 시스템의 부하를 고려하여 작업을 시작할지 말지를 결정하는데
  - 이 결정에 따라 정해지는 시스템 전체의 프로세스 수를 '멀티프로그래밍 정도(degree of multiprogramming)'
- 일괄처리할 때 사용된다.

#### 저수준 스케쥴링
- 가장 작은 단위의 스케쥴링
- 어떤 프로세스에 CPU를 할당할지, 어떤 프로세스를 대기, 준비, 실행 등으로 보내는지를 관리
- 프로세스의 상태는 대부분 저수준 스케쥴링에 해당함.
- 아주 짧은 시간에 일어나기 때문에 단기(short-term) 스케쥴링이라고도 함.
- 실제 작업이 이루어짐.
- CPU 스케쥴러는 대부분 중간 수준 스케쥴러와 저수준 스케쥴러로 구성되어 있음.

#### 중간 수준 스케쥴링
- 고수준 스케쥴링을 프로세스의 활성화 여부를 결정해서 프로세스 수를 조절함.
- 프로세스가 활성화된 후에도 여러가지 사정으로 시스템 과부하가 걸릴 수 있음. 
- 이때 부하 조절에 중간 수준 스케쥴링을 고려해야한다. 
  - 활성화된 프로세스 중 일부를 보류 상태로 보내고, 처리 능력에 여유가 생기면 다시 활성화 된다. 
- 중지(suspend), 활성화(active)로 전체 시스템의 활성화된 프로세스 수를 조절하여 과부하를 막는다.
- 저수준 스케쥴링이 원만하게 이루어지도록 완충하는 buffer 역할을 한다.


### 3. 스케쥴링의 목적
프로세스가 시스템 자원을 독접하거나 파괴하는 것을 막기 위해 중요도에 따라 우선순위를 배정해야 한다.

- 공평성 : 모든 프로세스가 자원을 공평하게 배정받아야 함.
- 효율성 : 유휴 시간 없이 자원이 사용되도록 스케쥴링을 해야함. 
- 안정성 : 우선순위를 사용하여 중요 프로세스가 먼저 작동하도록 배정 및 프로세스로부터 자원보호
- 확장성 : 프로세스가 증가해도 시스템이 안정적으로 작동하도록 조치
- 반응 시간 보장 : 적절한 시간안에 프로세스의 요구에 반응을 해야함. 응답이 없는 경우 시스템이 멈춘것으로 가정
- 무한 연기 방지 : 특정 프로세스의 작업이 무한히 연기되어서는 안됨.

## 02 스케쥴링 시 고려 사항
### 1. 선점형 스케쥴링과 비선점형 스케쥴링
#### 선점형 : 프로세스가 CPU를 할당받아 실행중이더라도 OS가 강제로 빼앗을 수 있음.
- OS가 필요하다고 판단되면 실행상태에 있는 프로세스의 작업을 중단 시킬 수 있음.
- 대표적인 예가 인터럽트임. 
- 인터럽트를 받으면 커널을 깨워서 인터럽트 처리하게 하고, 처리가 완료되면 원래의 작업으로 돌아감.
- 문맥 교환 같은 부가적인 작업으로 인해 낭비가 생긴다는 단점이 있음. 
- 대화형 시스템이나 시분할 시스템에 적합함. 
- 대부분의 저수준 스케쥴러는 선점형 스케쥴링 방식을 사용함. 

#### 비선점형 : 다른 프로세스가 빼앗을 수 없음.
- 그 프로세스가 종료되거나 자발적으로 대기 상태에 들어가기 전까지는 계속 실행된다. 
- 선점형 스케쥴링보다 스케쥴러의 작업량이 적고 문맥 교환에 의한 낭비도 적다. 
- CPU 사용시간이 긴 프로세스 때문에 CPU 사용 시간이 짧은 여러 프로세스가 오랫동안 기다리게 되어 전체 시스템의 처리율이 떨어진다.
- 비선점형 스케쥴링은 과거의 일괄 작업 시스템에서 사용하던 방식이다. 

시스템 백업은 선점형 스케쥴링 방식이지만, 비선점형 프로세스이다. 

### 2. 프로세스 우선순위
프로세스의 중요도에 따라 CPU 스케쥴러는 우선순위를 적용한다.

- 프로세스는 커널프로세스와 일반 프로세스로 나뉘어 진다.
- 커널 프로세스 > 일반 프로세스
- 우선순위가 높다는건 더 빨리 자주 실행된다는 의미.
- 작업이 끝날때까지 계속 CPU를 사용함.
- 일반 프로세스의 우선순위는 사용자가 조절할 수 있다.
  - 유닉스의 nice 명령어를 사용하여 우선순위 조절 가능. 단, 관리자만 높일 수 있고. 낮추는건 일반계정도 가능함.
- 우선순위를 높인다는 것은 다른 프로세스의 실행속도에도 영향을 미치기 때문에 주의해야한다.

### 3. CPU 집중 프로세스와 입출력 집중 프로세스
프로세스는 CPU를 사용하여 작업하는 실행 상태 또는 입출력을 요청하여 완료되기까지 기다리는 대기 상태에 있다.

CPU를 할당받아 실행하는 작업을 CPU burst, 입출력 작업을 I/O burst라 한다.

- CPU 집중 프로세스(cpu bound process) : 수학 연산과 같이 CPU를 많이 사용하는 프로세스
- 입출력 집중 프로세스(I/O bound process) : 저장장치에서 데이터를 복사하는 일과 같이 입력을을 많이 사용하는 프로세스

두 개중에서는 입출력 집중 프로세스를 먼저 실행상태로 옮기는 것이 효율적이다.<br> 
-> 이유는 입출력 집중 프로세스가 실행 상태로 가면 입출력 요구에 의해 대기 상태로 옮겨지고, 이때 다른 프로세스가 CPU를 사용할 수 있기 때문.<br>
-> 입출력 프로세스가 CPU 집중 프로세스보다 실행 상태에 먼저 들어가는 경우가 사이클 훔치기임.<br>
-> cpu가 처리속도가 더 빠르기 때문에 i/o에게 양보한다고 앞에서 나왔었음.

### 4. 전면 프로세스와 후면 프로세스
GUI에서 화면의 맨 앞에 놓은 프로세스가 '전면 프로세스', 뒤에 놓인 프로세스가 '후면 프로세스'

- 전면 프로세스 : 현재 입력과 출력을 사용하는 프로세스
  - 사용자와 상호작용이 가능하며 상호작용 프로세스라고도 한다.
  - 사용자의 요구에 즉각 반응해야한다.
  - 우선순위가 더 높음
- 후면 프로세스 : 사용자와 상호작용이 없는 프로세스
  - 압축 프로그램처럼 입력 없이 작동하기 때문에 일괄 작업 프로세스라고도 한다.
  - 우선순위가 낮아 CPU를 할당받을 확률이 낮다.

### 5. 정리

#### 우선순위 높음
- 커널 프로세스
- 전면 프로세스
- 대화형 프로세스
- 입출력 집중 프로세스

#### 우선순위 낮음
- 일반 프로세스
- 후면 프로세스
- 일괄 처리 프로세스
- CPU 집중 프로세스


