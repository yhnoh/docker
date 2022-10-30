### Docker Volume

![](imgocker-volume.png)

- 애플리케이션에서 발생하는 데이터에 접근하고 이것을 공유하기 위해서 도커 볼륨 기능을 사용할 수 있다.
- 애플리케이션에서 발생하는 여러 가지 상황이 데이터에 영향을 주지 않고 언제든 다른 컨테이너로 이전할 수 있다면 운영자는 데이터를 안전하게 관리하고 운영할 수 있다.
- 도커 볼륨은 컨테이너에서 생성, 재사용할 수 있고 호스트 운영체제에서 직접 접근이 가능하다.
- 또한 보존되어야 하는 데이터를 유지하기 위한 메커니즘을 제공한다.
- 일반적으로 컨테이너 내부의 데이터는 컨테이너의 라이프사이클과 연관되어 종료 시 삭제된다.
- 도커 볼륨을 사용하면 컨테이너가 삭제되어도 볼륨은 독립적으로 운영되기 때문에 함께 삭제되지 않는 특징이 있다.



### Docker Volume 타입

- 호스트 파일 시스템의 특정 디렉토리와 컨테이너의 디렉터리를 연결하여 데이터를 저장하기 위한 여러 방법들이 있다.

#### 1. volume
   - 도커에서 권장하는 방법으로 `docker volume create [volume name]`을 통해 볼륨을 생성한다.
   - 도커 볼륨은 도커 명령어를 통해 관리할 수 있다.
   - 여러 컨테이너 간에 안전하게 공유할 수 있다.
   - 볼륨 드라이버를 통해 원격 호스트 및 클라우드 환경에 볼륨 내용을 저장하고 암호화할 수 있다.
   - 새 볼륨으로 지정될 영역에 데이터를 미리 채우고 컨테이너에 연결하면 컨테이너 내에서 바로 데이터를 사용할 수 있다.
   ```shell
   ## volume 생성
   docker volume create [volume name]
   
   ## volume 조회
   docker volume ls
   
   ## volume 확인
   docker volume inspect [volume name]
   
   ## --mount 옵션을 이용한 볼륨 지정
   docker run -d --name [container name] --mount soruce=[volume name],target=[container directory] [image name]
   
   ## -v 옵션을 이용한 볼륨 지정
   docker run -d --name -v [volume name]:[container directory] [image name]
   
   ## docker volume create 하지 않아도 호스트 볼륨 이름을 쓰면 자동 생성
   docker run -d --name -v [not create volume name]:[container directory] [image name]
   
   ## 볼륨 제거, 현재 연결된 컨테이너가 있으면 에러 발생
   docker volume rm [volume name]
   ```

#### 2. bind mount

- 도커 볼륨 기법에 비해 사용이 제한적이다.
- `호스트 파일 시스템 절대 경로:컨테이너 내부 경로`를 직접 마운트 하여 사용한다.
- 사용자가 파일 또는 디렉터리를 생성하면 해당 권한은 소유자 권한으로 연결되고, 존재하지 않은 경우에는 자동 생성되어 root 권한으로 연결된다.
- 컨테이너 실행 시 지정하여 사용하고, 컨테이너 제거 시 바인드 마운트는 해제되지만 호스트 디렉터리는 유지된다.


```shell

## -mount 옵션으로 호스트 경로와 바인드 마운트 지정
docker run -d --name [container name] --mount type=bind,soruce="${pwd}"/[host directory],target=[container directory] [image name]

## -v 옵션으로 호스트 경로와 바인드 마운트 지정
docker run -d --name [container name] --v ${pwd}"/[host directory]:[container directory] [image name]

## ro를 사용하여 읽기 전용으로 호스트 경로와 바인드 마운트 지정
docker run -d --name [container name] --v ${pwd}"/[host directory]:[container directory]:ro [image name]
## ro를 사용하여 읽고 쓰기 호스트 경로와 바인드 마운트 지정
docker run -d --name [container name] --v ${pwd}"/[host directory]:[container directory]:rw [image name]


```

