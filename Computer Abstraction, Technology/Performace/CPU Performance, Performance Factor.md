-----
### CPU 성능과 성능 인자
-----
1. 컴퓨터의 성능을 표시하는 데 사용자가 사용하는 척도와 설계자가 사용하는 척도가 서로 다를 수 있음
2. 이 때, 서로 다른 척도 간의 상관관계를 구할 수 있다면, 설계상 변화가 사용자가 느끼는 성능에 얼마나 영향을 미치는지 평가할 수 있음
3. 가장 기본적인 척도인 클럭 사이클 수와 클럭 사이클 시간으로 CPU 시간을 표시하면 다음과 같음
<div align="center">
<img src="https://github.com/user-attachments/assets/2d86cf80-78a7-4ea6-a56a-aeda52c3da7e">
</div>

   - 클럭 속도와 클럭 사이클은 역수 관계
<div align="center">
<img src="https://github.com/user-attachments/assets/eccead44-e41c-4bce-af80-cd9ae578eae2">
</div>

   - 클럭 사이클의 길이를 줄이거나 프로그램 실행에 필요한 클럭 사이클 수를 줄이면 성능을 개선할 수 있음
   - 이 둘 중 하나를 감소시키면 다른 하나가 증가하는 경우가 자주 발생
     + 예) 클럭 사이클 수를 줄이는 기법 중에는 클럭 사이클 시간을 증가시키는 것들이 많음

4. 성능 개선 예제
   - 2 GHz 클럭의 컴퓨터 A에서 10초에 수행되는 프로그램이 존재
     + 이 프로그램을 6초 동안 실행할 컴퓨터 B를 설계하고자 함
     + 클럭 속도는 얼마든지 빠르게 만들 수 있는데, 이렇게 하면 CPU 다른 부분의 설계에 영향을 미쳐 같은 프로그램에 대해 A보다 1.2배 많은 클럭 사이클이 필요
     + B의 클럭속도 구하기

   - 우선 A에서 프로그램을 수행하는 데 필요한 클럭 사이클 수
<div align="center">
<img src="https://github.com/user-attachments/assets/c9838939-c2c0-4ddf-b3cf-a1c96a07f93f">
</div>

   - 이 값을 이용해 B에 대한 CPU 시간 구하기
<div align="center">
<img src="https://github.com/user-attachments/assets/918b9237-67ce-42f1-9566-fd4f78d6f3e7">
</div>

   - 결국 컴퓨터의 B의 클럭은 A보다 2배 빨라야 함
