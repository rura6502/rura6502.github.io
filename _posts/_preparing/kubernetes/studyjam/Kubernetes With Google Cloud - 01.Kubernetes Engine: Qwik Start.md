클러스터 : 하나 이상의 클러스터 마스터 머신과 노드라는 다수의 작업자 머신으로 구성됨.

노드 : 클러스터 구성을 위해 쿠퍼넷 프로세스를 실행하는 가상머신 인스턴스

> 컴퓨팅 영역(리전) 설정 : `gcloud config set compute/zone us-central1-a
`

> Kubernetes Engine 클러스터 생성 : `gcloud container clusters create [CLUSTER-NAME]`

> 쿠버넷 클러스터에서 특정 이미지를 실행, 태그를 지정하지 않을 경우 lastest를 가져옴
> `kubectl run hello-server --image=gcr.io/google-samples/hello-app:1.0 --port 8080`


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAyOTAwOTk0OCwtMTg3Mzg3NTY5MF19
-->