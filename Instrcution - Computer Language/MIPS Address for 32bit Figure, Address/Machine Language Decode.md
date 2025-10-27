-----
### 기계어의 해독
-----
1. 기계어로부터 원래의 어셈블리 언어를 추출하는 작업
   - 예) 코어 덤프(Core Dump)를 읽을 때 사용
   - MIPS 기계어 필드의 값 : 어셈블리 언어와 기계어를 손으로 변환할 때 유용
<div align="center">
<img src="https://github.com/user-attachments/assets/2f06beea-82c5-43b1-ba2b-c0811c6643ae">
</div>

2. 예) 다음 기계어에 해당하는 어셈블리 명령어
```
00AF 8020 (hex)
```
   - 16진수를 이진수로 바꾸어 op 필드 찾기
```
0000  0000  1010  1111  1000  0000  0010  0000
```
   - 위 표를 참고하면 비트 31 ~ 29가 000이고, 비트 28 ~ 26도 000이므로 R형식 명령어
   - R 형식에 맞게 이진수를 R형식 필드에 맞게 재구성
<div align="center">
<img src="https://github.com/user-attachments/assets/ea9bb946-b591-4187-bc9b-0bda011e5fd5">
</div>

```
  op      rs      rt      rd     shamt   funct
000000   00101   01111   10000   00000   100000
```
   - R형식 명령어 중에서 어떤 것인지 확인 : 비트 5 ~ 3이 100이고, 2 ~ 0이 000이므로 add 명령어
   - 나머지 필드 해독
     + rs 필드의 값은 십진수 5, rt 필드는 15, rd 필드는 16 (add 명령은 shamt 필드를 사용하지 않음)
     + 이 레지스터 번호는 순서대로 $a1, $t7, $s0에 해당
```
add $s0, $a1, $t7
```
   - MIPS 명령어 형식 전체
<div align="center">
<img src="https://github.com/user-attachments/assets/ea9bb946-b591-4187-bc9b-0bda011e5fd5">
</div>
