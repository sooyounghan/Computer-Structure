-----
### 문자와 문자열
-----
1. 컴퓨터는 원래 숫자를 빨리 처리하기 위해 발명된 것이지만, 상용화되자마자 텍스트 처리에 이용되기 시작
2. 오늘날 대부분의 컴퓨터는 8비트 바이트로 문자를 표현하며, 거의 모든 컴퓨터가 ASCII(American Standard Code for Information Interchange)를 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/44d20585-52e0-42a4-bf04-93566675bd5b">
</div>

3. ASCII와 이진수
   - 10억은 1,000,000,000이므로 10개의 ASCII 자릿수가 필요
   - 각각이 8비트 길이이므로 메모리는 (10 X 8) / 32, 즉, 2.5배가 더 많이 필요
   - 메모리가 더 필요할 뿐만 아니라 십진수를 더하고, 빼고, 곱하고, 나누는 하드웨어는 더 만들기 어려움

4. 명령어 몇 개를 이용하면, 워드 내 특정 바이트를 추출 가능
   - 그러므로 워드 적재와 워드 저장은 워드 뿐 아니라 바이트 전송에도 충분히 사용 가능
   - 하지만, 텍스트 데이터를 다루는 프로그램이 많으므로 효율성을 위해 MIPS에서는 바이트 전송 명령어가 따로 존재
   - lb(load byte)는 메모리에서 한 바이트를 읽어서 레지스터의 오른쪽 8비트에 채우는 명령
   - sb(store byte)는 레지스터의 오른쪽 8비트를 메모리에 보내는 명령
   - 그러므로, 한 바이트를 복사할 때는 다음과 같은 명령어 사용 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/0e709cad-98a8-4485-bcaa-859abc90b409">
</div>

5. 문자 데이터는 보통 여러 개가 모여서 문자열(String)을 이루는데, 문자열의 길이는 가변적임
   - 가변 길이의 문자열을 표현하는 방법은 세 가지 존재
     + 문자열의 맨 앞에 길이를 표시하는 방법
     + 같이 사용되는 변수에 그 길이를 표시하는 방법(구조체와 같이)
     + 마지막에 문자열의 끝을 표시하는 특수 문자를 두는 방법
   - C언어에서는 값이 0인 바이트(ASCII의 null) 하나를 두는 세 번째 방법 사용
   - 그러므로 문자열 "Cal"은 67, 97, 108, 0의 네 바이트로 표현
   - 자바는 첫 번째 방법 사용

6. 예) 문자열 복사 프로시저의 번역
   - 프로시저 strcpy는 문자열 y를 문자열 x에 복사하는 것
   - 문자열 표현은 C언어 관례에 따라 null 바이트로 끝나는 방식 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/bf25bc21-8a32-4269-a531-38a5119df6ba">
</div>

   - MILS 어셈블리 코드
   - 기본적인 MIPS 어셈블리 코드 : 배열 x와 y의 시작 주소는 $a0와 $a1에 있고, i는 $s0에 있다고 가정
     + strcpy는 먼저 스택 포인터를 조정하고 $s0를 스택에 넣음
<div align="center">
<img src="https://github.com/user-attachments/assets/f17ee59d-7ab3-494c-8ce5-ed1e580728dc">
</div>

   - 다음은 i를 0으로 초기화하기 위해 $s0에 0을 넣음 : 이 일은 0에 0을 더해서 $s0에 넣는 명령어로 처리
<div align="center">
<img src="https://github.com/user-attachments/assets/f2d3a2ab-8d3c-46eb-8871-dfa9eb6221cc">
</div>

  - 순환문 : 먼저 i에 y[]를 더해서 y[i]의 주소를 계산
<div align="center">
<img src="https://github.com/user-attachments/assets/ac311772-d461-4a7c-b78b-3e57f6f27de8">
</div>

   - y는 워드 배열이 아닌 바이트 배열이므로 i에 4를 곱할 필요가 없음
   - lb는 바이트를 부호확장하여 레지스터에 넣는 명령어고, lbu는 바이트를 0 확장해서 레지스터에 넣는 명령어
   - 그러므로 y[i]에 있는 글자를 $t2에 넣을 때는 lbu 명령 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/903d376c-8395-4a2a-8ada-93da7cdf8b73">
</div>

   - 같은 방식으로 주소를 계산하여 x[i]의 주소를 $t3에 넣은 후, 이 주소를 이용하여 $t2에 있는 글자를 x[i]에 넣음
<div align="center">
<img src="https://github.com/user-attachments/assets/e4b1335b-14d4-4191-a522-46b06472dfd1">
</div>

   - 복사한 문자가 0이면, 즉, 문자열의 마지막 바이트이면 순환에서 빠져나옴
<div align="center">
<img src="https://github.com/user-attachments/assets/5b3d5ba3-192f-4e20-a8fc-4be503743044">
</div>

   - 0이 아니면 i를 증가시키고 다시 반복
<div align="center">
<img src="https://github.com/user-attachments/assets/3332bb5c-fe9b-4b35-af17-0234a09623fc">
</div>

   - 순환의 처음으로 가지 않았다면, 문자열의 마지막 바이트를 만난 것이므로, $s0와 스택 포인터를 원상 복구하고 되돌아감
<div align="center">
<img src="https://github.com/user-attachments/assets/15f00b58-df41-439e-afe8-ee9eb9d47f50">
</div>

   - C에서 i에 대한 연산을 피하기 위해 보통은 배열 대신 포인터 사용

7. 프로시저 strcpy는 말단 프로시저이므로 컴파일러가 i를 임시 레지스터에 할당했으면 이를 저장하고 복구하는 일을 피할 수 있음
   - 그러므로 $t 레지스터들을 단순히 임시 레지스터로만 생각하지 말고, 사용에 불편하지만 않으면 피호출 프로그램이 사용해야 하는 레지스터 생각하는 것이 좋음
   - 실제로 컴파일러가 말단 프로시저를 만나면 우선 이런 레지스터들을 쓰고, 부족하면 그 때 저장해야 하는 레지스터 사용
