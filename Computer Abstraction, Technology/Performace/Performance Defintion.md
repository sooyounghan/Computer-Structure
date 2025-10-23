-----
### 성능의 정의
-----
1. 응답시간(Response Time), 실행시간(Execution Time) : 작업 개시에서 종료까지의 시간
   - 컴퓨터가 테스크를 완료하기까지의 총 소요시간으로 디스크 접근 / 메모리 접근 / 입출력 작업 / 운영체제 오버헤드 및 CPU 시간 모두 포함

2. 처리량(Throughput) 또는 대역폭(Bandwidth) : 단위 시간 당 완료하는 테스크의 수(처리하는 작업의 양)를 나타내는 또 다른 성능 척도

3. 컴퓨터의 성능을 논할 때는 주로 응답 시간에 초점을 맞출 것
   - 성능의 최대화하기 위해서는 어떤 태스크의 응답 시간 또는 실행 시간을 최소화해야 함
   - 따라서, 어떤 컴퓨터 X의 성능과 실행 시간의 관계는 다음과 같음
<div align="center">
<img src="https://github.com/user-attachments/assets/7a80ca0c-468a-4286-b7a9-1beafac788fa">
</div>

   - 그러므로 두 컴퓨터 X와 Y에 대해 X의 성능이 Y의 성능보다 좋을 경우
<div align="center">
<img src="https://github.com/user-attachments/assets/c5afadb2-2b63-4924-80e5-4e6e8ed5148d">
</div>

   - 즉, X가 Y보다 빠르면, Y에서 실행시간은 X에서의 실행시간보다 김
   - 컴퓨터 설계에 관해 설명할 때는 두 컴퓨터의 성능을 정량적으로 비교해야 하는 경우 발생 : 정량적으로 표현하면 "X가 Y보다 n배 빠르다"라고 표현
<div align="center">
<img src="https://github.com/user-attachments/assets/f90727ae-3b16-4b1a-ada4-ae746bf065a1">
</div>

   - X가 Y보다 n배 빠르다면, Y에서의 실행시간이 n배 길 것
<div align="center">
<img src="https://github.com/user-attachments/assets/fa1e5f7b-62e8-405e-84fe-b735750e4add">
</div>
