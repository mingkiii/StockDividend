# StockDividend Project
주식 배당금 정보를 제공하는 API서비스 입니다.

## ⚙️ Tech Stack
- Spring Boot, Java 8, JPA, H2, Redis, Jsoup, Docker
- https://www.yahoo.com/ 에서 주식 배당금 정보 스크래핑

### 📋 Process
- Auth
  - 회원 등록 
    - roles - 권한(일반(read), 관리자(read,write)) 부여
    - 중복 ID 배제 - 이미 사용중인 ID인 경우 예외 발생
    - password 암호화

  - 로그인
    - 존재하지 않는 ID인 경우 예외 발생
    - ID의 password가 맞지 않을 경우 예외 발생
    - 로그인 성공시 JWT 부여

- Finance
  - 회사명으로 배당금 정보 조회 - 스크래핑
    - 회사명으로 스크래핑 하여 회사의 배당금 정보 반환
    - 캐시에 {key = 회사명, value = 배당금 정보} 저장
    - 잘못된 회사명일 경우 예외 발생

- Company
  - 회사 추가 - 관리자 권한
    - 회사의 ticker로 회사 정보 저장
    - 존재하지 않는 회사 ticker인 경우 예외 발생
    - 이미 저장된 회사인 경우 예외 발생

  - 회사 조회
    - 저장된 모든 회사 목록 반환(Page인터페이스 형태로 size = 10)
    - 자동 완성으로 검색 
      - keyword로 시작하는 회사명인 회사 목록 10개 반환
  
  - 회사 배당금 정보 업데이트
    - 스케줄러 매일 0시마다 캐시에 저장된 모든 정보 삭제하고 새로 스크래핑 하여 새로운 값 저장

  - 회사 정보 삭제
    - ticker로 회사
    - 회사의 배당금 정보 삭제
    - 캐시에 저장된 정보 삭제
  
  
  
  
