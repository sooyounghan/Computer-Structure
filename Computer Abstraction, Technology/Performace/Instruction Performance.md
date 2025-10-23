-----
### 명령어 성능
-----
1. 프로그램 수행에 필요한 명령어 개수에 관한 사항이 포함되어 있지 않음
   - 그러나 컴파일러가 실행할 명령어를 생성하고 컴퓨터는 이 명령어를 실행해야 하므로, 실행 시간은 명령어 수와 관련있음
   - 이런 관점에서 실행 시간을 실행 명령어 수에 명령어의 평균 실행시간으로 곱한 값으로 계산 가능
   - 그러므로 프로그램 실행에 필요한 클럭 사이클 수는 다음과 같음
<div align="center">
<img src="https://github.com/user-attachments/assets/40a89fa8-309c-4c7e-a88f-57ab0cdb5876">
</div>

2. 명령어당 클럭 사이클 수(Clock cycles Per Instruction)는 CPI로 줄여쓰기도 함
   - CPI : 프로그램 전체 혹은 일부에서 명령어 하나 실행에 필요한 평균 클럭 사이클 수
   - 명령어마다 실행 시간이 다르므로 CPI는 프로그램이 실행한 모든 명령어에 대한 평균한 값을 사용
   - 명령어 집합 구조가 같으면 프로그램에 필요한 명령어 수는 같으므로, CPI는 서로 다른 구현을 비교하는 한 가지 구현이 될 수 있음

3. 성능식의 이용
   - 같은 명령어 집합 구조를 구현한 컴퓨터 두 종류가 존재
     + 컴퓨터 A의 클럭 사이클은 250 ps, 어떤 프로그램에 대한 CPI는 2.0
     + 컴퓨터 B의 클럭 사이클 시간은 500ps, CPI는 1.2
    - 이 프로그램에 관해서 어떤 컴퓨터가 더 빠른가?
      + 두 컴퓨터가 실행해야 하는 명령어의 개수는 서로 같을 것인데, 이를 I라고 가정
      + 먼저, 각 컴퓨터에서 소요되는 프로세서 클럭 사이클 수 계산
<div align="center">
<img src="https://github.com/user-attachments/assets/65f258c8-cea2-4943-9c19-ab0b4ea061f6">
</div>

   - 각 컴퓨터의 CPU 시간
<div align="center">
<img src="https://github.com/user-attachments/assets/bcbd83b2-d21e-4626-a8ad-107b2d3f3377">
</div>

   - A가 빠른데, 얼마나 빠른지 확인
<div align="center">
<img src="https://github.com/user-attachments/assets/25c3bf1a-8e68-4d1e-9392-2590122ffe73">
</div>
