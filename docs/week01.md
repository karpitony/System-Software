# 1주차 이론

- 레지스터 24bit
- PC(프로그램 카운터) 
    - 한 명령어씩 넘어감
    - 시작되자마자 다음 수행핢 명령어의 메모리 주소를 가지고 있음
    - 메모리 주소를 옮겨가면서 next instruction(다음 수행할 명령어!!) 바이트 주소
- SW
    - 상태값을 갖고 있음 ex) if문의 결과
    - 다양한 컨디션 관련 저장
    - Condition Code(CC) 포함
- X(indeX 레지스터)
    - 한 워드씩 증가하는데 어드레싱 할때 사용
    - 어드레싱: 다이렉트, 인다리엑트, pc 랠러티브, base 랠러티브
- A(범용 레지스터)
    - 5에 10더할 때 5를 A에 넣고, ADD로 10을 읽으면 A레지스터에 15
    - 이중에서 혼자만 데이터 들어감
- 레지스터의 Number는 알필요 없지만, 5개의 레지스터가 뭔지, 뭐하는지는 알아야 한다.

- SIC 머신
    - 정수하고 문자만 (실수(floating point)가 없다)
    - 정수: 24bit binary number
        - 음수: 2의 보수를 취한다.
    - 문자: 8-bit ASCII 코드
- opcode x address
-     8       1      15
    - LDA 16 일떄 LDA가 opcode에, 
    - x가 0이면 direct, 1이면 indexed모드여서 address + (X 레지스터 값) 주소 찾아가기


- SIC/XE Insturciton Set Table은 외울 필요 X, 갖고와서 셤봐도 됨, 파란색이 SIC
- J = JUMP 값이 같으면 이동시키기
- COMP = A레지스터와 값 비교하고 condition code(CC)를 SW 레지스터에 저장 (True False 결과를 저장)
- ADD, SUB, MUL, DIV 10 -> 10 주소에 가서 값을 A 레지스터에 연산 (10을 더하는게 아님!!!!!!)

- JLT, JEQ, JGT -> Conditional Jump, CC값을 보고 조건에 따라 점프
- JSUB 서브루틴을 점프, L 레지스터에 돌아올 주소(지금 PC값) 저장하고, PC값을 원하는 메모리 주소로 바꿈
- RSUB L레지스터 값을 읽어서 PC에 저장
    - L레지스터 값이 덮어쓰면 안되서 2번 점프는 못하지만… 복잡한 컴퓨터는 스택 사용하면 계속 뽑아가면서 가능

- Input & Output 한번에 8bit씩 옮겨서 A레지스터에
- 3개의 I/O 명령어 가지고 있음
    - TD: Test device
    - 장치 준비 됐는지 확인
    - 결과를 CC에 저장
    - RD: Read data
        - A레지스터의 8bit를 가져옴
    - WD: Write data
        - A레지스터 오른쪽에 8bit 추가함

- RESW (Reserve Word): 메모리 한칸(워드 크기) 비워놔라 ~ 변수용 
- WORD : Word 크기의 상수
- BYTE : 한 바이트 상수
- RESB : 한 바이트 변수

- SIC/XE 머신
- 2^20승 메모리
- B: 베이스 레지스터, 어드레싱용
- S, T 레지스터: 범용 레지스터
- F 레지스터: Floating-point accumulator (48 bits)
    - s exponent fraction
    - 1       11             36
    - sign bit s, 지수부(부호가 없다. 1024보다 작으면 -, 1024보다 크면 +), 가수부(정규화 한다)
    - 시소프에서는 F덕분에 실수도 포함된다 정도만~

- SIC/XE 포맷 3하고 SIC하고 호환
- b, p, e가 주소로 표현된다 -> 이걸 누가 표시해줘야 함 
    - n, i가 어떤 값을 가지면 b, p, e가 주소번지다
    - 0, 0이면 flag로는 의미 없고, 주소의 일부

- 포맷2: 8bit, op, r1, r2만 No memory reference -> r1, r2는 레지스터
- 레지스터끼리 연산 sic보다 레지스터가 늘어나서 레지스터끼리 연산
- 포맷1: 메모리 레퍼런싱 X

- 포맷4: 메모리를 넓힘
    - e flag가 0이냐 1이냐에 따라 3번 4번 포맷 결정
- nixbpe < flag 순서는 중요

- 어드레싱 모드
- Direct addressing: 그대로 사용하는 것
    - 포맷 3, 4에서 b, p가 0일 때
- Indexed addressing
    - x가 1일 때, combined(어떠한 방법이든 인덱스 어드레싱은 다 포함 가능)
- Relative addressing
    - 포맷 3에서만
    - base 레지스터 값을 가지고 상대주소 만듬 (b = 1, p = 0)
    - TA = (B) + disp (0 <= disp <= 4095)
    - base = 거의 시작주소를 가리켜서 0이상부터
    - pc 레지스터 값을 가지고 상대주소 만듬 (b = 0, p = 1)
    - TA = (PC) + disp (-2048 <= disp <= 2047)
    - pc는 주소가 변하기 때문에 -도 가능

- x, b, p, e
    - b, p는 상호베타적이다.
    - x는 모든 주소 계산 방법에 포함될 수 있다.
- n, i: 타겟 주소를 어떻게 사용할건지
    - n = 0, i = 1, x = 0 : 즉시 주소(immediate)
        - TA 값 자체가 값이다
    - n = 1,i = 0, x = 0 : 간접 주소
        - TA 주소로 가면 또 주소가 있다.
        - 그 주소를 따라가면 값이 있다.
    - n = 0, i = 0 : SIC 형식 주소? operand values
    - n = 1, i = 1 : SIC/XE 형식 주소 

SIC : direct 주소만 있음
immedate 즉시 : 주소 자체가 값, 앞에 #붙여서 즉시를 표현
direct 직접 : 주소로 찾아가면 바로 값
indirect 간접 : 주소로 찾아가면 또 주소

- RMO : Registe move 레지스터 값 옮기기
- SVC: 수퍼바이저 콜 -> 일종의 인터럽트인데 이 두개는 간단하게만~

- IO 디바이스 똑같이, 한 바이트씩 A 레지스터 8비트
- IO 채널: 별도의 프로세스가 있다~ 컴구조 시간에, 
    - CPU는 빠르고 IO는 느리니까 CPU가 명령 던져놓을 채널
    - CPU가 IO 때문에 병목 안걸리게

