# 2주차 실습

1. DOSBOX SIC 시뮬레이터 마운트
```
mount c: X:\sic\
```
- `X:\sic\`은 본인 SIC 시뮬 설치 루트로

2. sic 폴더 내의 `SICEditor`로 `SRCFILE`수정

3. `save`하고 DOSBox에서 `sicasm` 명령어 입력으로 어셈블링

4. `l2u DEVF2`로 모두 대문자 전환

5. `sicsim` 입력

6. `b 1000`으로 1000에 브레이크 포인트 걸고 r 12번 정도

7. `h 1`하고 `r`

8. `d r,1000-1020`으로 덤프