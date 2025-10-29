-----
### 부동 소수점 덧셈
-----
1. 부동 소수점으로 표현된 수들의 덧셈 예시) 과학적 표기법으로 표시된 두 수 9.999 X $10^{1}$와 1.610 X $10^{-1}$
   - 유효자리에 네 자리 그리고 지수에 두 자리 십진수를 저장할 수 있다고 가정
   - 단계 1 : 두 수를 더하기 위해서는 먼저 작은 지수를 갖는 수의 소수점을 정렬
     + 따라서, 작은 수 1.610 X $10^{-1}$을 큰 지수와 일치하는 형태로 바꿔야 함
     + 과학적 표기법에서 정규화되지 않은 부동 소수점 수는 다양한 표현이 존재하므로 다음과 같은 변환 가능
<div align="center">
<img src="https://github.com/user-attachments/assets/83f27d34-c5d6-4662-9de6-ce3dd87e12fe">
</div>

   - 가장 오른쪽 숫자가 필요한 형태 : 지수가 큰 수 9.999 X $10^{1}$의 지수와 일치하기 때문임
   - 따라서, 첫 번째 단계는 작은 수의 유효 자리를 지수의 큰 수의 것가 일치할 때까지 오른쪽으로 자리이동 하는 것
   - 그러나, 네 자리 십진수 만을 표시할 수 있으므로 자리이동 한 후의 수는 다음과 같음
<div align="center">
<img src="https://github.com/user-attachments/assets/38fbb8fc-2bd3-4ae2-a6b0-089b98aa5eea">
</div>

   - 단계 2 : 유효자리를 서로 더함
<div align="center">
<img src="https://github.com/user-attachments/assets/84dfa9e1-822d-493f-85a9-15665006b629">
</div>

   - 합은 10.015 X $10^{1}$
   - 단계 3 : 정규화된 과학적 표기법이 아니므로 정돈할 필요가 있음
<div align="center">
<img src="https://github.com/user-attachments/assets/ba8a7955-986f-4d58-bd01-0b22c1b9ca53">
</div>

   - 덧셈을 한 후에 이를 정규화된 형태로 만들기 위해 지수를 적절하게 조정하고 합을 자리이동 시켜야 함
   - 이 예는 오른쪽 자리이동을 보여주지만, 한 수가 양수이고, 다른 수가 음수이면 결과가 선행하는 0을 가질 수 있고, 이 때는 왼쪽 자리이동이 필요
   - 지수가 증가하거나 감소할 때마다 오버플로우와 언더플로우를 검사해야 함 : 즉, 지수가 지수 필드에 들어가는지 확인해야 함

   - 단계 4 : 유효자리가 4자리라고 가정(부호 제외)했으므로, 수를 자리맞춤(Rounding)해야 함 (반올림)
     + 다음의 값을 네 자리에서 반올림
<div align="center">
<img src="https://github.com/user-attachments/assets/3ef865b6-f77f-4396-8767-990431be300d">
</div>

<div align="center">
<img src="https://github.com/user-attachments/assets/a9c7282a-e4ed-4917-87b1-c97e7e48c9bd">
</div>

   - 소수점 오른쪽 네 번쨰 자리의 수가 5에서 9 사이이므로, 1을 더해줌
   - 반올림을 수행할 때, 연속된 9에 1을 더해주는 것과 같은 경우는 정규화 형태가 아니므로, 단계 3을 다시 수행

2. 이진수 부동 소수점 덧셈 알고리즘
<div align="center">
<img src="https://github.com/user-attachments/assets/76cdf7c5-0c04-4cc8-9c67-419bc6465695">
</div>

   - 단계 1, 2는 위와 같음 : 작은 지수를 갖는 수의 유효자리를 조정한 후 두 유효자리 수를 더함
   - 단계 3 : 오버플로우와 언더플로우 검사는 피연산자가 단일 정밀도인지, 2배 정밀도인지에 따라 달라짐
   - 지수 필드가 모두 0인 패턴은 예약되어 있으며, 0.0의 부동 소수점 표현을 위해 사용됨
   - 또한, 지수 필드가 모두 1인 패턴은 일반적인 부동 소수점의 수를 벗어나는 값들과 상황을 표현하기 위해 예약
   - 따라서, 단일 정밀도 표현 방식의 최대 지수는 127, 최소 지수는 -126

3. 예) 이진 부동 소수점 수의 덧셈 : 0.5와 -0.4375을 더하기
   - 정규화된 과학적 표기법에 따른 두 수의 이진 표현 방법 (4비트 정밀도 사용)
<div align="center">
<img src="https://github.com/user-attachments/assets/d602e77f-fae2-4742-b016-8933cc949bf6">
</div>

   - 단계 1 : 작은 지수를 갖는 수($-1.11_{2}$ X $2^{-2}$)의 유효자리를 지수가 큰 수와 일치할 때까지 오른쪽으로 자리 이동
<div align="center">
<img src="https://github.com/user-attachments/assets/a99e6844-9c8d-475d-b97d-cc39808bdaa6">
</div>

   - 단계 2 : 유효자리를 더함
<div align="center">
<img src="https://github.com/user-attachments/assets/5ac46d0e-1071-4c63-85d3-5790fff442bb">
</div>

   - 단계 3 : 오버플로우와 언더플로우를 검사하면서 합을 정규화
<div align="center">
<img src="https://github.com/user-attachments/assets/29dc69d6-a6af-4900-9741-7b0dff81835c">
</div>

   - 127 ≥ - 4 ≥ -126 이므로 언더플로우와 오버플로우는 발생하지 않음 (바이어스된 지수는 -4 + 127 = 123이 되고, 이 값은 예약되지 않은 지수 중 가장 작은 값 1과 가장 큰 값 254 사이이므로 문제 없음
   - 단계 4 : 결과를 자리맞춤
<div align="center">
<img src="https://github.com/user-attachments/assets/17cb61b4-6e7e-42ad-b202-9883f897a0f9">
</div>

   - 결과는 이미 4비트 정밀도와 일치하며, 자리맞춤할 필요가 없음
   - 결과는 다음과 같음
<div align="center">
<img src="https://github.com/user-attachments/assets/2abb7a0f-859d-4f70-a3c0-50ab05b4108e">
</div>

   - 이 값은 0.5을 -0.4375에 더한 결과이므로 옳은 연산

4. 부동 소수점 계산을 빠르게 하기 위해 전용 하드웨어를 사용하는 컴퓨터가 많이 존재
5. 부동 소수점 덧셈을 위한 하드웨어 기본 구조
<div align="center">
<img src="https://github.com/user-attachments/assets/7a372334-ab21-4745-bcd6-e7bfa841323d">
</div>

