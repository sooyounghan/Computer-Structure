-----
### 실례 : Intel Core i7
-----
1. 작업부하와 벤치마크
   - 작업부하 : 사용자가 실제로 실행하는 응용 프로그램들의 모음 또는 그러한 모음과 유사한 특성을 갖도록 실제 프로그램에서 발췌하여 구성한 프로그램들의 집합
     + 전형적인 작업부하는 프로그램 목록과 각 프로그램의 상대적인 실행 빈도를 같이 표시
   - 두 컴퓨터의 시스템을 평가하려면 두 컴퓨터에서 같은 작업부하의 실행시간만 비교하면 됨
   - 하지만, 일반적으로 성능을 측정하기 위해 선택된 프로그램의 집합, 벤치마크(Benchmark, 컴퓨터 성능을 비교하기 위해 선택된 프로그램)를 사용해 성능 평가
     + 벤치마크는 사용자의 실제 작업부하에 대한 성능을 잘 반영할 것으로 생각되는 프로그램들로 구성된 작업부하

2. SPEC(System Performance Evaluation Cooperative) CPU 벤치마크 : 최신의 컴퓨터 시스템을 위한 표준 벤치마크를 만들기 위해 여러 컴퓨터 회사에서 만든 벤치마크
    - SPEC 정수형 벤치마크 프로그램과 Intel Core i7에서 프로그램들의 실행 시간
<div align="center">
<img src="https://github.com/user-attachments/assets/74fd513d-4059-4519-86e3-943af4a55f07">
</div>

   - 실행시간을 결정하는 요소인 명령어 개수, CPI와 클럭 사이클 시간을 함께 보여주며, 프로그램에 따라 CPI가 4배 이상 차이가 남

3. SPE-Cratio
   - SPEC은 정수형 벤치마크 10개의 결과를 숫자 하나로 요약하기로 결정 : 기준 프로세서 실행시간을 측정하려는 컴퓨터의 실행시간으로 나누어 실행 시간을 정규화한 결과
   - SPE-Cratio가 클수록 성능이 더 좋은 컴퓨터 (즉, SPE-Cratio는 실행 시간의 역수이며, 이를 기하평균해서 SPECspeed 2017의 요약값을 구함)
     + SPE-Cratio를 이용해 2개의 컴퓨터를 비교할 때, 기하 평균을 이용 (그러면 정규화에 사용한 컴퓨터가 무엇인지 상관없이 동일한 상대적 답을 제공하며, 정규화된 실행시간의 산술평균을 구한다면, 그 값은 어떤 컴퓨터를 기준으로 선정하느냐에 따라 달라짐)
     + 기하평균 공식
<div align="center">
<img src="https://github.com/user-attachments/assets/a1614d7d-a3e2-4da5-9c89-5d86eed63290">
</div>

   - 여기서 (실행시간 비)는 작업부하 n개 프로그램 중 i번째 프로그램의 실행 시간을 기준으로 정규화한 값
<div align="center">
<img src="https://github.com/user-attachments/assets/3b6e92e2-43ba-4321-a826-87ca8c354e71">
</div>

4. SPEC 전력 벤치마크
   - 에너지와 전력의 중요성 증대에 따라 SPEC은 전력 측정을 위한 벤치마크를 추가 : 일정시간 동안 작업부하를 10%씩 증가시키면서 서버 전력 소모 측정
   - SPECpower는 Java 비즈니스 응용을 위한 SPEC 벤치마크로부터 시작
     + 프로세서와 캐시, 메인모메리를 사용할 뿐만 아니라 Java 가상 머신 / 컴파일러 / 가비지 컬렉터(Garbage Collector), 운영체제의 일부를 작동시킴
     + 이 벤치마크에서 성능은 처리량으로 측정되며, 단위는 초당 수행되는 비즈니스 연산
     + 이 값들은 '전체 ssj_ops/wait'라는 숫자 하나로 표현되며, 척도에 관한 식
<div align="center">
<img src="https://github.com/user-attachments/assets/f347de96-f80c-4fa1-adc1-d4deedc4fcc0">
</div>

   - $ssj_ops{_i}$는 10%씩 증가될 때 마다 성능
   - $power_{i}$는 각 성능 수준에서 소비되는 전력
