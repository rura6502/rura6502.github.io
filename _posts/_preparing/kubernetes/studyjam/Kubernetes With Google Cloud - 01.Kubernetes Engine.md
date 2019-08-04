클러스터 : 하나 이상의 클러스터 마스터 머신과 노드라는 다수의 작업자 머신으로 구성됨.

노드 : 클러스터 구성을 위해 쿠퍼넷 프로세스를 실행하는 가상머신 인스턴스

> 컴퓨팅 영역(리전) 설정 : `gcloud config set compute/zone us-central1-a
`

> Kubernetes Engine 클러스터 생성 : `gcloud container clusters create [CLUSTER-NAME]`

> 쿠버넷 클러스터에서 특정 이미지를 실행, 태그를 지정하지 않을 경우 lastest를 가져옴
> `kubectl run hello-server --image=gcr.io/google-samples/hello-app:1.0 --port 8080`
> --port : 컨테이너가 노출할 포트를 지정

> 쿠버넷 서비스 생성
> `kubectl expose deployment hello-server --type="LoadBalancer"`
> --type="LoadBalancer" : 컨테이너의 컴퓨팅 엔진 부하 분산기를 생성

> 서비스 확인
> `kubectl get service hello-server`

> 서비스 삭제
> `gcloud container clusters delete [CLUSTER-NAME]`

## 팟(Pods)

1개 이상의 컨테이너가 포함된 모음. 팟은 볼륨을 포함하고 있음. 볼륨은 팟과 같이 존재하는 데이터 디스크. 컨테이너에 의해 사용될 수 있음. 팟은 컨텐츠에 공유된 네임스페이스를 제공. 같은 팟에 있는 컨테이너는 서로 통신할 수 있고 볼륨을 공유함. 네트워크 네임스페이스도 공유함. 팟은 IP 주소를 한개씩 가지고 있음

### 팟 만들기

팟은 팟 구성 파일을 사용하여 만들 수 있음. 아래의 팟 구성 파일로 알 수 있는 것

* 1개의 컨테이너로 구성되어 있음.
* args로 몇가지 인수를 전달함
* http 용 80번 포트를 개방함.

```bash
cat pods/monolith.yaml
apiVersion: v1
kind: Pod
metadata:
  name: monolith
  labels:
    app: monolith
spec:
  containers:
    - name: monolith
      image: kelseyhightower/monolith:1.0.0
      args:
        - "-http=0.0.0.0:80"
        - "-health=0.0.0.0:81"
        - "-secret=secret"
      ports:
        - name: http
          containerPort: 80
        - name: health
          containerPort: 81
      resources:
        limits:
          cpu: 0.2
```

팟을 생성하는 명령어

```bash
kubectl create -f pods/monolith.yaml
```

실행중인 팟 확인

```
kubectl get pods

kubectl describe pods monolith
```
팟에는 기본적으로 클러스터 밖에서 접근할 수 없는 IP주소가 부여됨. 



<!--stackedit_data:
eyJoaXN0b3J5IjpbODc1NzQ0Nzg3LDg3ODY1NDQ3Ml19
-->