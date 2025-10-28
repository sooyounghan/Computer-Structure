-----
### 배열과 포인터
-----
1. 배열을 사용한 clear (clear1)
   - 인수 array와 size는 레지스터 $a0와 $a1에서 찾을 수 있고, 변수 i는 $t0에 할당되었다고 가정
<div align="center">
<img src="https://github.com/user-attachments/assets/79d4cfee-87a1-4694-b83e-564d0c760eca">
</div>

   - for 순환문의 첫 부분은 i의 초기화
```
move $t0, $zero    # i = 0 (레지스터 $t0 = 0)
```
   - array[i]를 0으로 하려면, array[i]의 주소를 구해야 하는데, i에 4를 곱해서 바이트 주소를 계산
```
loop1: sll  $t1, $t0, 2    # $t1 = i * 4
```
   - 배열의 시작 주소는 레지스터에 있으므로 add 명령어를 이용하여 시작 주소를 인덱스에 더함
```
add  $t2, $a0, $t1    # $t2 = array[i]의 주소
```
   - 마지막으로 해당 주소에 0을 저장
```
sw   $zero, 0($t2)    # array[i] = 0
```
   - 이 명령어가 순환문 본체의 끝이므로 다음은 i를 증가시키는 것
```
addi  $t0, $t0, 1     # i = i + 1
```
   - 반복 검사는 i가 size보다 작은지 비교
```
slt  $t3, $t0, $a1      # $t3 = (i < size)
bne  $t3, $zero, loop1  # (i < size)라면, loop1로 이동
```
   - 배열 인덱스를 이용해 배열을 0으로 만드는 MIPS 코드
<div align="center">
<img src="https://github.com/user-attachments/assets/751ea2f3-71e4-43eb-bf3f-1c8d02b761af">
</div>

2. 포인터를 사용한 clear
   - 포인터를 사용하는 두 번쨰 프로시저는 두 인수 array와 size를 레지스터 $a0와 $a1에, 변수 p를 레지스터 $t0에 할당
   - 두 번쨰 프로시저는 포인터 p에 배열의 첫 번쨰 원소의 주소를 넣는 것으로 시작
```
move $t0, $a0    # p = array[0]의 주소
```
   - for 순환의 본체로서 p가 가리키는 메모리에 0을 넣음
```
loop2: sw  $zero,  0($t0)    # Memry[p] = 0
```
   - 이 명령어가 순환의 본체를 구성 : 순환 증가로서 p가 다음 워드를 가리키도록 하면 됨
```
addi $t0, $t0, 4    # p = p + 4
```
   - C에서 포인터 하나 증가시키는 것은 포인터를 다음 원소로 옮긴다는 뜻
     + p는 정수 포인터이고, 정수는 4바이트이므로 컴파일러는 p를 4 증가시킴

   - 반복 검사 : 첫 단게는 array의 마지막 원소의 주소를 계산 (바이트 주소를 구하기 위해 size에 4를 곱함)
```
sll  $t1, $a1, 2    # t1 = size * 4
```
   - 이 값을 array의 시작 주소에 더해서 array의 마지막 워드 다음 주소를 구함
```
add  $t2, $a0, $t1  # t2 = array[size]의 주소
```
   - 반복 검사는 p가 array의 마지막 원소 주소보다 작은가를 검사하면 됨
```
slt  $t3, $t0, $t2      # t3 = (p < &array[size])
bne  $t3, $zero, loop2  # (p < &array[size])라면, loop2로 이동
```
   - 전체 코드
<div align="center">
<img src="https://github.com/user-attachments/assets/1f6b2e95-f9b7-4b16-9b2c-9a563c79b037">
</div>

   - 첫 번쨰 프로그램과 마찬가지로 size는 0보다 커야 함
   - array의 마지막 원소 주소는 변하지 않는 값인데도 이 프로그램을 반복할 떄마다 매번 새로 계산 : 이 계산을 순환문 밖으로 꺼내면, 실행 속도가 빨라짐
<div align="center">
<img src="https://github.com/user-attachments/assets/d7024095-6d12-43a5-9d14-033167a8e51a">
</div>

3. 두 프로그램 비교 (포인터로 바뀐 부분은 파란색 표시)
<div align="center">
<img src="https://github.com/user-attachments/assets/a38e5d75-f2e2-4ebe-b9ea-0085d169a72f">
</div>

   - 왼쪽 프로그램 : 매번 i를 증가시킨 후 새 인덱스 값으로 주소를 새로 계산하므로, 순환문 내에서 곱셈과 덧셈을 해야 함
   - 오른쪽 프로그램 : p를 직접 증가시키고, 배열의 마지막 원소 주소 계산을 순환문 바깥으로 빼냄으로 반복마다 실행되는 명령어를 6개에서 4개로 줄임
   - 이는 컴파일러 최저화 중 강도 감소(곱셈 대신 자리이동), 유도 변수 제거(배열 주소 계산을 순환에서 제거)에 해당

4. C 컴파일러라면, size가 0보다 크다는 것을 확인하기 위해 추가 검사 실시하는데, 이를 위한 한 가지 방법으로 순환의 첫 명령어 직전에 slt 명령어로 점프를 삽입하는 것
