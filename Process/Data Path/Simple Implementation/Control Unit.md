-----
### 제어 유닛의 완성
-----
1. 제어 유닛의 출력은 제어선들이며, 입력은 6비트의 opcode 필드(Op[5:0])
   - 따라서, opcode의 이진수 인코딩을 이용해 각 출력의 진리표를 만들 수 있음

2. 제어 유닛의 논리에 대한 진리표
<div align="center">
<img src="https://github.com/user-attachments/assets/2430f28b-9aec-4d6b-96b9-10ec3b859df2">
</div>

   - 이 표는 모든 출력을 망라하고 있으며, opcode 비트들을 입력으로 사용
   - 제어 기능을 완벽하게 명시하며, 자동화된 방법을 이용해 곧바로 게이트로 구현 가능

3. 단일 사이클 구현(Single-Cycle Implementation) : 단일 클럭 사이클 구현
   - 명령어 하나를 한 클럭 사이클에 실행하는 구현
   - 따라서, MIPS 핵심 명령어 집합에 대한 단일 사이클 구현을 거의 완성
