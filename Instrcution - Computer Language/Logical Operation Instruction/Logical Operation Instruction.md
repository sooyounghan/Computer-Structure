-----
### 논리 연산 명령어
-----
1. 워드 내 8비트로 저장된 문자를 검사하는 작업이 필요
   - 또한, 비트들을 워드로 묶는(Packing) 작업과 워드를 비트 단위로 나누는(Unpacking) 작업을 간단하게 명령어들이 프로그래밍 언어와 명령어 집합에 추가됐는데, 이러한 명령어들을 논리 연산 명령
   - C, Java, MIPS에서의 논리 연산
<div align="center">
<img src="https://github.com/user-attachments/assets/ffa7b704-d9de-4db3-88c0-525ff160bdf2">
</div>

2. 자리이동(Shift)
   - 이 명령어는 워드 내 모든 비트를 왼쪽 또는 오른쪽 이동시키고, 이동 후 빈자리는 0으로 채움
   - 예를 들어, $s0 레지스터에 다음과 같은 값이 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/f98097fe-8039-4c86-8eb7-0d14827e6b02">
</div>

   - 왼쪽으로 네 번 자리이동 시키는 명령어를 실행시키면 다음과 같음
<div align="center">
<img src="https://github.com/user-attachments/assets/73656a10-e27b-4275-bd26-43b980a42fee">
</div>

   - 왼쪽 자리이동과 대칭되는 것이 오른쪽 자리이동
   - MIPS 자리이동 명령어의 실제 이름은 sll(Shift Left Logical)과 srl(Shift Right Logical)
   - 예) 원래 값은 $s0에 있고, 결과는 $t2 레지스터에 저장된다고 가정
```
sll $t2, $s0, 4   # reg $t2 = reg $s0 << 4 bits
```
   - R 형식 명령어의 smart 필드 : 자리 이동량(Shift Amount)를 나타내는 것으로 자리 이동 명령어에서 사용
<div align="center">
<img src="https://github.com/user-attachments/assets/b6840202-19c1-4017-82ba-ff044ed50fd2">
</div>

   - sll은 op 코드 funct 필드가 0, rd는 10($t2), rt는 16($0), smart는 4를 갖도록 인코딩 (rs 필드는 사용하지 않으므로 0)
   - sll 명령은 왼쪽으로 i비트 자리이동하면 $2^{i}$을 곱한 것과 같은 결과가 됨
     + 마치 십진수 수를 i자리만큼 곱하면 $10^{i}$을 곱하는 것과 같음
     + 예를 들어, 위 sll는 네 자리가 이동했으므로, $2^{4} = 16$을 곱한것과 같음
     + 처음 비트열의 값이 9이고, 자리이동 후 비트열의 값은 9 X 16 = 144

4. AND : 비트 대 비트, 비트열 연산자로서, 두 비트 값이 모두 1일 경우에만 결과가 1이 됨
   - 예를 들어, $t2 레지스터의 다음과 같이 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/b512ee3f-b52f-4824-b76b-c2e5ffbb8f36">
</div>

   - $t1 레지스터에는 다음과 같은 값이 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/95d1b88b-c916-4fd0-983d-c473efa80ab5">
</div>

   - 다음과 같은 MIPS 실행
```
and $t0, $t1, $t2    # 레지스터 t1과 t2의 값을 AND 연산하고, t0에 저장
```
   - $t0 레지스터의 값
<div align="center">
<img src="https://github.com/user-attachments/assets/e8f12d8d-e823-426e-b8c4-6b8f1200664f">
</div>

   - AND는 어떤 비트 패턴에서 0의 위치를 해당하는 비트들을 강제로 0을 만드는 데 사용
   - AND와 함께 쓰이는 이러한 비트 패턴은 일부 피트를 감추는 역할을 하므로 마스크(Mask)라고 부름

5. OR : 비트 대 비트, 비트별 연산으로 두 비트 중 하나가 1이면 결과가 1이 되는 것
   - 예) $t1, $t2 레지스터의 값이 변하지 않았다고 가정할 경우, MIPS 명령어 수행 결과
```
or $t0, $t1, $t2    # 레지스터 t1와 t2의 값을 OR 연산하고, t1에 저장
```
   - $t0 레지스터의 값
<div align="center">
<img src="https://github.com/user-attachments/assets/02ee01e8-1460-46ad-a158-05d4f7d719f1">
</div>

6. NOT : 피연산자는 1개로, 각 비트의 값을 반대로 바꾸는 비트 대 비트 논리 연산, 즉, 피연산자 하나를 받아서 비트가 1이면 결과 비트를 0으로, 0이면 결과를 1로 만듬
   - NOT은 $\overline{x}$를 계산하게 됨
   - MIPS 설계자들은 3-피연산자 형식을 유지하기 위해 NOT 대신 NOR(NOT OR) 명령어를 포함
   - 피연산자 하나가 0이면 NOT과 같아짐
     + 예를 들면 NOR 0 = NOT (A OR 0) = NOT (A)
   - 예) $t1이 바뀌지 않았고, 레지스터 $t3에 0이 있다면, 다음 MIPS 명령어의 실행 결과
```
nor $t0, $t1, $t3    # 레지스터 $t1, t3의 OR 결과에 대해 NOT 연산한 뒤 $t0에 저장
```
   - $t0의 값
<div align="center">
<img src="https://github.com/user-attachments/assets/08a01896-802e-4ab8-a10f-7ce51f7ba880">
</div>

7. 상수는 산술 연산에서뿐만 아니라 AND와 OR 연산에서도 유용
   - MIPS는 andl(and immediate)와 ori(or immediate) 명령어도 제공
   - NOR의 주 용도는 NOT 대신 사용하는 것이므로, 상수는 잘 사용하지 않음
   - 따라서, 상수 피연산자를 사용하는 NOR 명령어는 구현하지 않음

8. MIPS 전체 명령어 집합에는 XOR(exclusive or) 명령도 포함되는데, 두 비트가 서로 다르면 1, 같으면 0이 됨
   - C에서는 워드 안에서 비트 필드(Bit Field) 또는 필드라는 것을 정의할 수 있음
   - 이를 이용하면, 한 워드 안에 여러 값을 넣을 수 있고, 입출력 장치같이 외부의 융통성 없는 인터페이스에 맞출 수 있음
   - 모든 필드는 한 워드에 들어갈 수 있는 크기이어야 하며, 각 필드는 최소 길이 1비트의 부호없는 정수
   - C 컴파일러는 MIPS의 논리연산 and, or, sll, srl 등을 사용해 필드를 삽입하거나 추출

9. 부호확장하는 add immediate와 달리 논리 연산 명령어인 andi(and immediate)와 ori(or immedite)는 16비트 상수를 32비트 상수로 바꿀 때 16비트에 0을 삽입
