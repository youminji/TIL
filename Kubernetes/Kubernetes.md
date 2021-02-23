# Kubernetes

**가상화란?**

* 호스트 머신의 남는 리소스를 100퍼 사용하기 위해 등장한 것이 가상화
* 하드웨어 자산을 파일로 관리한다.



**컨테이너의 가장 큰 장점**

* 크기가 작기 때문에 네트워크를 통해서 파일을 전달하는 것이 쉽다.



**쿠버네티스**

* 모든 리소스를 '오브젝트' 형태로 관리
* 쿠버네티스에서 사용할 수 있는 오브젝트롤 확인하는 명령어

```
kubectl api-resources
```

* shortname을 확인할 수 있다. => 오브젝트의 별칭

  `kubectl get nodes` = `kubectl get no`	

```
kubectl explain pods
```



**쿠버네티스 클러스터**

* 쿠버네티스 클러스터는 Control Plain과 application Plain이라는 두 부분으로 구성된다.
* Control Plain: 사용자가 클러스터와 상호 작용하고 커맨드 라인 툴인 kubectl을 사용해 커맨드 라인 내에서 클러스터가 할 작업을 지시하는 방식인 API가 있는 곳, 사용자, 클러스터, 외부 구성요소는 API를 통해 서로 통신할 수 있다.
* 



![image-20210222135753501](7주차_cloud_infra/9주차_k8s.assets/image-20210222135753501.png)

* 마스터와 워커노드로 구분된다.
* 마스터노드
  * 전체 쿠버네티스 시스템을 제어하고 관리하는 쿠버네티스 *컨트롤 플레인(Contorl plain)*을 실행
  * 컨트롤 플레인(Control Plain) 구성요소
    * kube-apiserver: 









