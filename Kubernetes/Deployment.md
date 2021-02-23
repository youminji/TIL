## 디플로이먼트 (Deployment)

* 레플리카셋은 포드의 상위개념
* 디플로이먼트는 레플리카셋의 상위 개념
* 디플로이먼트 생성하면  하위개념인 레플리카셋, 포드 관리 가능



1. **디플로이먼트 정의**

deployment-nginx.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      name: my-nginx-pod
      labels:
        app: my-nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.10
          ports:
          - containerPort: 80

```

```
kubectl apply -f deployment-nginx.yaml
```



![image-20210223155705832](Deployment.assets/image-20210223155705832.png)





2. **디플로이먼트 삭제 => 레플리카셋, 포드도 함께 삭제되는 것을 확인**

```
kubectl delete deployment my-nginx-deployment
```

![image-20210223155957251](Deployment.assets/image-20210223155957251.png)



---

#### 디플로이먼트를 사용하는 이유

----

어플리케이션의 업데이트와 배포를 편하게 만들기 위해서 사용

버전업그레이드 하면 하나씩 알아서 바꿔줌





1. **--record 옵션을 추가해서 디플로이먼트를 생성**

```
kubectl apply -f deployment-nginx.yaml --record
```



![image-20210223162919181](Deployment.assets/image-20210223162919181.png)





2. **kubectl set image 명령으로 포드의 이미지를 변경**

```
kubectl set image deployment my-nginx-deployment nginx=nginx:1.11 --record
```



![image-20210223163004835](Deployment.assets/image-20210223163004835.png)

![image-20210223163049853](Deployment.assets/image-20210223163049853.png)

* 처음에 생성했던 레플리카셋과 나중에 생성한 레플리카셋



3. **리비전 정보 확인**

```
kubectl rollout history deployment my-nginx-deployment
```

![image-20210223163342683](Deployment.assets/image-20210223163342683.png)





4. **이전 비전의 레플리카셋으로 롤백**

```
kubectl rollout undo deployment my-nginx-deployment --to-revision=1
```

* undo : 바로 이전
* --to-revision: 특정 버전으로 이동시 사용

![image-20210223163449795](Deployment.assets/image-20210223163449795.png)

![image-20210223163504305](Deployment.assets/image-20210223163504305.png)

![image-20210223163640880](Deployment.assets/image-20210223163640880.png)

![image-20210223163734701](Deployment.assets/image-20210223163734701.png)



5. **디플로이먼트 상세 정보를 출력**

```
kubectl describe deployment my-nginx-deployment
```



![image-20210223164210504](Deployment.assets/image-20210223164210504.png)

![image-20210223164255969](Deployment.assets/image-20210223164255969.png)



6. **모든 리소스를 삭제**

```
kubectl delete deployment,replicaset,pod --all
```







---

#### 리비전을 여러개 했을 때

----

**여러개 버전의 디플로이먼트 생성**

```
kubectl apply -f deployment-nginx.yaml --record
```

```
kubectl set image deployment my-nginx-deployment nginx=nginx:1.11 --record
```

```
kubectl set image deployment my-nginx-deployment nginx=nginx:1.12 --record
```

```
kubectl set image deployment my-nginx-deployment nginx=nginx:1.13 --record
```

![image-20210224004815017](Deployment.assets/image-20210224004815017.png)

* 1.13 버전의 pod상태

```
kubectl rollout history deployment my-nginx-deployment
```

![image-20210224004932046](Deployment.assets/image-20210224004932046.png)



**이전상태로 돌리기**

```
kubectl rollout undo deployment my-nginx-deployment
```

![image-20210224005116884](Deployment.assets/image-20210224005116884.png)

![image-20210224005137037](Deployment.assets/image-20210224005137037.png)

* 기존의 파드 Terminating되고 1.12버전의 파드들이 생성되고있음

![image-20210224005204026](Deployment.assets/image-20210224005204026.png)

* undo: 바로 이전상태로



```
kubectl rollout history deployment my-nginx-deployment
```

![image-20210224005242382](Deployment.assets/image-20210224005242382.png)



 **특정 버전으로 돌리기**

```
kubectl rollout undo deployment my-nginx-deployment --to-revision=1
```

![image-20210224005742382](Deployment.assets/image-20210224005742382.png)

* --to-revision: 버전정보가 아니라 history에서 revision 정보



![image-20210224005946967](Deployment.assets/image-20210224005946967.png)

* pod, deployment, replicaset 확인





![image-20210224010138179](Deployment.assets/image-20210224010138179.png)

![image-20210224010158523](Deployment.assets/image-20210224010158523.png)

![image-20210224010230599](Deployment.assets/image-20210224010230599.png)

* 현재 디플로이먼트 상태 확인, 



![image-20210224010303290](Deployment.assets/image-20210224010303290.png)

![image-20210224010354196](Deployment.assets/image-20210224010354196.png)

![image-20210224010503833](Deployment.assets/image-20210224010503833.png)

* revision 바뀐거 확인

