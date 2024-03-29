> AWS의 구조

![[스크린샷 2024-02-20 오후 8.20.34.png]]

> VPC란?

> "VPC(Virtual Private Cloud)는 사용자의 AWS 계정 전용 가상 네트워크입니다. VPC는 AWS 클라우드에서 다른 가상 네트워크와 논리적으로 분리되어 있습니다. Amazon EC2 인스턴스와 같은 AWS 리소스를 VPC에서 실행할 수 있습니다. IP 주소 범위와 VPC 범위를 설정하고 서브넷을 추가하고 보안 그룹을 연결한 다음 라우팅 테이블을 구성합니다."
우리가 Netflix, Github 등을 이용할 때는 퍼블릭 인터넷을 이용한다.

AWS 클라우드도 마찬가지로 퍼블릭 인터넷을 통하여 이용한다. 이게 일반적인 사용 방법임.

이 중에 예외가 있는데, VPC이다.

VPC는 퍼블릭 인터넷에서 접근이 불가능한 것이 원칙.

- 가상으로 존재하는 데이터 센터
- 외부에 격리된 네트워크 컨테이너 구성 가능
	- 여기서 말하는 컨테이너는 도커같은거 사용할 때의 컨테이너가 아니고 일종의 네트워크를 담을 수 있는 유닛 같은 의미이다.
	- 원하는대로 사설망 구축 가능
		- 부여된 IP 대역을 분할하여 사용 가능
- 리전 단위

> VPC의 사용 사례

- [[EC2]], [[RDS(Relational Database Service)]], [[Lambda]] 등의 AWS의 컴퓨팅 서비스 실행
- 다양한 서브넷 구성
- 보안 설정(IP BLock, 인터넷에 노출되지 않는 EC2 등 구성)

> VPC의 구성 요소

- [[Subnet|서브넷]]
- [[IGW(Internet Gateway)|인터넷 게이트웨이(IGW)]]
- [[NACL(Network ACL)|NACL]]/[[보안그룹]]
- [[Route Table|라우트 테이블]]
- NAT Instance/NAT Gateway
- Bastion Host
- VPC Endpoint