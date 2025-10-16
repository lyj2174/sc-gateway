# SCG 프로젝트 생성
- https://start.spring.io/
<img width="1686" height="933" alt="Image" src="https://github.com/user-attachments/assets/3b1c6b67-df7c-4eb7-8d87-cbd2ce37d3df" />

<img width="1761" height="886" alt="Image" src="https://github.com/user-attachments/assets/d83fec59-751a-4f8b-8bc2-2ef7d69bb6e4" />

# 라우팅 설정
- 생성된 프로젝트에 application.yml 생성(기존 파일 이름 변경)
```yaml
server:
  port: 9000

spring:
  application:
    name: only-apigateway-service 
  
  cloud:
    gateway:
      routes:
        - id: hello_route # 새로 추가할 라우트의 ID
          uri: http://localhost:8765 # 타겟 서비스의 URI
          predicates:
            - Path=/hello # 매칭할 경로 (정확히 /hello)
```

# 프로젝트 RUN
```bash
./gradlew clean bootRun
.
.
.
The use of configuration keys that have been renamed was found in the environment:

Property source 'Config resource 'class path resource [application.yml]' via location 'optional:classpath:/'':
        Key: spring.cloud.gateway.routes[0].id
                Line: 11
                Replacement: spring.cloud.gateway.server.webflux.routes[0].id
        Key: spring.cloud.gateway.routes[0].uri
                Line: 12
                Replacement: spring.cloud.gateway.server.webflux.routes[0].uri
        Key: spring.cloud.gateway.routes[0].predicates[0]
                Line: 14
                Replacement: spring.cloud.gateway.server.webflux.routes[0].predicates[0]


Each configuration key has been temporarily mapped to its replacement for your convenience. To silence this warning, please update your configuration to use the new keys.
```

# Spring Cloud Gateway 테스트
```bash
curl http://localhost:9000/hello
{"message":"Hello, World!","timesptamp":1760515161856,"koreatime":"2025-10-15T16:59:21.856628293+09:00[Asia/Seoul]"}% 
```

# 전제 조건
- 아직 SCG 가 도커 컴포즈에 들어 있지 않으므로
- 아래와 같이 host 에서 8765 로 접근 가능  포트를 열어 주어야함
- compose/spring_lb/compose.yml
```bash
blog:
    # build: ../../docker_file/httpd
    image: datamario24/spring-rest-api:0.3.1
    ports:
      - "8765:8080"
```

# 공식 가이드
- Routing 에 대한 Predicate 나 방법들을 공식 문서
- https://docs.spring.io/spring-cloud-gateway/docs/current/reference/html/#gateway-request-predicates-factories


# 핵심 개념
```
- Spring Cloud Gateway(SCG) 핵심 개념

- Route : Client 의 요청을 '어디'로 보낼지 결정

- Predicate(조건) : 특정 조건에 따라 Routing 수행
Ex) Path, Cookie, Header, IP 등등

- Filter
Request/Response 에 대한 수정(Header 추가)
적절한 요청인지 검사 및 응답 거부
인증, 로깅 등과 같은 공통관심사 해결
```
<img width="1280" height="1736" alt="Image" src="https://github.com/user-attachments/assets/d3f773cd-74cf-4ac1-b35d-9fed087fc284" />
