-----
### x86 명령어 인코딩
-----
1. 80386은 여러 가지 명령어 형식을 사용해 명령어를 아주 복잡하게 인코딩
   - 80386의 명령어 길이는 1바이트 (피연산자가 없는 명령어) 부터 15바이트까지 매우 다양

2. 대표적인 명령어 형식
<div align="center">
<img src="https://github.com/user-attachments/assets/cec35515-e04f-4cb1-b05e-4714b58daf40">
</div>

  - op-code 바이트에는 피연산자의 크기가 8비트인지 32비트인지 표시하는 비트가 포함되어 있는 것이 보통
  - 어떤 명령어에서는 주소 지정 방식과 레지스터 필드가 opcode에 포함
    + 레지스터 = 레지스터 op 상수 형태의 명령어는 포함
    + 다른 명령어들은 mod, reg, r/m이라는 추가 opcode 바이트(Postbyte)를 사용하며, 여기에는 주소 지정 방식에 관한 정보가 들어감
    + postbyte는 메모리에 접근하는 명령어에서 주로 사용
  - 베이스 + 스케일링 인덱스 주소지정 방식은 sc, index, base라는 두 번째 Postbyte 사용

3. 16비트와 32비트 주소지정 방식에서 두 Postbyte 주소 지정자의 인코딩
<div align="center">
<img src="https://github.com/user-attachments/assets/42167e18-b90c-4c82-abe8-001fde223cf8">
</div>
