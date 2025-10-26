-----
### 중첩된 프로시저
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/2ffa6443-0d23-4613-9811-2ba73fdcc901">
</div>

1. 다른 프로시저를 호출하지 않는 프로시저 : 말단(Leaf) 프로시저
2. 프로시저도 다른 프로시저를 호출할 수 있음
   - 자기 자신을 호출하는 프로시저 : 재귀(Recursive) 프로시저

3. 예) 주 프로그램이 인수값을 3을 가지고 프로시저 A를 호출했다고 가정
   - 이 때, 레지스터 $a0에 3을 넣고, jal A 명령을 실행했다고 가정
     + 프로시저 A가 다시 인수 7($a0에 저장)을 가지고 jal B을 통해 프로시저 B를 호출했다고 가정
     + 아직 A가 다 끝난 것이 아니므로 레지스터 $a0 사용에서 충돌이 발생
     + 마찬가지로 레지스터 $ra에 지금은 B에 복귀주소가 있으므로, $ra 복귀 주소에도 충돌이 발생
   - 한 가지 방법 : 값이 보존되어야 할 모든 레지스터를 스택에 넣는 것
     + 호출 프로그램은 인수 레지스터($a0 ~ $a3)와 임시 레지스터($t0 ~ $t9) 중 프로시저 호출 후에도 계속 사용해야 하는 것은 모두 스택에 넣음
     + 피호출 프로그램은 복귀 주소 레지스터 $ra와 저장 레지스터($s0 ~ $s7) 중에서 피호출 프로그램이 사용하는 레지스터를 모두 저장
   - 스택 포인터 $sp는 스택에 저장되는 레지스터 개수에 맞추어 저장
   - 복귀한 후에는 메모리에서 값을 꺼내 레지스터를 원상 복구하고 이에 맞춰 스택 포인터 다시 조정

4. 예) 재귀 프로시저의 컴파일 : n 계승을 계산하는 재귀프로시저에 해당하는 MIPS 어셈블러 코드
<div align="center">
<img src="https://github.com/user-attachments/assets/75f26a8c-74d5-4451-9054-42d8f1ac185e">
</div>

   - 인수 n는 인수 레지스터 $a0에 해당
   - 번역된 프로그램은 프로시저 레이블오 시작하며, 뒤이어 복귀 주소와 $a0를 스택에 저장하는 명령어가 나옴
<div align="center">
<img src="https://github.com/user-attachments/assets/f92db180-00b5-4af1-bbc3-5838323e569b">
</div>

   - fact가 처음 호출되었을 때, sw는 fact를 호출할 프로그램의 주소를 저장
   - 다음은 n이 1보다 작은지 검사해서, n ≥ 1이면, L1으로 가게 하는 명령어
<div align="center">
<img src="https://github.com/user-attachments/assets/3d7d00c4-5550-404b-af26-571c2dc489cf">
</div>

   - n이 1보다 작으면, 1을 결과값 레지스터에 넣음
   - 이 때, 0에다 1을 더해서 $v0에 넣음
   - 복귀하기 전 스택에 저장된 값 2개를 버리고, 복귀 주소로 점프
<div align="center">
<img src="https://github.com/user-attachments/assets/8a29ef13-9cb8-40d1-8e95-9aaed12a5aa6">
</div>

   - 스택에서 값 2개를 꺼내서 $a0와 $ra에 넣을 수 있으나, n이 1보다 작을 때 $a0와 $ra는 변하지 않으므로 그럴 필요가 없음
   - n이 1보다 작지 않으면, 인수 n을 감소시키고 이 감소된 값으로 다시 fact를 호출
<div align="center">
<img src="https://github.com/user-attachments/assets/462da8a4-469f-4559-829f-3c878bc7b268">
</div>

   - 호출한 프로그램으로 되돌아가는 부분 : 스택 포인터를 이용해 이전 복귀 주소와 인수 값을 구함
<div align="center">
<img src="https://github.com/user-attachments/assets/8c00998e-f721-44f4-8407-b1ee6b482b67">
</div>

   - 인수 $a0와 결과값 레지스터에 현재 값을 곱해서 $v0에 넣음
<div align="center">
<img src="https://github.com/user-attachments/assets/4de5d92e-cd17-4a64-a7ea-eb55db760010">
</div>

   - 마지막으로 복귀 주소를 이용해 되돌아감
<div align="center">
<img src="https://github.com/user-attachments/assets/e9d1188e-b4eb-4b04-b6ab-4d5ab3122d42">
</div>

5. C 변수는 기억 장치의 한 장소에 해당 : 기억된 내용을 어떻게 해석하는가는 데이터형(Type)과 저장 유형(Storage Class)에 따라 달라짐
   - 예를 들어, 정수형 / 문자형이 있음
   - C에는 자동(Automatic)과 정적(Static) 두 가지 저장 유형이 존재
   - 자동 변수는 프로시저 내에서만 정의되는 것으로 프로시저가 종료되면 없어짐
   - 정적 변수는 프로시저로 들어간 후나 프로시저에서 빠져나온 후에도 계속 존재
   - 모든 프로시저 외부에서 선언된 C 변수는 정적 변수로 간주되며, static이라는 키워드를 사용해 선언된 변수도 마찬가지
   - 그 나머지는, 모두 자동 변수
   - 정적 변수에 대한 접근을 단순화하기 위해 MIPS 소프트웨어는 전역 포인터(Global Pointer, $gp)라는 레지스터 사용
     + 전역 포인터 : 정적 영역을 가리키도록 예약된 레지스터

6. 프로시저 호출 전후의 값 보존 관계
<div align="center">
<img src="https://github.com/user-attachments/assets/e00622e2-0ec4-496f-aa8d-74167df17c23">
</div>

   - 여러 가지 방법으로 스택을 보존하여 호출 프로그램 스택이 저장한 값과 같은 값을 꺼낼 수 있게 보장하고 있음
   - 피호출 프로그램이 $sp보다 위쪽에는 값을 쓰지 못하게 함으로써 $sp 윗 부분 스택을 원상태로 유지
   - $sp 자체는 뺀 값만큼 피호출 프로그램이 도로 더해서 원래 값을 유지
   - 다른 레지스터는 (만일 프로시저내에서 사용되면) 스택에 저장했다가 다시 꺼내서 원래 값 유지하게 됨
