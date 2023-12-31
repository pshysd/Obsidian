> 도커의 등장 배경

1. 2010년을 넘어서며 서버 시장이 급속히 클라우드 환경으로 옮겨가기 시작. 물리적인 서버를 구입하여 구축하기보다 사용 요금만 내고 가상 서버를 빌려 씀
	- 물리적인 서버를 구축하려면 서버 구입과 설치에 상당한 비용이 들었지만 클라우드 환경에서는 1대가 되었든 1,000대가 되었든 클릭 몇 번 만으로 가상 서버를 만들어낼 수 있게 됨
2. 이렇게 생성된 가상 서버에 각종 소프트웨어를 설치하고 설정을 해야 하는데 서버가 한 두대라면 쉽게 설정을 할 수 있었지만 많아지기 시작하면 사람이 하기 어려움 -> 설치와 배포가 큰 어려움으로 다가옴
	- LINUX / UNIX 환경에서 오랫동안 사용해왔던 셸 스크립트로 설치 및 설정 자동화를 구현해도 되지만 이것도 한계가 있었음. 셸 스크립트로는 중앙 관리 기능이나 복잡한 기능은 구현하기 힘들었기 때문
	- 리눅스 환경은 설치해야 할 응용 프로그램이 많고 설정도 복잡. 특히 사소한 설저 하나가 운영 체제와 서비스의 안정성에 큰 영향을 미침
3. 이런 상황에서 **[[가변 인프라와 불변 인프라|Immutable Infrastructure]]**라는 패러다임이 등장
	- 호스트 OS와 서비스 운영 환경(서버 프로그램, 소스 코드, 컴파일된 바이너리)을 분리하고, 한 번 설정한 운영 환경은 변경하지 않는다(Immutable. 불변)는 개념
	- 서비스 운영 환경을 이미지로 생성한 뒤에 서버에 배포하여 실행. 이 때 서비스가 업데이트되면 운영 환경 자체를 변경하지 않고, 이미지를 새로 생성하여 배포하는 것
	- 클라우드 플랫폼에서 서버를 쓰고 버리는 것처럼 서비스 운영 환경 이미지를 한번 쓰고 버린다.

![[Pasted image 20231228165018.png]]

4. **Immutable Infrastructure**를 구현하기 위해 **Docker**가 등장함

> 가상 머신과 Docker

- Docker는 지금까지 사용해왔던 VMware, Microsoft Hyper-V, Xen, 리눅스 KVM 등의 가상 머신과 비슷하다.
	- 가상 머신에 리눅스를 설치한 뒤 각종 서버 프로그램과 DB를 설치하고, 개발한 애플리케이션이나 웹 사이트를 실행했다. 이렇게 구축한 가상 머신 이미지를 여러 서버에 복사해서 실행하면 보통 이미지 하나로 서버를 계속 만들어 낼 수 있었다
	- 가상 머신 서버를 독자적으로 운영하거나, 서비스 형태로 제공되는 AWS, Azure, GCP 등을 사용하기도 한다.

> 가상 머신

- 가상 머신은 편하긴 하지만 성능이 좋지 못한 것이 단점이었다. 지금까지 CPU에 가상화를 위한 기능들이 많이 추가되었지만 아직도 가상 머신은 리얼 머신에 비해 속도가 느리다.
	- 전가상화(Full Virualization)의 느린 속도를 개선하기 위해 반가상화(Paravirtualization) 방식이 개발되었고, 현재 널리 쓰이고 있다.
	- ![[Pasted image 20231228173503.png]]
	- 가상 머신 자체는 완전한 컴퓨터라 항상 게스트 OS를 설치해야 한다. 그래서 이미지 안에 OS가 포함되기 때문에 이미지 용량이 커진다.
	- ![[Pasted image 20231228173552.png]]
	- 아무리 네트워크와 인터넷 속도가 빨라졌다 하더라도 가상화 이미지를 주고받는 것은 꽤 부담이다. 특히 오픈 소스 가상화 소프트웨어는 OS를 가상화하는 것에만 초점이 맞춰져있다. 그래서 이미지를 생성하고 실행하는 기능만 있을 뿐 배포와 관리 기능이 부족하다.
		- 가상 머신을 중앙에서 관리하고 배포하는 상용 제품은 VMware vCenter, Microsoft System Center가 있다.

