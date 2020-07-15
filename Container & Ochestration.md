
### Docker  

- VM vs Docker  
    VM : OS 위에 OS가 또 올라감  
    Docker : OS 위에 격리만 함 
<br> 

- 도커의 특징
    - 1) 확장성
        - 도커가 설치되었으면 어디서든 컨테이너 실행 가능
        - 특정 회사/서비스에 종속적이지 않음
        - 개발 서버, 테스트 서버 쉽게 만들 수 있음
    - 2) 표준성
        - 배포 방식이 다 다르지만 run이라는 것 하나로 가능
    - 3) 이미지
    - 4) 설정
        - 환경 변수를 넣어서 관리
    - 5) 자원
        - 첨부 파일을 s3 같은 곳에 업로드
        - paas처럼 제한이 없지만 클라우드 이미지보다 관리 쉬움
        - 빌드 기록이 남음  
        
> Dockerfile 형식  

```docker
        FROM ubuntu:14.04
        MAINTAINER Foo Bar <foo@bar.com>  
    
        RUN apt-get update
        RUN apt-get install -y nginx
        RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
        RUN chown -R www-data:www-data /var/lib/nginx
        EXPOSE 8080   
    
        WORKDIR /etc/nginx  
    
        CMD ["nginx"]  
        COPY app.js
```

- FROM : 어떤 이미지를 기반으로 할지 설정  
- MAINTAINER : 메인테이너 정보  
- RUN : 쉘 스크립트 혹은 명령 실행  
이미지 생성 중에는 사용자 입력을 받을 수 있어서, apt-get install에서 -y 옵션을 꼭 넣어줘야 함. 안 넣으면 Fail  
- RUN은 한줄이 이미지 하나로 빌드됨  
- EXPOSE : 포트 노출  
- CMD : 컨테이너가 시작되었을 때 실행할 실행 파일 또는 쉘 스크립트  
- WORKDIR : CMD에서 설정한 실행 파일이 실행될 디렉터리  
- COPY : 말 그대로 복사  

출처[docker&k8s](https://zzsza.github.io/development/2018/04/17/docker-kubernetes/)

### Container Ochestration  
- Container Orchestration이 왜 필요한가?  
    적은 수의 컨테이너까지는 docker-compose 를 사용하여 관리 가능  
    하지만, 몇 백개의 컨테이너, 여러 호스트, 호스트 간의 서비스등을 고려해야됨에 따라 등장  
    <br>  
    
- 컨테이너 오케스트레이션의 등장
    - 여러 대의 서버와 여러 개의 서비스를 관리해주는 서비스
        - 스케줄링
            - 적당한 서버에 배포
        - 클러스터링
            - 여러 개의 서버를 하나의 서버처럼 사용
        - 서비스 디스커버리  
            - 클라이언트나 API 게이트웨이가 호출할 서비스를 찾는 매커니즘 
            - key value에 저장할 필요 없이 바로 가져올 수 있음
        - 로깅, 모니터링으로 중앙에서 관리 가능

- AWS ECS , Kubernetes 등이 존재 

###  쿠버네티스 인그레스란 ? K8s Ingress?  
- 인그레스 (Ingress) 란  
    일반적으로, 네트워크 트래픽은 Ingress와 egress 로 구분된다. Ingress는 외부로부터 서버 내부로 유입되는 네트워크 트래픽을, egress는 서버 내부에서 외부로 나가는 트래픽을 의미   

<br>  
- 쿠버네티스 인그레스 란  
    - 쿠버네티스의 Ingress는 외부에서 쿠버네티스 클러스터 내부로 들어오는 네트워크 요청 : 즉, Ingress 트래픽을 어떻게 처리할지 정의한다.  
    - Ingress는 외부에서 쿠버네티스에서 실행 중인 Deployment와 Service에 접근하기 위한, 일종의 관문 (Gateway) 같은 역할을 담당한다.  
    - Ingress를 사용하지 않았다고 가정했을 때, 외부 요청을 처리할 수 있는 선택지는 NodePort, ExternalIP 등이 있다.  
    - 그러나 이러한 방법들은 일반적으로 Layer 4 (TCP, UDP) 에서의 요청을 처리하며, 네트워크 요청에 대한 세부적인 처리 로직을 구현하기는 한계가 있다.  

![ingress](https://blogfiles.pstatic.net/MjAxOTA0MDFfMzEg/MDAxNTU0MDk0MzE3ODQw.bAWecmPjZ7FdiGK1BuTnSe7D9EaYK2TIJgDwB8ENczkg.MZJ_mFOHBM6RkOIOZMlYhH9uQ_xBQOfSNQRcq7OW14cg.PNG.alice_k106/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2019-04-01_%EC%98%A4%ED%9B%84_1.51.46.png?type=w2)  

출처 [쿠버네티스 인그레스](https://blog.naver.com/alice_k106/221502890249)
