# minikube-helloword
virtualbox에 우분투 머신 생성 후 minikube kubectl hello-world 맛보기

# virtualbox 머신 설치
```
ubuntu-18.04.3-desktop-amd64.iso 설치

메모리 4089/ cpu 3개
```

# 도커설치
```
sudo apt update -y
```
Docker CE 의존성 패키지설치
```
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```
(우분투 /var/lib/dpkg/lock-frontend 잠금 파일을 얻을 수 없습니다) 오류 발생시
```
재부팅
```

도커 패키지 저장소를 apt에 등록
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```
apt 패키지 목록을 업데이트
```
sudo apt update -y
```
도커CE를 설치
```
sudo apt install -y docker-ce
```
도커를 시작
```
sudo systemctl start docker
```
도커 상태확인
```
sudo systemctl status docker
```
도커 버전확인
```
docker version
```

도커 오류시
(Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/json?all=1: dial unix /var/run/docker.sock: connect: permission denied)
```
sudo usermod -a -G docker $USER
sudo service docker restart
재부팅 or 로그아웃 후 로그인
```

# Minikube 설치
최신 버전을 설치
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube
```
Minikube 실행파일이 사용자 실행 경로에 추가
```
sudo mkdir -p /usr/local/bin/
sudo install minikube /usr/local/bin/
```
Minikube 실행
```
minikube start
```
상태 확인
```
minikube status
```

# Kubectl 설치
설치 진행
```
sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```
확인
```
kubectl get nodes
```

# 배포
hello-world pull
```
docker pull tutum/hello-world
```
yml 생성
```
vim hello-world.yml
```
hello-world.yml 작성
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
  labels:
    app: hello-world
spec:
  selector:
    matchLabels:
      app: hello-world
      tier: frontend
  template:
    metadata:
      labels:
        app: hello-world
        tier: frontend
    spec:
      containers:
        - image: tutum/hello-world
          name: hello-world
          ports:
            - containerPort: 80
              name: hello-world

---
apiVersion: v1
kind: Service
metadata:
  name: hello-world
  labels:
    app: hello-world
spec:
  type: NodePort
  ports:
    - port: 80
  selector:
    app: hello-world
    tier: frontend
```
배포확인
```
kubectl get all
```
로컬 ip 확인
```
minikube ip
```


# 출처
https://soyoung-new-challenge.tistory.com/52

http://www.kwangsiklee.com/2017/05/%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95-solving-docker-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket/

https://hanseokhyeon.tistory.com/entry/Ubuntu%EC%97%90-Minikube-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0

https://subicura.com/k8s/guide/#%E1%84%8B%E1%85%AF%E1%84%83%E1%85%B3%E1%84%91%E1%85%B3%E1%84%85%E1%85%A6%E1%84%89%E1%85%B3-%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9
