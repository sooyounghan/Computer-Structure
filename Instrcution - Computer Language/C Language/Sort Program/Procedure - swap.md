-----
### 프로시저 swap
-----
<div align="center">
<img src="https://github.com/user-attachments/assets/76269a85-17b2-48b5-8533-6a3fb61ccfb6">
</div>

1. 이 프로시저는 메모리 내 두 값을 단순히 바꾸는 것
   - 프로그램 변수에 레지스터 할당
   - 프로시저 본체에 해당하는 코드를 생성
   - 프로시저 호출 후 레지스터 내용을 호출 전과 같도록 만듬

2. 프로시저 swap의 레지스터 할당
   - MIPS에서는 인수를 전달하는데 레지스터 $a0, $a1, $a2, $a3을 사용
   - swap의 인수는 v, k 2개뿐이므로 레지스터 $a0와 $a1에 할당
   - 그 외 변수는 temp 하나뿐인데, swap은 말단 프로시저이므로 레지스터 $t0에 할당

3. 프로시저 swap의 본체 프로그램
   - swap의 나머지 C 코드
<div align="center">
<img src="https://github.com/user-attachments/assets/f92104cf-c733-4085-af20-feb280e3df76">
</div>

   - MIPS는 바이트 주소를 사용하므로 각 워드는 실제로 4바이트씩 떨어져있음
   - 그러므로 인덱스 k를 주소에 더하기 전에 4를 곱해야 함 : 순차적인 워드 주소가 1이 아닌, 4씩 차이남
   - v[k]의 주소를 구하기 위해 처음해야 할 것은 k에 4를 곱하는 것
<div align="center">
<img src="https://github.com/user-attachments/assets/19556984-b13b-4dcd-a33d-ed371ed7157f">
</div>

   - $t1을 이용해 v[k]를 적재하고, $t1에 4를 더해 v[k + 1]를 적재
<div align="center">
<img src="https://github.com/user-attachments/assets/b3258388-c3bc-4dda-ac09-44def9f6a513">
</div>

   - $t0와 $t2를 맞바꾼 주소에 저장할 차례
<div align="center">
<img src="https://github.com/user-attachments/assets/253e00b3-9672-4278-8279-f94888a60008">
</div>

4. 완성된 프로시저 swap : 프로시저 레이블과 복귀를 위한 점프만 추가하면 전체 루틴 완성
<div align="center">
<img src="https://github.com/user-attachments/assets/0f475475-4ecc-46e2-8f80-0a2aa935d0c7">
</div>

----
### 프로시저 sort
----
1. 버블정렬(교환정렬) 방법으로 정수 배열을 정렬하는 프로그램
   - 버블정렬 : 가장 빠른 정렬 방법은 아니지만, 가장 간단한 방법 중 하나
<div align="center">
<img src="https://github.com/user-attachments/assets/4c30a5e3-64b4-48d5-aecc-d8464d09112a">
</div>

2. 프로시저 sort의 레지스터 할당
   - 프로시저 sort의 두 인수 v와 n은 인수 레지스터 $a0, $a1에 넣고, 변수 i와 j는 각각 레지스터 $s0와 $s1을 할당

3. 프로시저 sort의 본체 프로그램 - 첫 번쨰 순환문
   - 프로시저 본체는 중첩된 for 순환문 2개와 프로시저 swap을 호출하는 부분으로 구성
   - 바깥쪽 프로그램
```
for(i = 0; i < n; i += 1)
```
   - C언어의 for문장은 초기화, 반복조건 검사, 순환 제어 변수 증가 세 단계로 구성 : i값을 초기화하는 것이 for 문장 가장 앞부분에 해당
```
move   $s0, $zero    # i = 0
```
   - move는 어셈블러가 제공하는 의사명령어
   - i값을 증가시키는 부분은 for 문장의 가장 마지막 부분으로, 다음 명령어 하나로 번역
```
addi   $s0, $s0, 1    # i += 1
```
   - 순환문에서 i < n이 아니면, 순환에서 빠져나옴 (즉, i ≥ n이면, 순환이 종료)
   - slt 명령으로 $s0 < $a1이면, $t0을 1로, 아니면 0으로 만들 수 있음
   - $s0 ≥ $a1인가를 검사해야 하므로, $t0가 0일 때 분기하면 됨
