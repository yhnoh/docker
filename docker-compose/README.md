
### 도커 컴포즈
---
- 공통의 목적을 같는 어플리케이션 스택을 yaml 코드로 정의해서 한번에 서비스를 올리고 관리할 수 있는 도구이다.
- 도커 컴포즈로 실행된 컨테이너는 독립된 기능을 가지며 공통 네트워크로 구성되기 때문에 컨테이너간 통신이 쉽다
- 테스트, 개발, 운영의 모든 환경에서 구성이 가능한 오케스트레이션 도구 중 하니이지만 다양한 관리 기능을 갖고 있지 않기 때문에 테스트와 개발환경에 적합하다.
  - 실제 운영환경에서는 도커 스웜이나 쿠버네티스 같은 오케스트에이션 도구가 가지고 있는 자동 확장, 모니터링, 복구 등의 운영에 필요한 기능을 함께 사용하는 것을 권장한다.

### 도커 컴포즈 네트워크
---
- docker-compose up 명령을 수행하면 자체 기본 네트워크가 생성된다.
  - 기본 네트워크를 `디렉터리명_default`이름으로 생성한다.
- 이 기본 네트워크를 통해 IP가 아닌 서비스명(컨테이너명)으로 서비스간 통신이 가능하다.
- 도커 컴포즈의 네트워크는 자동 생성되지만 사용자가 지정 네트워크를 설정할 수 있다.

### 도커 컴포즈 작성 방법
---
- 도커 컴포즈는 yaml 파일로 작성되기 때문에 기본적인 문법을 숙지하고 있어야 한다.
- docker-compose.yml을 구조
    ```yml
    version: "3.0"
    services:
        service1:
            # 애플리케이션 설정값 정의
        service2:
            # 애플리케이션 설정값 정의 

    networks:
    # 네트워크 설정, 미지정 시 자동 생성
    volumes:
    # 볼륨 설정
    ```
    - 일반적으로 가장 먼지 실행되어야 하는 애플리케이션을 작성하고, 이와 의존성을 갖는 데이터베이스 및 하위 어플리케이션을 작성한다.
    - 그 다음 의존성을 갖는 데이터베이스 및 하위 애플리케이션을 작성한다.
#### version 정의
- 도커 컴포즈의 버전은 도커 엔진 릴리스와 연관되기 때문에 도커 엔진 릴리스에 맞게 적합한 버전을 설정해 주어야 한다.
    ```yml
    version: "3.0"
    ```
#### services 정의
- 다중 컨테이너 서비스 실행을 목적으로 하기 때문에 복수형으로 작성된다.
- 별도 이미지 개발 없이 도커 허브에서 제공하는 이미지를 사용하는 경우 `image`를 사용한다.
    ```yml
    version: "3.0"
    services:
        nginx:
            image: nginx:latest
        mariadb:
            image: mariadb:10.4.6
    ```
- Dockerfile을 작성하여 컨테이너를 실행하는 경우에는 미리 빌드해서 `image`를 작성해도 되지만 `build`옵션을 사용하면 도커 컴포즈 실행과 함께 이미지가 빌드된다.
    ```yml
    version: "3.0"
    services:
        ngnix:
            build: .
    ```
    - `build`옵션을 통해서 Dockerfile 경로를 지정하고 docker-compose.yml 파일과 동일한 경로에 위치한 경우에는 '.'을 이용해 빌드할 수 있다.
    - Dockerfile의 위치와 파일명이 다른 경우에는 `context`와 `build` 옵션을 통해서 정의할 수 있다.
#### serivce 하위 옵션
|옵션|설명|
|---|---|
|container_name|컨테인 이름을 부여하는 옵션이며 생략시 자동으로 부여된다. docker run의 --name과 동일한 옵션|
|ports|서비스 내부 포트와 외부 호스트 포트를 지정하여 바인드하는 옵션이며 docker run의 -p와 동일한 옵션|
|expose|호스트 운영체제와 직접 연결하는 포트를 구성하지 않고, 서비스만 포트를 노출. 필요시 링크로 연결된 서비스와 서비스 간의 통신에 사용|
|networks|최상위 레벨의 networks에 정의된 네트워크 이름을 작성하며 docker run의 -net 옵션과 동일|
|volumes|서비스 내부 디렉터리와 호스트 디렉터리를 연결하여 데이터 지속성을 설정. docker run의 -v와 동일한 옵션|
|enviroment|서비스 내부 환경 변수 설정. 환경 변수가 많은 경우에는 파일로 만들어 evn_file 옵션에 파일명을 지정한다. docker run의 -e 옵션과 동일하다.|
|command|서비스가 구동 이후 실행할 명령어를 작성하는 옵션|
|restart|서비스 재시작을 지정하는 옵션이며 docker run의 --restart와 동일한 옵션, (no: 수동 재시작, always: 컨테이너 수동 제어를 제외하고 항상 재시작, on-failure: 오류가 있을 시 재시작)|
|depends_on|서비스 간의 종속성을 의미하며 머저 실행해야하는 서비스를 지정하여 순서를 지정하는 옵션, 이 옵션에 지정된 서비스가 먼저 시작됨|

#### 네트워크 정의
- 다중 컨테이너들이 사용할 최상위 네트워크를 정의하고 하위 서비스 단위로 이 네트워크를 선택할 수 있다.
- `networks` 지정 시 해당 이름의 네트워크가 생성되고, 대역은 172.x.x.x로 자동으로 할당되며 기본 드라이버는 브리지로 지정된다.
- 도커에서 생성한 기존 네트워크를 지정하는 경우에는 external 옵션에 네트워크 이름을 작성한다.
    ```yml
    version: "3.0"
    services:
        #...
    networks:
        default:
            external:
                name: 네트워크 이름
    ```
- `docker network ls` 명령어를 통해서 도커 네트워크를 확인 가능하다.
    ```commandline
    $ docker network ls
    NETWORK ID     NAME                              DRIVER    SCOPE
    830e57820f83   associated-relationship_backend   bridge    local
    6cdd9a4177ab   associated-relationship_default   bridge    local
    ee2359a3c925   bridge                            bridge    local
    f1b30b7f6016   host                              host      local
    cdd1ffed972c   none                              null      local
    ```
#### 볼륨 정의
- 데이터 지속성을 유지하기 위해 최상위 레벨에 볼륨을 정의하고, 서비스 레벨에서 볼륨명과 서비스 내부의 디렉터리를 바인드한다.
- `docker volume create`와 동일하게 도커거 관리하는 가상영역(/var/lib/docker/volumes)에 자동으로 배치된다.
    ```commandline
    $ docker volume ls
    DRIVER    VOLUME NAME
    local     jenkins_home
    ```
- `docker volume ls` 명령어로 확인이 가능하고 `docker inspect 볼륨명`으로 세부 디렉터리 경로까지 확인 가능하다.
    ```commandline
    $ docker inspect --type volume jenkins_home
    [
        {
            "CreatedAt": "2022-10-17T14:06:52Z",
            "Driver": "local",
            "Labels": null,
            "Mountpoint": "/var/lib/docker/volumes/jenkins_home/_data",
            "Name": "jenkins_home",
            "Options": null,
            "Scope": "local"
        }
    ]    
    ```
- 최상위 레벨에 volumens를 정의하지 않고 서비스 하위 레벨에 호스트의 절대경로와 컨테이너의 디렉터리 경로를 직접 사용하는 바인드 마운트 방식도 가능하다.
  - ${PWD}를 이용해 현재 docker-compose가 위치한 디렉토리에 바인드 마운트 할 수 있다.