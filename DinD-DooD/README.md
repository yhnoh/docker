- Jenkins와 같은 CI/CD 툴을 docker 컨테이너로 이용할 때, 컨테이너 내부에 docker를 설치해야하는 상황이 종종 발생한다.
> 현재 내가 다니고 있는 회사에서는 개발 젠킨스의 경우 docker를 이용하여 실행중인데, 스프링 테스트 컨테이너를 실행하는 도중 젠킨스 컨테이너 내부에 docker가 존재하지 않아 오류가 발생하였다.

- docker 컨테이너는 호스트와 격리되어 있는 상황이기 때문에 호스트에 docker가 설치되어 있다고해서 docker 컨테이너 내부에서 docker를 사용할 수는 없다.
- 때문에 docker 컨테이너 내부에서 docker를 실행하기 위한 어떠한 방법이 필요한데 이러한 해결 방법이 DinD(Docker in Docker), DooD(Docker out of Docker)이다.

1. DinD  

> https://docs.docker.com/engine/install/debian/ <br/>
> https://bitgadak.tistory.com/3 <br/>