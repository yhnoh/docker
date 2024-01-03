### Docker Conainer 내부에서 Docker가 필요한 상황
---

- 종종 Docker 컨테이너 내부에 Docker를 실행해야하는 상황이 있다. 하지만 docker 컨테이너는 호스트와 격리되어 있기 때문에 호스트에 docker가 설치되어 있다고해서 docker 컨테이너 내부에서 docker를 사용할 수는 없다.
> 현재 내가 다니고 있는 회사에서는 개발 젠킨스의 경우 docker를 이용하여 실행중인데, 스프링 테스트 컨테이너를 실행하는 도중 젠킨스 컨테이너 내부에 docker가 존재하지 않아 오류가 발생하였다.
- 때문에 docker 컨테이너 내부에서 docker를 실행하기 위한 어떠한 방법이 필요한데 이러한 해결 방법이 DinD(Docker in Docker), DooD(Docker out of Docker)이다.

### 2. Docker Architecture

- DinD와 DooD를 구성하는 방법을 확인하기 전에 Docker Architecture에 대해서 먼저 알아보자.
![](./img/docker-architecture.png)

1. Docker Client (docker)
   - Docker Client는 Docker 사용자가 Docker와 상호작용할 수 있는 인터페이스 역할을 한다. 
   - 사용자가 docker 명령어를 입력하면 Docker Daemon과 통신하여 필요한 명령을 수행할 수 있는 입력 창구같은 역항를 한다.
2. Docker Deamon (dockerd)
   - Docker Daemon은 Docker Client로 부터 들어온 요청을 수신하여, Docker를 구성하는 객체(이미지, 컨테이너, 볼륨...)를 관리한다.
3. Registry
   - Registry는 Docker Hub나 호스트에 이미지를 저장하고 사용자가 이미지를 사용할 수 있도록 하는 저장소이다.

> 정리하자면 Docker Client를 통해서 Docker 명령어를 입력하면 Docker Daemon에서 요청을 받아 명령을 수행하고, Docker와 관련된 이미지는 Registry에서 관리한다.

- DinD와 DooD의 차이점은 컨테이너 내부의 Docker Daemon을 사용할지, 호스트에 있는 Docker Daemon을 사용할지에 따라 나뉜다.  


1. DinD (Docker in Docker)

- 호스트 도커 데몬과는 별개의 도커 컨테이너 내부에 새로운 도커 데몬을 실행시키는 방식이다.
- 호스트 도커와 완전히 격리된 새로운 도커 컨테이너를 실행시키기 때문에 호스트와 완전히 격리된 도커 실행이 가능하다.




1. DooD (Docker Out Of Docker)
- 호스트 도커 데몬이 사용하는 소켓을 공유하여 도커 클라이어트 컨테이너에서 컨테이너를 실행시칸다.
- 호스트 도커 데몬과 

> https://docs.docker.com/get-started/overview/#docker-architecture <br/>
> https://pyojuncode.github.io/Docker-DinD,-DooD/
> [젠킨스 DinD 설치 방법](https://www.jenkins.io/doc/book/installing/docker/) <br/>
> https://docs.docker.com/engine/install/debian/ <br/>
> https://bitgadak.tistory.com/3 <br/>