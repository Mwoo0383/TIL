# 📌 AWS 학습하면서 배포하기

## 터미널에서 EC2 인스턴스에 접속하기
- ```ssh -i ~/.ssh/Room91.pem ubuntu@3.39.127.143```  
>```ssh``` -> Secure Shell. 원격 접속 명령어  
```-i``` -> "identity file" 옵션. 비공개 키 파일 Room91.pem 지정  
```ubuntu``` -> 원격 서버의 사용자 이름  
```@3.39.127.143``` -> 접속하려는 서버의 퍼블릭 IP 주소

- ```chmod 400 ~/파일``` : 해당 파일을 오직 소유자만 읽을 수 있게 만들어주는 권한

## 인스턴스 접속 후 패키지 다운 받기

- ```sudo su``` : 현재 사용자를 루트 사용자로 전환하는 명령어

- ```apt-get update``` : 시스탬 패키지 목록을 최신화 하는 명령어

- ```sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common```  
> ```apt-transport-https``` : apt가 HTTPS를 통해 안전하게 외부 저장소에서 패키지 받아올 수 있게 해줌  
```ca-certificates``` -> HTTPS 인증서(SSL)를 검증하는 데 필요한 신뢰된 인증기관(CA) 목록  
```curl``` -> 웹에서 파일이나 API를 요청할 수 있는 명령줄 툴  
```software-properties-common```-> ```add-apt-repository``` 명령어 등 레포지토리 관리 도구 포함. 외부 저장소 등록할 때 필수

## Ubuntu 리눅스 서버에 Docker 설치

- ☹️ 이전 방식 ```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -```  
: **Docker 저장소에서 제공하는 공식 GPG 키를 시스템에 등록**  
😆 최신 방식 ```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg```

  > 보안 강화용!  
이전 방식(apt-key add) : 시스템 전체에 키를 등록  
최신 방식 : 해당 저장소에만 키를 국한해서 지정할 수 있어서 더 안전

- ☹️ 이전 방식 ```add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"```  
: **Docker 저장소 등록**  
😆 최신 방식 ```echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] ..." | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null```  

  > 보안 강화 -> echo | tee 방식  

    이후 ```sudo apt-get update``` 명령어 사용으로 Docker 설치