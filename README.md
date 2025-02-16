# springboot-docker-aws-build-study

## Prerequisites

- AWS EC2 인스턴스 (Ubuntu 20.04 기준)
- OpenJDK 17
- Docker 설치

## Step 1: Setting up the Linux Server

```bash
# 1. Root 권한 획득
sudo su -

# 2. 패키지 업데이트
sudo apt update

# 3. OpenJDK 17 설치 -y -> 자동 yes
sudo apt install openjdk-17-jdk -y
```

---

## Step 2: Cloning the Project

```bash
# Home 디렉토리로 이동 후 프로젝트 클론
cd ~
git clone <repository-url>
cd springboot-docker-aws-test
```

---

## Step 3: Building the Project

```bash
# Gradle Wrapper 실행
# Gradle Wrapper (gradlew) 스크립트를 실행할 때 사용하는 명령어
sh gradlew
# 프로젝트 build
sh gradlew build
```

- 빌드가 성공적으로 완료되면 `build/libs` 디렉토리에 `.jar` 파일이 생성된다. 확인해보자.

```bash
cd build/libs
ls
```

- **springboot-aws-0.0.1-SNAPSHOT.jar** 파일을 확인할 수 있다.
- 지금 바로 `java -jar springboot-aws-0.0.1-SNAPSHOT.jar` 명령어로 실행할 수 있지만, Docker 컨테이너에서 실행할 예정이므로 잠시 미루자.

---

## Step 4: AWS Security Group 설정

AWS 보안 그룹에서 **8080 포트**를 **인바운드 규칙**에 추가해 HTTP 요청이 허용되도록 설정해준다.

---

## Step 5: Running with Docker

1. **Docker 설치**
    
    ```bash
    # Ubuntu 기준 Docker 설치
    apt install docker.io -y
    apt install docker-buildx
    ```
    
2. **Docker 이미지 생성 - Dockerfile 기반**
    
    ```bash
    cd ~
    cd springboot-docker-aws-test
    docker build -t springboot-docker-aws-test .
    ```
    
3. **Docker 이미지 확인**
    
    ```bash
    docker images
    ```
    
4. **Docker 컨테이너 실행**
    
    ```bash
    docker run -d -p 8080:8080 springboot-docker-aws-test
    ```
    
5. **브라우저에서 확인**
    
    `http://<EC2-인스턴스-퍼블릭-IPv4>:8080`
    
    예시: `http://54.123.45.67:8080`
