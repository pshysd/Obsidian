> 

> EC2의 사용

- 서버를 구축할 때
	- 게임 서버, 웹 서버, 애플리케이션 서버
- 애플리케이션을 사용하거나 호스팅할 때
	- 데이터베이스
	- 머신 러닝
	- 비트코인 채굴
	- 연구용 프로그램
- 기타 다양한 목적
	- 그래픽 렌더링
	- 게임 등

> EC2의 특성

- 초 단위 온디맨드 가격 모델
	- 온디맨드 모델에서는 가격이 초 단위로 결정
	- 서비스 요금을 미리 약정하거나 선입금이 필요없음
- 빠른 구축 속도와 확장성
	- 몇 분이면 전 세계에 인스턴스 수백여대를 구축 가능
- 다양한 구성 방법 지원
	- 머신러닝, 웹 서버, 게임 서버, 이미지 처리 등 다양한 용도에 최적화된 서버 구성 가능
	- 다양한 과금 모델 사용 가능
- 여러 AWS 서비스와 연동
	- 오토스케일링, Elastic Load Balancer(ELB), CloudWatch

> EC2의 구성

- 인스턴스
	- 클라우드에서 사용하는 가상 서버로 CPU, 메모리, 그래픽카드 등 연산을 위한 하드웨어를 담당
- EBS(Elastic Block Storage)
	- 클라우드에서 사용하는 가상 하드디스크
- AMI
	- EC2 인스턴스를 실행하기 위한 정보를 담고있는 이미지
- 보안 그룹
	- 가상의 방화벽

> 실습 내용

- 목표: EC2 한 대를 프로비전하여 웹 서버 구성하기
- 순서
	1. EC2를 구성하기 위한 AMI 선택
	2. EC2의 유형과 사이즈 선택
	3. EBS 설정
	4. 보안 그룹 설정
	5. EC2 생성
	6. EC2 접속 후 웹 서버 설치 및 웹 서버 실행
	7. 웹 브라우저에서 접속 테스트