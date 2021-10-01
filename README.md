# eks-service-test
UWS AWS TF팀 내부 EKS 구성 테스트를 위한 자료입니다.
## AWS EKS Test

#### AKS 생성 및 접속

* AKS 생성(추가 할 것)

* AKS 접속

  ```
  aws configure
  
  aws sts get-caller-identity
  
  aws eks --region ap-northeast-2 update-kubeconfig --name cluster_name
  ```

* AKS 노드 생성

* kubectl get 을 통해 EKS 연결확인

  ```
  kubectl get pods --all-namespaces
  ```

* pod 생성해보기

  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: counter-pky
    labels:
      app: counter-pky
  spec:
    containers:
      - name: app-pky
        image: ghcr.io/subicura/counter:latest
        env:
          - name: REDIS_HOST
            value: "localhost"
      - name: db-pky
        image: redis
  ```

  ```
  apply -f pod.yaml
  ```

* pod 생성 확인

  ```
  kubectl get pods
  ```

* pod 별 log 확인

  ```
  kubectl logs conter-pky app-pky
  kubectl logs counter-pky app-pky
  ```

* pod 접속

  ```
  kubectl exec -it counter-pky -c app-pky -- sh
  ```

* curl 및 telnet 설치

  ```
  apk add curl busybox-extras
  ```

* 서비스 확인

  ```
  curl localhost:3000
  ```

* pod 삭제

  ```
  kubectl delete -f pod.yaml
  ```






- deployment 버전 히스토리 확인

  ```
  kubectl rollout history deploy/counter-deploy-pky
  ```

- deployment 버전 상세 히스토리 확인

  ```
  kubectl rollout history deploy/counter-deploy-pky --revision=2
  ```

- deployment roll back

  ```
  kubectl rollout undo deploy/counter-deploy-pky --to-revision=1
  ```

  

pod가 각 노드로 분산되지않음.

lb의 az c 영역이 추가되지않음.

하나이상의 Pod가 올라가지않음



노드를 큰걸로 바꿔서 다시 올려볼 것.

