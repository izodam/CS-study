# 프로세스 & 스레드

## 1. 프로세스와 스레드의 개념

### 프로세스와 스레드

[] (https://asset.hankooktire.com/content/dam/hankooktire/common/media/KR/ko/2023/03/news_img_2023_0329.jpg)

* 프로세스: 하나의 프로그램 인스턴스(독립)
	* 프로그램 그 자체는 그냥 코드의 덩어리일 뿐
	* 이를 동적으로 실행하는 것이 바로 프로세스
	* OS가 CPU, 메모리, 네트워크 등의 자원 할당

[] (https://filestore.community.support.microsoft.com/api/images/dbf3517c-b8cd-4daf-b05d-1b43c28ad506?upload=true)

* 스레드: 프로세스가 자원을 사용하는 실행의 단위
	* 메모리는(Stack, Code, Heap, Data)를 받음
	* Stack을 제외한 메모리는 스레드간 공유
	* 프로세스는 독립적이기에 다른 프로세스 메모리 간섭 X

### 메모리 공간에 대한 추가 설명

* Code: 프로그램 내부의 코드를 CPU가 이해가능하게 번역한 기계어가 담긴 부분
* Data: 코드 내부의 전역 변수와 기타 데이터들이 모임
	* .data: 전역, static 변수 등의 데이터가 저장
	* .BSS: 초기값을 주지 않는 전역, static 변수 저장
	* .rodata: 상수 키워드(const)가 저장
* Stack: 지역 변수가 호출될 때의 로컬 변수가 할당됨. 함수가 종료시 소멸
* Heap: 생성자, 인스턴스 등의 동적 데이터가 할당됨. 사용자에 의해 동적으로 할당.

* Code, Data는 크기가 변하지 않는다. 하지만 Stack과 Heap은 함수, 혹은 사용자에 의해 변한다.

### 프로세스는 진짜 서로 간섭 불가?

* 결론적으로 서로 영향을 주는 법은 있음
	* IPC, LPC, 공유 메모리 사용 등

* 그러면 왜 안쓸까?
	* 캐시 메모리가 초기화 되고, 레지스터도 교체한다.
	* 그에 따라 자원 부담이 커짐.
	* 그럴 바에 한 프로세스에 다수의 스레드 할당이 효율적


## 2. 프로세스 상태와 PCB

[] (https://encrypted-tbn3.gstatic.com/shopping?q=tbn:ANd9GcR9HtCKnvTQodd9OQVzTw8eOyBO5jgPcqBLBxROe9T_LVvL3fDAmBv--AAeyMMHABUjKjz_JP8XTpyB6eMMDpK84ONkagqakEyE1fW6LtlATSnCVprxPhw7&usqp=CAE)
> 187만원짜리 최신 인텔 14900K

> 기본적으로 요즘의 최신 CPU도 24코어에 32스레드 정도다.

> 그런데 작업관리자를 보면 수많은 프로세스가 나열되어 있다.

> 어떻게 이게 다 작동하고 있을까?

### 프로세스 상태

* 프로세스는 상시 작동하는 것이 아님

* 기본적으로 프로세스는 5가지의 상태가 있다
	1. 생성: 프로세스에 자원을 할당, PCB도 이때 생성
	2. 준비: CPU할당과 문맥 교환을 기다린다
	3. 실행: CPU를 점유하고 각종 동작을 수행
	4. 대기: 입출력이나 이벤트를 기다리며 실행을 잠시 멈춤
	5. 종료: 실행 완료, 메모리 해제, PCB 제거

### 프로세스 제어 블록(Process Control Block)

* Process ID : 프로세스를 구분하는 ID
* Process state : 각 State 들의 상태를 저장한다.
* Program Counter : 다음 Instruction의 주소 저장. 
* Register : Accumulator, CPU Register, General Register 등이 포함.
* CPU Scheduling Information : 우선순위, 최종 실행시간, CPU 점유시간 등이 포함된다.
* Memory Information : 해당 프로세스 주소공간(lower bound ~ upper bound) 정보를 저장
* Process Information : 페이지 테이블, 스케줄링 큐 포인터, 소유자, 부모 등
* Device I/O Status : 프로세스에 할당된 입출력 장치 목록, 열린 목록등
* Pointer : 부모/자식 프로세스에 대한 포인터, 자원에 대한 포인터 등
* Open File List : 프로세스를 위해 열려있는 파일의 리스트

### PCB의 역할

* CPU는 수행중인 프로세스를 지속적으로 변경(문맥 교환)

* 이때 프로세스의 상태에 대한 여러 정보가 PCB에 저장

* 마찬가지로 교체되는 프로세스 또한 PCB의 정보를 바탕으로 실행