> Docker

- Docker는 반가상화보다 좀 더 경량화된 방식이다. 게스트 OS를 설치하지 않는다.
	- Docker 이미지에 서버 운영을 위한 프로그램과 라이브러리만 격리해서 설치할 수 있고 OS 자원(시스템 콜)은 호스트와 공유한다.
	- ![[Pasted image 20231228173754.png]]
- Docker는 하드웨어를 가상화하는 계층이 없기 때문에 메모리 접근, 파일 시스템, 네트워크 속도가 가상 머신에 비해 월등히 빠르다.

|             | 성능 측정 도구 | 호스트 | Docker |
|:-----------:|:--------------:|:------:|:------:|
|     CPU     |    sysbench    |   1    | 0.9937 |
| 메모리 쓰기 |    sysbench    |   1    | 0.9913 |
| 메모리 읽기 |    sysbench    |   1    | 0.9987 |
| 디스크 I/O  |       dd       |   1    | 0.9921 |
|  네트워크   |     iperf      |   1    | 0.9893 |

- Docker는 가상 머신과는 달리 이미지 생성과 배포에 특화된 기능을 제공한다.
	- Git에서 소스를 관리하는 것처럼 이미지 버전 관리 기능을 제공한다.
	- 중앙 관리를 위해 저장소에 이미지를 올리고, 받을 수 있다.(Push / Pull)
	- GitHub처럼 Docker 이미지를 공유할 수 있는 Docker Hub도 제공한다.
- 다양한 API를 제공하기 때문에 원하는 만큼 자동화를 할 수 있어 개발과 서버 운영에 매우 유용하다.

> 리눅스 컨테이너

- 오래전부터 LINUX/UNIX 환경은 `chroot`라는 명령을 제공했다.
	- 파일 시스템에서 루트 디렉토리(/)를 변경하는 명령이다.
- chroot로 특정 디렉토리를 루트 디렉토리로 설정하면 chroot jail(감옥)이라는 환경이 생성되는데, 이처럼 chroot는 디렉토리 경로를 격리하기 때문에 서버 정보 유출과 피해를 최소화하는데 주로 사용되었다.
	- ![[Pasted image 20231229161753.png]]
	- `chroot`는 `chroot jail`에 들어갈 실행 파일과 공유 라이브러리를 직접 준비해야 하고 설정 방법이 복잡하다. 또한 완벽한 가상 환경이 아니기 때문에 각종 제약이 많음.
		- 이후 리눅스는 LXC(LinuX Container)라는 시스템 레벨 가상화를 제공
			- LXC는 컴퓨터를 통째로 가상화하여 OS를 실행하는 것이 아닌 리눅스 커널 레벨에서 제공하는 일종읙겨리(Isolate)된 가상 공간이다. 이 가상 공간에는 OS가 설치되지 않기 때문에 가상 머신이라 하지 않고 컨테이너라 부른다.
			- 리눅스 커널의 COntrol Groups(cgroups)는 CPU, 메모리, 디스크, 네트워크 자원을 할당하여 완전한 형태의 가상 공간을 제공한다. 또한 프로세스 트리, 사용자 계정, 파일 시스템, IPC 등을 격리시켜 호스트와 별개의 공간을 만든다. (Namespace Isolation. namespaces)
			- ![[Pasted image 20231229162115.png]]
			- LXC는 격리된 공간만 제공할 뿐 개발 및 서버 운영에 필요한 부가 기능이 부족했음.
			- Docker는 리눅스 커널의 cgroups와 namespaces를 기반으로 하여 이미지, 컨테이너 생성 및 관리 기능과 다양한 부가 기능을 제공한다.
			- Docker가 처음 개발될 당시에는 LXC를 기반으로 구현하였지만 최근에는 Containerd(runC)를 개발하여 사용하고 있다.
			- ![[Pasted image 20231229162306.png]]

