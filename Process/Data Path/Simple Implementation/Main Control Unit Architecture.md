-----
### 주 제어 유닛 설계
-----
1. 명령어 필드들을 데이터패스에 연결하는 방법을 이해하기 위해서는 세 가지 명령어 종류(R 형식 명령어, 분기 명령어, 적재 / 저장 명령어) 형식을 알아야함
<div align="center">
<img src="https://github.com/user-attachments/assets/44718511-0ec3-4ef7-9ece-e7035c3a6544">
</div>

  - opcode라 불리는 op 필드는 항상 31:26에 존재 (이 필드를 Op[5:0]라고 칭함)
  - 읽을 레지스터 2개는 항상 rs, rt 필드에 의해 지정 : rs, rt 필드는 비트 25:21과 비트 20:16에 존재 (R형식 명령어와 같을 시 분기 및 저장 명령어에 적용)
  - 적재 명령어와 저장 명령어를 위한 베이스 레지스터는 항상 비트 25:21(rs)에 존재
  - 같을 시 분기, 적재, 저장 명령어를 위한 16비트 변위는 항상 비트 15:0에 존재
  - 목적지 레지스터는 둘 중 하나
    + 적재 명령어에서는 20:16(rt)에 있고, R형식 명령어에는 비트 15:11(rd)에 존재
    + 따라서, 쓰기가 행해질 레지스터 번호로 명령어의 어느 필드를 사용할지 선택하기 위해 멀티플렉서를 추가해야 함

2. 위 정보를 이용해 단순한 데이터패스 명령어 레이블과 또 다른 멀티플렉서(레지스터 파일의 Write register 번호 입력을 위해)를 추가
   - ALU 제어 블록, 상태 소자용 쓰기 신호, 데이터 메모리용 읽기 신호, 멀티플렉서용 제어 신호
<div align="center">
<img src="https://github.com/user-attachments/assets/9c79d7a8-0768-4bc3-bbd5-4e1ad2c216a9">
</div>

   - 모든 멀티플렉서는 입력이 2개이므로 멀티플렉서 제어선은 1비트면 됨
   - 1비트 제어선 7개와 2비트 ALUOp 제어 신호 하나가 존재
     + ALUOp 제어 신호의 동작은 이미 정의하였으므로, 명령어를 실행할 때 이 제어 신호들의 값을 어떻게 설정할 지 결정하기 전에 이 제어 신호들의 동작을 먼저 정의해야 함
     + 7개 제어선의 기능
<div align="center">
<img src="https://github.com/user-attachments/assets/23406e28-d778-4f9e-9b46-e2a894df22ff">
</div>

3. 제어 유닛은 제어 신호 중 PCSrc 하나를 제외한 나머지 모두를 명령어 opcode 필드만 보고 결정할 수 있음
   - 실행 중인 명령어가 branch on equal이며(제어 유닛이 알 수 있음), 동시에 ALU의 Zero 출력이 참일 경우에만 PCSrc가 인가 되어야 함
   - PCSRc 신호를 만들려면 제어 유닛에서 나오는 Branch 신호와 ALU의 Zero 신호를 AND 해야함
   - 이들 9개 제어 신호들은(위 7개와 ALUOp 두 비트) 제어 유닛의 6개 입력 신호(opcode 비트 31:26)에 따라서 결정
   - 제어 유닛과 제어 신호가 나와 있는 데이터 패스
<div align="center">
<img src="https://github.com/user-attachments/assets/552c9973-c805-4180-92e9-1be004daae0a">
</div>

4. 제어 유닛에 대한 논리식이나 진리표를 작성하기 전 제어 기능을 간략하게 정의
   - 제어 신호 값은 opcode에 의해서만 결정되므로, 각 opcode 값에 대해 각 제어 신호가 0, 1, don't care(X) 중 어느 값이 되어야 하는지 정의
   - 제어 신호들의 각각의 opcode에 대해 어떤 값이 되어야 하는지 나타내는 그림
<div align="center">
<img src="https://github.com/user-attachments/assets/ca493cc5-a781-4587-92ac-36d96c6e3f60">
</div>
