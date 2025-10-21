# Vector
## 에러 발생 대처
### requires a larger buffer size, lines are too long. Skipping file.
- Tail Input으로 수집중이던 로그파일보다 길이가 긴 로그 발생하여 해당 로그들 수집 대상에서 제외하는 내용
- Buffer_Max_Size 조절하여 해결
    - default : 32k