> Docker 이미지와 컨테이너

- Docker에는 이미지와 컨테이너라는 개념이 있다.
	- 베이스 이미지
		- 리눅스 배포판의 유저 랜드만 설치된 파일을 뜻한다.
		- 보통 리눅스 배포판 이름으로 되어있다. 또는 리눅스 배포판 [[Userland]]에 REDIS나 Nginx 등이 설치된 베이스 이미지도 있다. 그래서 Docker 이미지라고 하면 베이스 이미지에 필요한 프로그램과, 라이브러리, 소스를 설치한 뒤 파일 하나로 만든 것을 말한다.
		- 각 리누스 배포판 이름으로 된 베이스 이미지는 배포판 특유의 패키징 시스템을 이용할 수 있다. 
		- 원한다면 베이스 이미지를 직접 만들 수도 있다
		- 매번 베이스 이미지에 필요한 프로그램과 라이브러리, 소스를 설치하면 용량이 큰 이미지가 중복되어 생성될 것이라고 생각하기 쉬운데, Docker 이미지는 베이스 이미지에서 바뀐 부분(Δ)만 이미지로 생성하고, 실행할 때는 베이스 이미지와 바뀐 부분을 합쳐서 실행한다.
			- ![[Pasted image 20231229162851.png]]
		- ![[Pasted image 20231229163102.png]]
		- Docker 이미지 간의 의존 관계를 보여준다.
			- Docker 이미지는 16진수로 된 ID로 구분하고, 각각의 이미지는 독립적이다. `centos:centos6` 이미지에 서비스 운영에 필요한 프로그램을 설치한 뒤 Docker 이미지를 생성하면 `example:0.1`과 같은 형태가 된다.
			- 즉 Docker는 이미지를 통째로 생성하지 않고, 바뀐 부분만 생성한 뒤 부모 이미지를 계속 참조하는 방식으로 동작한다. 이를 `레이어`라고 한다.
		- Docker 이미지는 파일이기 때문에 저장소에 올린 뒤 다른 곳에서 받을 수 있다. 그리고 저장소에 올릴 때는 자식 이미지와 부모 이미지를 함께 올린다. 받을 때도 마찬가지로 부모 이미지를 함께 받는다. 이후에는 내용이 바뀐 이미지만 주고 받는다.
	- Docker는 특정 실행 파일 또는 스크립트를 위한 실행 환경이라 보면 된다. LINUX/UNIX 계열은 파일 실행에 필요한 모든 구성 요소가 잘게 쪼개어져 있다.
		- 시스템 구조가 단순해지고 명확해지는 장점이 있지만 의존성 관계를 해결하기는 어려워짐
			- 그래서 리눅스 배포판 별로 미리 컴파일된 패키지(rpm, deb 등)라는 시스템이 나왔다. 하지만 서버를 실행할 때 마다 일일히 소스를 컴파일하거나 패키지를 설치하고, 설정하려면 상당히 귀찮다.
			- 서버가 한 두대 라면 큰 어려움이 없겠지만 클라우드 환경에서는 서버를 몇십 몇백개를 생성해야 하기 때문에 서버 구성을 미리 해놓은 Docker 이미지를 사용하면 실행할 서버가 몇 개가 되든 손쉽게 해결할 수 있다.

> **Union mount, Union File System** 
> 생성된 Docker 이미지는 읽기 전용 상태이다. 여기서 내용이 바뀌면 이미지를 수정 하지 않고 쓰기 이미지를 생성한 뒤 내용을 기록한다. 이러한 방식을 Union mount라고 하며 Union mount를 지원하는 파일 시스템을 Union File System이라고 한다.


___
> 학습 자료
> - [이재홍의 언제나 최신 Docker](https://pyrasis.com/jHLsAlwaysUpToDateDocker)
> - [이재홍의 언제나 최신 Kubernetes](https://pyrasis.com/jHLsAlwaysUpToDateKubernetes)