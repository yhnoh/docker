### Dockerfile

- Dockerfile은 원하는 개발환경을 코드로 구성하여 이미지 또는 컨테이너를 제공한다.
    > IaC (Infrastructure as Code)는 인프라 구축을 코드화하여 개발한다.
#### Dockerfile을 사용하는 이유
- 수정사항이 생기면 Dockerfile 내부에서 코드를 수정하면 되기 때문에 변경에 유연하다.
- 해당 변경사항을 서버에 반영하기 전에 변경된 이미지 또는 컨테이너를 먼저 로컬 PC에서 테스트가 가능하다.
- 커맨드 기반으로 인프라를 구성시 사용자 실수가 발생할 수 있다. 서버가 많아질 수록 사용자 실수가 발생할 확률이 높다.
- Dockerfile을 통해서 인프라 구축시 동일한 환경을 많은 서버에 배포할 수 있다.
- 코드를 작성한 파일형식으로 되어있기 때문에 공유가 가능하다.
    - 형상 관리 시스템에서도 관리가 가능하기 때문에 변경 이력을 확인할 수 있다.
- 

#### Dockerfile 사용시 고려해야할 것들
1. 경량의 컨테이너 서비스를 제공
2. Dockerfile에 담기는 레이어를 최소화
3. 하나의 어플리케이션은 하나의 컨테이너에서 제공
4. 캐시 기능을 활용
5. IaC 환경 개발은 디렉터리 단위로
6. 서버리스 환경으로 개발

### Dockerfile 명령어
|명령어|설명|비고|
|---|---|---|
|내용1|내용2|내용3|


### 이미지 생성을 위한 Dockerfile 빌드
- docker build 명령어를 통해서 dockerfile로 부터 이미지를 생성할 수 있다.
> `docker build [옵션] 이미지명:[태그] path | URL | 압축 파일(tar | tar.gz)`

#### 1. 옵션
- -t : '이미지명:[태그]'를 지정한다.
    - 일반적으로 태그는 버전 관리 차원으로 고려한다.
    - `docker build -t image:1.0 image:2.0`과 같이 여러 이미지를 동새이 생성할 수 있다.
- -f : Dockerfile이 아닌 다른 파일명을 사용하는 경우에 사용된다.

#### 2. 경로
- path : 주로 Dockerfile을 디렉토리 단위 개발을 권고하고, 현재 경로에 Dockerfile이 있다면 '.'을 사용한다.
    - `docker build .`
- url : Dockerfile이 포함된 URL을 제공한다.
- 압축 파일 : 압축 파일 내에 Dockerfile이 포함된 경우 사용된다.

### Dockerfile 작성 라이프 사이클
- Dockerfile은 인프라 구성을 위해 필요한 명령을 담은 일반 텍스트 문서다.
- Dockerfile은 빈 디렉터리에서 생성하여 빌드하는 것을 권장한다.
    - 빌드가 시작되면 현재 디렉토리에 있는 모든 파일과 디렉토리 콘텐츠는 도커 데몬에 빌드 컨텍스트로 전달되기 때문이다.
    > docker build를 실행하는 현재 작업 중인 디렉토리를 `빌드 컨텍스트`라고 부른다.
    > 빌드 컨텍스트에서 제외대상이 있다면 `.dockerignore` 파일에 작성하면 된다.
- Dockerfile을 통해서 이미지 빌드시 문법적 오류나 오타가 발견되면 빌드 과정에서 에러가 출력된다. (유효성 검사 실시)
- 동일한 이미지를 두번 빌드할 때 `Using Cache`절이 많이 나온다.
    - 이것을 `빌드 캐시`라고 한다. docker build는 빌드 속도 향상을 위해서 실행 중간에 있는 이미지 캐시를 사용한다.
    > 빌드 과정에서 캐시를 사용하지 않는 경우 --no-cache를 지정하여 빌드하고, 다른 이미지로 부터 만들어진 캐시를 사용해 빌드하려면 --cache-from 옵션을 통해 할 수 있다.
