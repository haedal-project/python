
# 1. Instagram 자동화 (2021.01 ~ 2021.03)

<img src="https://github.com/user-attachments/assets/d54f5cac-d750-4474-bd13-ac89623842ba" />

<br> 

## 주요 기능
- **자동 좋아요**: 해시태그 기반으로 타 계정 게시글에 자동 좋아요
- **자동 팔로우**: 좋아요와 연계하여 자동 팔로우
- **언팔 관리**: 맞팔이 아닌 계정을 자동으로 언팔로우


## 구현 과정

### 자동 좋아요 (2021.01)
`while True` 루프로 CSS 선택자를 통해 좋아요 버튼 상태를 감지하고 클릭.   
봇 감지 방지를 위해 랜덤 딜레이(2~5초) 적용

### 자동 좋아요 + 팔로우 (2021.01)
`if/elif`로 팔로우 버튼 텍스트 상태 분기 처리. 

봇 제재 방지를 위해 딜레이를 10~15초로 상향 조정

**CSS 선택자 오류 수정**
```
button.sqdOP.yWX7d.y3zKF → div.bY2yH > button.sqdOP.yWX7d
```

### 예외 처리 (2021.02)
- 사진 미로드 시 `try/except`로 다음 사진으로 스킵
- 경고 메시지 중첩 발생 시 중첩 `try/except` 구조로 처리
- 제재 방지를 위해 `while True` → `for d in range(80)` 변경
- 제자리 맴돌기 방지: 현재 URL을 파일에 저장 후 해당 위치부터 재시작

### 언팔 관리 (2021.03)
| 차수 | 방식 |
|:---:|:---|
| 1차 | 크롤링 결과를 Excel(.xlsx)로 저장 후 수동 비교 |
| 2차 | 팔로워/팔로잉 각각 txt 파일로 저장, `set` 차집합으로 언팔 대상 추출 |
| 3차 | 파일 없이 리스트 `a`, `b`에 메모리 저장 → 크롤링 완료 후 바로 언팔 실행 |
| 4차 | 인스타 UI 업데이트로 변경된 CSS 선택자 반영 |
| 5차 | 화면 크기별 CSS 분기 처리, 함수로 중복 코드 제거 |


<br><br><br> 

---

# 2. 쿠팡 최저가 상품 알림 봇 (Python)

<img src="https://github.com/user-attachments/assets/34e01427-aa62-4b9c-969f-d65442c26ac5" width="30%"/>

<img src="https://github.com/user-attachments/assets/7d628252-be36-438c-9ec0-21106b87aaf1" width="25%" />  

<br> 

## 주요 기능
- 특정 상품의 가격을 주기적으로 크롤링하여 원하는 가격대 상품을 Line 메신저로 자동 알림

## 구현 과정

### 1. 전체 페이지 반복 크롤링
`page_num`을 증가시키며 상품이 없는 페이지가 나올 때까지 `while True`로 전 페이지 순회

### 2. 가격대 필터링
문자열 슬라이싱으로 4,000원대 / 5,000원대 분류 후 각 리스트에 상품명·링크 저장

### 3. Line API 알림 연동
```python
headers = {'Authorization': 'Bearer {발급키}'}
requests.post('https://notify-api.line.me/api/notify', headers=headers, data=data)
```

### 4. Windows 작업 스케줄러로 자동 실행 설정

### 6. 출력 형식 개선
- **1차**: 4,000원대 상품 존재 시 5,000원대 미출력 / 없을 시 안내 메시지 + 5,000원대 출력
  
- **2차**: 메모장 없이 가독성 있는 포맷으로 메시지 구성

