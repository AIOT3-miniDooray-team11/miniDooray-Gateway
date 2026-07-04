# miniDooray Gateway

Spring Cloud Gateway 기반의 API Gateway 서비스입니다. miniDooray 마이크로서비스들의 단일 진입점 역할을 하며, 요청 라우팅과 글로벌 로깅을 담당합니다.

## 기술 스택

| 항목 | 내용 |
|------|------|
| Language | Java 21 |
| Framework | Spring Boot 4.0.6 |
| Gateway | Spring Cloud Gateway (WebFlux) |
| Spring Cloud | 2025.1.1 |
| Build Tool | Maven |

## 주요 기능

### 라우팅 (RouterConfig)

| 경로 패턴 | 대상 서비스 | 포트 |
|-----------|------------|------|
| `/account-api/**` | Account API | 8081 |
| `/task-api/**` | Task API | 8082 |

### 글로벌 로깅 (LoggingConfig)

모든 요청/응답에 대해 글로벌 필터를 통해 자동으로 로그를 기록합니다.

- **요청 로그**: HTTP 메서드, URI, 클라이언트 IP
- **응답 로그**: HTTP 상태 코드

```
[Gateway] GET http://localhost:8000/account-api/users from /127.0.0.1:54321
[Gateway] Response: 200 OK
```

## 프로젝트 구조

```
src/main/java/com/nhnacademy/minidooraygateway/
├── MiniDoorayGatewayApplication.java   # 애플리케이션 진입점
└── config/
    ├── RouterConfig.java               # 라우팅 규칙 정의
    └── LoggingConfig.java              # 글로벌 로깅 필터
```

## 설정

`src/main/resources/application.yaml`

```yaml
spring:
  application:
    name: miniDooray-Gateway
  cloud:
    gateway:
      server:
        webflux:
          x-forwarded:
            enabled: true   # X-Forwarded-* 헤더 전달 활성화
server:
  port: 8000
```

## 실행 방법

```bash
# Maven Wrapper 사용
./mvnw spring-boot:run

# 또는 JAR 직접 실행
./mvnw clean package
java -jar target/miniDooray-Gateway-0.0.1-SNAPSHOT.jar
```

Gateway는 **8000번 포트**에서 실행되며, 하위 서비스들이 각각의 포트(8081, 8082)에서 실행 중이어야 라우팅이 정상 동작합니다.

## 아키텍처

```
Client
  │
  ▼
Gateway (8000)
  ├── /account-api/** ──► Account API (8081)
  └── /task-api/**   ──► Task API    (8082)
```
