## Container > Container Registry > 사용 가이드

## 사전 준비
### Docker(도커) 설치
Container Registry 서비스는 Docker 컨테이너 이미지를 저장하고 배포하기 위한 서비스입니다. 컨테이너 이미지를 다루기 위해서는 우선 사용자의 환경에 Docker가 설치되어 있어야 합니다.

#### Windows
Docker Hub에서 [Docker Desktop for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)를 다운로드해 설치합니다.

#### macOS
Docker Hub에서 [Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)을 다운로드해 설치합니다.

#### Linux
Linux 배포판에 따라 설치 과정이 다릅니다. CentOS 7, Ubuntu가 아닌 다른 배포판을 사용한다면 [Install Docker Engine](https://docs.docker.com/engine/install)을 확인하세요.

* CentOS 7
```
// Docker 설치에 필요한 패키지 설치
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2

// Docker 저장소 추가
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

// Docker 설치
$ sudo yum install -y docker-ce docker-ce-cli containerd.io

// Docker 서비스 시작
$ sudo systemctl start docker
```

* Ubuntu
```
// apt 인덱스 업데이트
$ sudo apt-get update

// repository over HTTPS를 사용하기 위한 패키지 설치
$ sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common

// GPG Key를 추가하고 확인
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]

// 저장소 추가하고 apt 인덱스 업데이트
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update

// Docker 설치
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

// Docker 서비스 시작
$ sudo systemctl start docker
```

### 앱키(Appkey) 확인
Docker 명령줄 도구(CLI)를 이용해 사용자 레지스트리에 로그인하려면 서비스 Appkey 또는 프로젝트 통합 Appkey가 필요합니다. 서비스 Appkey는 **Container > Container Registry** 서비스 페이지의 **URL & Appkey** 버튼을 클릭해 확인할 수 있습니다. 프로젝트 통합 Appkey는 **프로젝트 설정 페이지의 API 보안 설정**에서 생성해 사용할 수 있습니다.

## 컨테이너 레지스트리 사용

> [참고]
> 멤버 권한의 사용자는 컨테이너 이미지 저장, 삭제 기능은 사용할 수 없습니다.

### 사용자 레지스트리 주소 확인
사용자 레지스트리 주소는 **Container > Container Registry** 서비스 페이지의 **레지스트리 주소** 버튼을 클릭해 확인할 수 있습니다.

### 사용자 레지스트리 로그인
컨테이너 이미지를 저장하거나, 원하는 환경으로 가져오려면 Docker CLI 도구를 이용해야 합니다. Docker CLI 도구를 이용해 사용자 레지스트리에 접근하려면 로그인을 해야 합니다. 로그인에 사용되는 정보는 TOAST 사용자 계정 이메일 주소와 서비스 Appkey 또는 Container Registry 서비스가 활성화된 프로젝트의 통합 Appkey입니다.

```
$ docker login {사용자 레지스트리 주소}
Username: {TOAST 사용자 계정 이메일 주소}
Password: {Appkey}
Login Succeeded
```

> [참고]
> docker login 명령으로 얻어지는 토큰의 최대 유효 시간은 12시간 입니다.

### 태그 생성

컨테이너 이미지를 사용자 레지스트리에 저장하려면 사용자 레지스트리 주소를 포함한 리포지토리(repository) 이름과 태그(tag)가 필요합니다. Docker CLI 도구의 **tag** 명령을 이용해 만들 수 있습니다.

```
$ docker tag {리포지토리 이름}:{태그} {사용자 레지스트리 주소}/{리포지토리 이름}:{태그}
```

* 예시
```
$ docker tag ubuntu:18.04 example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
```

> [참고]
> 컨테이너 이미지 이름([리포지토리 이름]:[태그 이름])으로는 영어 소문자, 숫자, 일부 특수문자(-, _, /, .)만 허용합니다. 리포지토리 이름은 레지스트리 주소를 포함해 최대 255자, 태그 이름은 최대 129자로 제한됩니다. 이미지 이름이 길면 사용하기 불편할 수 있습니다. 적당한 길이의 이름을 사용하시기 바랍니다.

### 컨테이너 이미지 저장(Push)
Docker CLI 도구의 **push** 명령을 사용해 컨테이너 이미지를 사용자 레지스트리에 저장할 수 있습니다.

```
$ docker push {사용자 레지스트리 주소}/{리포지토리 이름}:{태그 이름}
```

* 예시
```
$ docker push example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
The push refers to repository [example-kr1-registry.container.cloud.toast.com/ubuntu]
16542a8fc3be: Pushed
6597da2e2e52: Pushed
977183d4e999: Pushed
c8be1b8f4d60: Pushed
18.04: digest: sha256:e5dd9dbb37df5b731a6688fa49f4003359f6f126958c9c928f937bec69836320 size: 1152
```

### 컨테이너 이미지 조회
저장된 컨테이너 이미지는 웹 콘솔에서 조회할 수 있습니다.

* 리포지토리 목록
**Container > Container Registry** 서비스 페이지를 열면 컨테이너 이미지 리포지토리 목록을 볼 수 있습니다.

* 태그 목록
리포지토리 목록에서 원하는 리포지토리를 클릭하면 선택한 리포지토리에 저장된 태그 목록을 조회할 수 있습니다. 원하는 태그를 선택하면 태그의 상세 정보를 확인할 수 있습니다. 제공되는 태그 주소를 이용해 원하는 환경에 배포할 수 있습니다.

### 컨테이너 이미지 가져오기(Pull)
Docker CLI 도구의 **pull** 명령을 사용해 이미지를 가져올 수 있습니다. 웹 콘솔에서 가져올 이미지의 태그 주소를 확인해야 합니다.

```
$ docker pull {태그 주소}
```

* 예시
```
$ docker pull example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
18.04: Pulling from ubuntu
5bed26d33875: Pull complete
f11b29a9c730: Pull complete
930bda195c84: Pull complete
78bf9a5ad49e: Pull complete
Digest: sha256:e5dd9dbb37df5b731a6688fa49f4003359f6f126958c9c928f937bec69836320
Status: Downloaded newer image for example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
example-kr1-registry.container.cloud.toast.com/ubuntu:18.04

$ docker images
REPOSITORY                                              TAG     IMAGE ID        CREATED         SIZE
example-kr1-registry.container.cloud.toast.com/ubuntu   18.04   4e5021d210f6    12 days ago     64.2MB
```

### 컨테이너 이미지 태그 삭제
태그 목록에서 삭제할 태그를 선택하고 **태그 삭제** 버튼을 클릭해 삭제합니다.
