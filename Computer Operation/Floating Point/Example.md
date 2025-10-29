-----
### 부동 소수점 표현 방식
-----
1. -0.75을 IEE 754 이진 표현법에 따라 단일 정밀도 및 2배 정밀도 표현 방식
   - 0.75은 -3/4 또는 -3 / $2^{2}$ 이며, 이는 $-11_{2}$ / $2^{2}$ 또는 $-0.11_{2}$로 표현 가능
   - 과학적 표기법에서 값은 $-1.1_{2}$ X $2^{-1}$
   - 단일 정밀도의 일반적인 표현 방식 : $(-1)^{S}$ X (1 + 소수) X $2^{지수-127}$
     + 따라서, $-1.1_{2}$ X $2^{-1}$의 지수에 바이어스 127을 빼면, 결과는 $(-1)^{1}$ X (1 + .1000 0000 0000 0000 0000 $000_{2}$) X $2^{126-127}$
     + 단일 정밀도 표현에 따른 -0.75의 표현
<div align="center">
<img src="https://github.com/user-attachments/assets/b7d0f04b-3ee5-4de8-a37c-6fc52e204a77">
</div>

   - 2배 정밀도 표현
<div align="center">
<img src="https://github.com/user-attachments/assets/69711862-369a-4c77-90a3-4eb5c9791c4f">
</div>

2. 이진 부동 소수점 수를 십진 부동 소수점 수로 변환
   - 아래 단일 정밀도 부동 소수점 수가 나타내는 값을 십진수로 표시
<div align="center">
<img src="https://github.com/user-attachments/assets/e847dca7-2eea-4f53-9b24-f58553f01d77">
</div>

   - 부호 비트는 1, 지수 부분은 129, 소수 부분은 1 X $2^{-2}$ = 1/4 또는 0.25
<div align="center">
<img src="https://github.com/user-attachments/assets/ffb2238a-e76b-481d-a3bf-c821eb9ee175">
</div>

