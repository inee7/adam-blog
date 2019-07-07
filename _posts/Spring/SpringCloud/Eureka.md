# Eureka (Dynamic Service Discovery)

- 수많은  API 서버의 IP Address와 Port 전달
- Ribbon 자동 설정
- 서버 감지하고 목록에 자동 추가/삭제
  - 인스턴스 관리 편함 
- API 호출시 주소 말고 이름으로 호출 
- 서버에 Eureka Client 탑재하여 서버 주소 Eureka Server로 전달 
- Eureka Client는 주기적으로 Eureka Server로 부터 Service별 IP 목록을 Fetch하여 보관 