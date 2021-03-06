# minikube-helloword
virtualbox minikube hello-world 맛보기

# virtualbox 머신 설치
```
ubuntu-18.04.3-desktop-amd64.iso 설치

메모리 4089/ cpu 3개
```

# 도커설치
```
$ sudo apt-get update
```
Docker CE 의존성 패키지설치
```
$ sudo apt-get install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg-agent \
  software-properties-common
```
도커의 공식 GPG 키를 추가
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
```
레포지터리를 추가
```
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" 
```
패키지 목록을 업데이트
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
정상적으로 도커가 설치확인
```
$ sudo docker run hello-world
```

#출처
https://jcil.co.kr/10
