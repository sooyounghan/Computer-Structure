-----
### 파이프라인 제어
-----
1. 파이프라인 데이터패스에 제어를 추가하는 설계 과정
   - 첫 번째 단계 : 기존 데이터패스의 제어선에 레이블을 붙이는 것
<div align="center">
<img src="https://github.com/user-attachments/assets/cdbe2aef-ffa8-478b-ae23-fd830d3d1e43">
</div>

   - 단일 사이클 구현에서와 같이 매 클럭 사이클마다 PC에 쓰기가 행해지며, 따라서 PC를 위한 쓰기 신호는 따로 없다고 가정
   - 같은 논리로 파이프라인 레지스터들(IF / ID, ID / EX, EX / MEM, MEM / WB)을 위한 쓰기 신호도 따로 없음
     + 파이프라인 레지스터 역시 매 클럭 사이클마다 쓰기가 행해지기 때문임

   - 파이프라인을 위한 제어를 명시하기 위해서는 각 파이프라인 단계 동안의 제어 값들을 정하기만 하면 됨
   - 제어선은 각 파이프라인 단계에서 활성화되는 구성 요소들과 연관되어 있으므로, 제어선을 파이프라인 단계에 따라 다섯 그룹으로 나눌 수 있음
     + 명령어 인출 : 명령어 메모리를 읽는 제어 신호와 PC에 쓰는 제어 신호는 항상 인가되어 있으므로 이 파이프라인 단게에는 특별히 제어할 것이 없음
     + 명령어 해독 / 레지스터 파일 읽기 : 이전 단계에서와 마찬가지로 매 클럭 사이클마다 같은 일이 일어나므로 설정할 제어선이 없음
     + 실행 / 주소 계산 : 이 단계에서 설정할 신호들은 RegDst, ALUOp, ALUSrc이며, 이 신호들은 목적지 레지스터와 ALU 연산을 선택하고, Read Data 2와 부호 확장된 수치값 중 하나를 ALU 입력으로 선택
     + 메모리 접근 : 이 단계에서 설정되는 제어 신호는 Branch, MemRead, MemWrite이며, 이 신호들은 beq, lw, sw 명령어일 때 인가됨, Branch = 1이고 ALU 결과 = 0 일 때만 PCSrc = 1이 되어 분기 목적지 주소를 선택함
     + 쓰기 : 이 단계에서 설정할 제어신호는 MemtoReg와 RegWrite이며, MemtoReg는 레지스터 파일에 ALU 결과를 보낼 것인지, 메모리 값을 보낼 것인지를 결정하며, RegWrite는 선택된 값을 레지스터에 쓰게 하는 신호
<div align="center">
<img src="https://github.com/user-attachments/assets/f10924b8-3c6d-4119-bacc-3c8ad5a1b0d5">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/96c01f53-8828-4bb9-939d-a2d137691e41">
</div>

   - 데이터패스를 파이프라이닝한다고 제어선의 의미가 달라지지 않으므로, 전과 같은 제어값을 사용할 수 있음
   - 제어선 9개가 파이프라인 단계에 따라 그룹화되는 것이 다름
<div align="center">
<img src="https://github.com/user-attachments/assets/b9100191-8bb3-46b8-be79-364292713fb0">
</div>

2. 제어를 구현하는 것은 각 단계에서 9개 제어 신호의 값을 그 명령어에 해당하는 값으로 설정하는 것을 의미
   - 이렇게 하는 가장 간단한 방법은 제어 정보를 포함하도록 파이프라인 레지스터를 확장하는 것

3. 제어선들이 EX 단계부터 나타나기 시작하므로 제어 정보를 ID 단계에서 생성해도 늦지 않음
   - 명령어가 파이프라인을 흘러 내려가면서 제어 신호들이 해당 파이프라인 단계에서 사용되는 것을 보여줌
<div align="center">
<img src="https://github.com/user-attachments/assets/68971eea-254f-45a3-b6c4-b6e649677038">
</div>

   - 마치 적재 명령어의 목적지 레지스터 번호가 파이프라인을 흘러 내려가는 것과 같음
   - 확장된 파이프라인 레지스터와 해당 단계에 연결된 제어선을 가지고 있는 전체 데이터패스를 보여줌
<div align="center">
<img src="https://github.com/user-attachments/assets/2a6bc67b-b4b5-4800-a236-438a94290c3b">
</div>