- docker 18.09 버전에 Buildkit이 추가되어 미지 빌드에 향상된 기능을 제공하기 시작했다.
    - 빌드 과정을 병렬 처리하여 더 빠른 빌드를 제공한다.
    - 사용되지 않는 빌드 단계를 찾아 비활성화 한다.
    - 비밀번호 등의 민감한 데이터가 포함되는 경우 비밀 구축이 가능하다.
    - 빌드 중 빌드 정보에 따라 변경된 파일만 전송한다.
    - 자동 빌드 시 빌드 캐시의 우선순위를 정한다.
    - `export DOCKER_BUILDKIT=1`
### docker build 속도


#### Dockerfile 레이어
- Dockerfile에 정의된 모든 명령이 레이어가 되는 것은 아니다.
    - RUN, ADD, COPY 이 세가지의 명령어만이 레이어에 저장된다.
    - CMD, LABEL, ENV, EXPOSE 등과 같이 메타정보를 다루는 명령어는 저장되지 않는다.

> 최적화된 Dockefile을 통해서 빌드를 수행하면 읽기 전용 레이어가 생성되며 컨테이너 실행 시 스냅샷처럼 활요할 수 있다.
#### 최적화된 Dockerfile 작성하기
1. FROM 명령어 사용시 용량이 작인 이미지 사용하기
    ```Dockerfile
    ## 변경 전
    FROM ubuntu:20.04

    ## 변경 후
    FROM python:3.9.2-alpine
    ```
2. COPY와 같은 명령어 사용시 RUN명령어 밑에서 사용하기
    - COPY에 해당하는 파일이 변경되면 그 이후의 모든 레이어의 빌드 캐시는 무효화 된다.
    ```Dockerfile
    ## 변경 전
    COPY app.py /app
    RUN apt-get update && apt-get -y install python python-pip
    RUN pip install -r requirement.txt

    ## 변경 후
    RUN apt-get update && apt-get -y install python python-pip
    RUN pip install -r requirement.txt
    COPY app.py /app
    ```
3. 최적화된 Dockefile을 통해서 빌드를 수행하면 읽기 전용 레이어가 생성되며 컨테이너 실행 시 스냅샷처럼 활요할 수 있다.

### Dockerfile 다양한 방법으로 구성하기
1. 셸 스크립트를 이용한 환경 구성 실습
    ```Dockerfile
    ## 셸 스크립트를 작성하고 실행 권한을 부여한다.
    RUN echo './etc/apache2/envvars' > /webapp/run_http.sh && \
        ehco 'mkdir -p /var/run/apache2' >> /webapp/run_http.sh && \
        ehco 'mkdir -p /var/lock/apache2' >> /webapp/run_http.sh && \
        echo '/usr/sbin/apache2 -D FOREGROUND' >> /webapp_run_http.sh && \
        chmod 744 /webapp/run_http.sh 
    ## ...
    CMD /webapp/run_http.sh   
    ```
2. ADD 명령어의 자동 압축 해제 기능 활용
    - ADD 명령어는 압축 파일을 해제하여 경로에 복사하는 장점이 있다.
    ```Dockerfile
    ##...
    ADD webapp.tar.gz /var/www/html
    ##...
    ```
3. 이미지 용량 절감하기
    - ubuntu의 경우 apt를 이용한 패키지 업데이트와 설치 시 남게 되는 캐시를 제거하여 생성되는 이미지의 용량이 감소됨을 확인할 수 있다.
    ```Dockerfile
    FROM ubuntu:14.04
    ##...
    RUN apt-get update && \
        apt-get install apache2 -y -qq --no-install-recommends && \
        apt-get clean -y && \
        apt-get autoremove -y && \
        rm -rfv /var/lib/apt/lists/* /tmp/* /var/tmp/*
    ## ...
    ```

### Docker 이미지 효율성 검증 도구
- dive