-----
### 명령어의 컴퓨터 내부 표현
-----
1. 명령어도 컴퓨터 내부에서는 높고 낮은 전기 신호의 연속으로 저장되므로 숫자로 표현 가능
   - 실제로 명령어의 각 부분은 숫자로 볼 수 있으며, 이 숫자들은 나란히 늘어놓으면 명령어가 됨

2. 거의 모든 명령어가 레지스터를 사용하므로 레지스터 이름을 숫자로 매핑하는 규칙이 존재
   - MIPS에서는 레지스터 $s0에서 $s7까지는 레지스터 번호 16 ~ 23번까지로, $t0 ~ $t7까지는 번호 8 ~ 15번까지로 매핑
   - 즉, $s0은 레지스터 16번, $s1은 17번을 나타내고, $t0은 레지스터 8번, $t1은 9번을 나타냄

3. 예) MIPS 어셈블리 언어를 기계어로 변환
<div align="center">
<img src="https://github.com/user-attachments/assets/94abbe44-9203-4aa9-9384-7c9548ee1ef6">
</div>

   - 명령어의 각 부분을 필드(Field)라고 부름
   - 처음과 마지막 필드(여기서는 0과 32에 해당 부분)가 컴퓨터에 덧셈을 하라고 지시하는 부분
   - 두 번쨰 필드는 사용할 첫 번째 피연산자 레지스터 번호(17 = $s1), 세 번째 필드는 두 번쨰 피연산자 레지스터 번호(18 = $s2), 네 번째 필드는 계산 결과가 들어갈 레지스터 번호(8 = $t0)
   - 이 명령어에는 다섯 번쨰 필드는 사용되지 않으므로 0으로 표시
   - 이 명령어는 레지스터 $s1을 레지스터 $s2에 더해서 그 합을 $t0에 넣으라는 뜻
   - 이 명령어의 각 필드 값을 이진수로 표현
<div align="center">
<img src="https://github.com/user-attachments/assets/17e6b323-e3ba-4517-aafd-b487c0b68f91">
</div>

   - 참고 : 16진수 - 2진수 변환표
<div align="center">
<img src="https://github.com/user-attachments/assets/2e042800-c9d9-454d-8b91-7bdfaa2936a7">
</div>

4. 위 예제에서 보인 레이아웃을 명령어 형식(Instrunction Format)이라고 함
   - 명령어 형식 : 이진수 필드로 구성된 명령어 표현 형식
   - MIPS 명령어의 길이는 데이트 워드와 마찬가지로 32비트
   - 어셈블리 언어와 구별하기 위해 명령어를 숫자로 표현한 것을 기계어라고 하고, 이런 명령어들의 시퀀스를 기계 코드(Machine Code)라고 함 (기계어 : 컴퓨터 시스템 내 사용하는 명령어의 이진수 표현
   - 모든 컴퓨터의 데이터 길이는 4의 배수이므로 16진수(Hexadecimal)가 많이 사용 : 기수 16은 2의 멱승이므로 이진수 4비트를 16진수 숫자 하나로 쉽게 바꿀 수 있음

5. 이진수와 16진수 간의 변환
<div align="center">
<img src="https://github.com/user-attachments/assets/6ab8b89d-c20b-4c85-aa0c-20c5831794b2">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/aa279834-147d-4f6c-ad8d-7695225eb2b5">
</div>