```
for1tst: slt    $t0, $s0, $a1      # $s0 ≥ $a1 (i ≥ n)이면, $t0 = 0
         beq    $t0, $zero, exit1  # $s0 ≥ $a1 (i ≥ n)d이면, exit1로 이동
```
   - 순환문의 맨 끝은 처음 반복조건 검사로 되돌아가는 명령어
```
        j    for1tst              # 바깥쪽 루프로 이동
exit1:
```
   - 첫 번쨰 for 순환문 형태
<div align="center">
<img src="https://github.com/user-attachments/assets/0f72e3df-467c-4a2a-ab20-2cd9e0e62bad">
</div>

4. 프로시저 sort의 본체 프로그램 - 두 번쨰 순환문
```
for (j = i - 1; j >= 0 && v[j] > v[j + 1]; j -= 1)
```
   - 순환문의 초기화 부분
```
addi  $s1, $s0, -1    # j = i - 1
```
   - j 값을 감소시키는 것도 하나의 명령어
```
addi $s1, #s1, -1     # j -= 1
```
   - 반복 조건 검사는 두 부분
     + 두 조건 중 하나라도 거짓이면 순환문에서 빠져나와야 하므로, 첫 번째 검사에서 실패하면(즉, j < 0) 순환문에서 나옴
```
for2tst: slti   $t0, $s1, 0        # $s1 < 0 (j < 0)이면, $t0 = 1
          bne   $t0, $zero, exit2  # $s1 < 0 (j < 0)이면, exit2로 이동
```
   - 이 분기 명령이 실행되면 두 번째 검사 생략
   - j ≥ 0이면, 두 번쨰 조건 검사 : 두 번째 조건 검사에서 v[j] ≤ v[j + 1]이면, 순환문에서 빠져나옴
     + 먼저 j에 4를 곱해서 v의 시작 주소에 더함
```
sll  $t1, $s1, 2      # $t1 = j * 4
add  $t2, $a0, $t1    # $t2 = v + (j * 4)
```
   - 이 주소를 이용해 v[j]를 읽음
```
lw   $t3, 0($t2)      # $t3 = v[j]
```
   - 다음 원소 주소는 4만큼 크므로, 레지스터 $t2에 4를 더해서 v[j + 1]를 읽음
```
lw   $t4, 4($t2)      # $t4 = v[j + 1]
```
   - v[j] ≤ v[j + 1] 검사는 v[j + 1] ≥ v[j]와 같으므로, 반복 조건 검사는 다음과 같음
```
slt  $t0, $t4, $t3      # $t4 ≥ $t3이면, $t0 = 0
beq  $t0, zero, exit2   # $t4 ≥ $t3이면, exit2로 이동
```
   - 마지막 명령은 내부 순환문의 조건 검사로 점프
```
j  for2tst    # 내부 순환문으로 이동
```
   - 두 번쨰 for 순환문
<div align="center">
<img src="https://github.com/user-attachments/assets/a5f5480b-2f04-4d9d-a57b-abbd94e248d8">
</div>

5. 프로시저 호출
   - 두 번째 for 순환의 본체 부분
```
swap(v, j);
```
   - swap 호출
```
jal  swap
```

6. 인수 전달
   - sort 프로시저도 레지스터 $a0와 $a1을 사용하고, swap 프로시저도 인수 전달을 위해 똑같른 레지스터를 사용해야 하므로 인수 전달 방법이 문제가 됨
   - 한 가지 방법은 프로시저 앞부분에서 sort의 인수를 다른 레지스터에 복사해서 swap을 호출할 때, 레지스터 $a0와 $a1을 사용할 수 있게 하는 것 (이렇게 복사하는 것이 스택에 저장했다 복구하는 것보다 빠름)
   - 먼저, $a0와 $a1을 각각 $s2와 $s3에 복사
```
move $s2, $a0    # $a0을 $s2로 인수 복사
move $s3, $a1    # $a1을 $s3로 인수 복사
```
   - swap에 인수 전달
