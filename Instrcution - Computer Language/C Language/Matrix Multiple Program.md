-----
### 행렬 곱셈 프로그램
-----
1. C로 작성한 프로그램 (DGEMM : Double precision CEneral Matrix Multiply)
<div align="center">
<img src="https://github.com/user-attachments/assets/d9c5dee3-296f-472c-abe8-c8e3b9611a59">
</div>

   - 좀 더 나은 성능을 위해 1차원 행렬 C, A, B와 주소 연산을 사용

2. 내부 순환문의 x86 어셈블리 언어 프로그램
<div align="center">
<img src="https://github.com/user-attachments/assets/7450aa95-a147-409b-a9f9-17c8ca9e379f">
</div>

   - 5개의 부동 소수점 명령어는 v로 시작
   - 스칼라 2배 정밀도(Scalar Double Precision)을 의미하는 sd가 이름에 포함

3. C 프로그램의 성능을 최적화 매개변수를 변화시키면서 Python 프로그램과 대비해서 보여줌
<div align="center">
<img src="https://github.com/user-attachments/assets/b6eb2d68-1461-45f8-97d6-48dbca383e4d">
</div>

   - 최적화하지 않은 C 프로그램 조차 월등히 빠르며, 최적화 레벨을 증가시킴에 따라 훨씬 빨라지는 것을 알 수 있음
   - 성능 향상의 원인은 인터프리터 대신 컴파일러를 사용한 것과 C의 타입 선언이 컴파일러로 하여금 훨씬 더 효과적 코드를 생성할 수 있게 해줌