```
move $a0, $s2    # 첫 번째 swap 인수 v
move $a1, $s3    # 두 번째 swap 인수 j
```

7. 프로시저 호출 전과 후의 레지스터 내용 같게 유지하기
   - 레지스터 내용을 저장했던 복구하는 프로그램 : sort 자신도 프로시저로서, 호출된 입장이므로 레지스터 $ra의 복귀 주소를 저장해야 함
   - 그 외에 저장해야 하는 레지스터는 $s0, $s1, $s2, $s3 등
   - 그러므로 sort 프로시저의 앞부분은 다음과 같음
<div align="center">
<img src="https://github.com/user-attachments/assets/0b63c5da-02b4-4759-970d-e65363251357">
</div>

   - 프로시저 끝부분은 위의 각 명령어의 역에 해당하는 명령에다 jr 명령만 추가

8. 완성된 프로시저
<div align="center">
<img src="https://github.com/user-attachments/assets/a2733183-76dc-4b6b-bf93-f172468d6096">
</div>

   - for 순환문 내 레지스터 $a0와 $a1에 대한 참조가 $s2와 $s3으로 변경

9. 프로시저 확장(Procedure Inlining) : 최적화 기법 중 하나
    - 인수를 전달하고 jal 명령으로 호출하는 대신, swap 호출 부분에 컴파일러가 swap 프로시저 본체의 코드를 복사
    - 명령어 4개를 줄일 수 있지만, 단점으로는 프로시저가 여러 번 호출될 때 코드 크기가 커진다는 점
    - 코드가 커져서 캐시 실패율이 증가하면 성능이 나빠질 수 있음

10. 최적화 컴파일러가 성능 / 컴파일 시간 / 클럭 사이클 / 명령어 개수 / CPI 등에 미치는 영향
<div align="center">
<img src="https://github.com/user-attachments/assets/b825bc6d-4a1f-4c72-8768-cbf14d33ed3a">
</div>

   - CPI는 최적화하지 않은 코드가 가장 좋고, 명령어 개수는 O1이 가장 적지만, 가장 빠른것은 O3 (프로그램 성능에 대한 정확한 척도는 오로지 실행시간 뿐임)

11. 프로그램 언어의 효과, 컴파일 대 인터프리팅, 알고리즘이 정렬 성능에 미치는 영향
<div align="center">
<img src="https://github.com/user-attachments/assets/92744778-3d96-4686-bfff-77c1f7d894d6">
</div>

   - 네 번쨰 열에서 보면, 버블정렬에서 최적화되지 않은 C 프로그램이 인터프리된 자바 코드보다 8.3배 빠름
     + JIT 컴파일러를 사용하면 자바 프로그램이 최적화 되지 않은 C 프로그램보다 2.1배 빠름
     + 최고로 최적화된 C 코드보다는 겨우 1.13배 정도만 느림
   - 다섯번쨰 열을 보면, 퀵 정렬의 성능 비율이 버블 정렬과 다른데, 실행 시 컴파일에 걸리는 추가 시간을 상쇄하기에는 실행 시간이 너무 짧음
   - 마지막 열은 알고리즘 영향을 보여줌
     + 데이터 100,000개를 정렬할 때, 알고리즘에 따라 천 배 이상 성능 차이가 나는 것을 알 수 있음
     + 인터프리팅 자바로 구현한 퀵 정렬(다섯 번째 열)과 최고로 최적화하여 컴파일한 C 코드 버블정렬(네 번째 열)을 비교해서 퀵 정렬이 50배 정도 빠름

12. 인수 저장이 필요할 때 MIPS 컴파일러는 항상 스택에 여유 공간을 확보해둠
    - 그러므로 실제 인수 레지스터 4개 (16바이트)를 모두 저장할 공간을 만들기 위해 $sp를 16만큼 감소
    - 이렇게 하는 이유 중 하나는 C 언억 vararg 옵션을 제공 : 이 옵션은 포인터가, 예를 들면 프로시저의 세 번째 인수를 꺼낼 수 있도록 해줌
    - 컴파일러가 vararg를 만나면 인수 레지스터 4개를 모두 스택의 예약된 장소에 복사
