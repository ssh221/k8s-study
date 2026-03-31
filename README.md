# 🐳 쿠버네티스 기초 개념 Quiz

### 문제 1. 워커 노드에서 Pod를 실행하는 컴포넌트

한 회사가 쿠버네티스 클러스터를 처음 구축하고 있습니다. 운영팀이 **kubectl**로 Pod 배포 명령을 실행했지만, 어떤 워커 노드에도 Pod가 스케줄링되지 않습니다. 조사 결과 워커 노드의 특정 컴포넌트가 동작하지 않고 있었습니다.

**Pod를 워커 노드에 실제로 실행시키는 역할을 담당하는 컴포넌트는 무엇입니까?**


1. kube-apiserver
2. etcd
3. kube-scheduler
4. kubelet

<details>
<summary><strong>정답 및 해설 보기</strong></summary>

#### **정답: 4번**

#### **풀이:**
**kubelet**은 각 워커 노드에서 실행되며, 마스터 노드로부터 Pod 배포 지시를 받아 실제로 컨테이너를 생성하고 관리하는 역할을 합니다.

- **kube-apiserver** → 마스터 노드의 API 진입점
- **etcd** → 클러스터 상태를 저장하는 분산 데이터베이스
- **kube-scheduler** → Pod를 어떤 노드에 배치할지 **결정**할 뿐, 직접 실행하지 않음

</details>

---

### 문제 2. 매니페스트의 핵심 구성 요소

개발팀이 Nginx 웹 서버를 쿠버네티스에 배포하려고 합니다. YAML 매니페스트 파일을 작성하여 Pod를 정의하려는데, 매니페스트에 반드시 포함해야 하는 핵심 정보가 **아닌 것**은 무엇입니까?

1. 사용할 컨테이너 이미지 이름 (예: nginx:latest)
2. Pod의 metadata (이름, 레이블 등)
3. apiVersion과 kind
4. Pod가 배포될 워커 노드의 IP 주소 

<details>
<summary><strong>정답 및 해설 보기</strong></summary>

#### **정답: 4번**

#### **풀이:**
쿠버네티스 매니페스트의 핵심 구성 요소는 다음 네 가지입니다:

```yaml
apiVersion:  # API 버전
kind:        # 리소스 종류 (Pod, Service 등)
metadata:    # 이름, 레이블 등
spec:        # 컨테이너 이미지, 포트 등 상세 스펙
```

Pod가 어떤 노드에 배치될지는 **kube-scheduler가 자동으로 결정**하므로, 워커 노드의 IP 주소를 매니페스트에 직접 지정할 필요가 없습니다.

</details>

---

### 문제 3. Pod 간 안정적인 통신

한 팀이 쿠버네티스 클러스터 내부에서 프론트엔드 Pod와 백엔드 Pod를 운영하고 있습니다. 프론트엔드 Pod가 백엔드 Pod에 접근해야 하는데, 백엔드 Pod가 재시작될 때마다 IP가 변경되어 통신이 끊기는 문제가 발생하고 있습니다.

**이 문제를 해결하기 위해 사용해야 하는 쿠버네티스 리소스는 무엇입니까?**


1. ConfigMap
2. Namespace
3. Service
4. PersistentVolume

<details>
<summary><strong>정답 및 해설 보기</strong></summary>

#### **정답: 3번**

#### **풀이:**
**Service**는 Pod 앞에 고정된 접근 포인트(IP와 DNS 이름)를 제공합니다. 백엔드 Pod의 IP가 변경되더라도 Service가 자동으로 정상 Pod에 트래픽을 전달해 주므로 통신이 끊기지 않습니다.

- **ConfigMap** → 설정 데이터 저장용
- **Namespace** → 리소스 격리용
- **PersistentVolume** → 데이터 영구 저장용

</details>

---

### 문제 4. 컨테이너 런타임의 역할

운영팀이 쿠버네티스 클러스터에서 컨테이너가 실행되지 않는 문제를 겪고 있습니다. 워커 노드에 접속하여 확인해보니 컨테이너 런타임(containerd)이 중지된 상태였습니다.

**컨테이너 런타임의 역할로 가장 올바른 설명은 무엇입니까?**


1. 클러스터의 모든 상태 정보를 저장하는 분산 데이터베이스이다.
2. 컨테이너 이미지를 가져오고 실제 컨테이너를 생성·실행·삭제하는 소프트웨어이다.
3. 외부 트래픽을 클러스터 내부의 Pod로 전달하는 로드밸런서이다.
4. Pod를 어떤 노드에 배치할지 결정하는 스케줄링 엔진이다.

<details>
<summary><strong>정답 및 해설 보기</strong></summary>

#### **정답: 2번**

#### **풀이:**
**컨테이너 런타임**(containerd, CRI-O 등)은 컨테이너 이미지를 풀(pull)하고 실제로 컨테이너를 생성·실행·삭제하는 역할을 합니다.

- A → **etcd**
- C → **Service** / **Ingress**
- D → **kube-scheduler**

</details>

---

### 문제 5. 마스터 노드(Control Plane)의 구성 요소

신입 엔지니어가 쿠버네티스 마스터 노드의 구성 요소를 학습하고 있습니다.

**다음 중 마스터 노드(Control Plane)의 구성 요소로만 올바르게 묶인 것은 무엇입니까?**


1. kubelet, kube-proxy, 컨테이너 런타임
2. kube-apiserver, kube-scheduler, kube-controller-manager, etcd
3. kube-apiserver, kubelet, etcd, kube-proxy
4. kube-scheduler, kube-proxy, 컨테이너 런타임, etcd

<details>
<summary><strong>정답 및 해설 보기</strong></summary>

#### **정답: 2번**

#### **풀이:**

**마스터 노드(Control Plane)** 의 핵심 구성 요소:

| 컴포넌트 | 역할 |
|----------|------|
| **kube-apiserver** | 모든 요청의 진입점 (API 서버) |
| **kube-scheduler** | Pod를 적절한 노드에 배치 |
| **kube-controller-manager** | 클러스터 상태를 원하는 상태로 유지 |
| **etcd** | 클러스터 상태 데이터 저장소 |

**워커 노드**의 구성 요소: **kubelet**, **kube-proxy**, **컨테이너 런타임**

</details>

<br>

---

# ✍️ 쿠버네티스 응용 개념 확인 Quiz


### [문제] 파드 배포 오류 분석 및 정보 확인

**상황 설명:** 클라우드 엔지니어인 당신은 **fisa-dev**라는 이름의 네임스페이스(Namespace)에 웹 서버 파드(Pod)를 배포하기 위해 YAML 파일을 작성하고 **kubectl apply -f web-server.yaml** 명령을 실행했습니다.

하지만 몇 분이 지나도 서비스가 정상 작동하지 않아 상태를 확인해 보니, 파드의 상태가 **ImagePullBackOff**로 표시되고 있습니다. 당신은 이 문제가 **이미지 이름 오타** 때문인지, 아니면 **컨테이너 런타임이 이미지를 찾지 못해서** 발생한 것인지 구체적인 원인을 파악하고자 합니다.

또한, 이 파드가 클러스터 내의 **어떤 노드(Node)에 스케줄링 되었는지**와 파드의 **IP 주소**도 함께 확인해야 합니다. 이때 가장 적절한 조치를 취할 수 있는 명령어는 무엇입니까?

1. kubectl logs web-server -n fisa-dev
2. kubectl get pod web-server -n fisa-dev
3. kubectl describe pod web-server -n fisa-dev
4. kubectl exec -it web-server -n fisa-dev -- /bin/bash


#### **📖 관련 개념 키워드**
- **Pod Lifecycle:** Pending → ContainerCreating → Running  
   - 오류 시 ImagePullBackOff, CrashLoopBackOff 등이 발생할 수 있음.
- **Control Plane:** Scheduler가 파드를 적절한 Node에 배치하는 과정을 이해해야 함.
- **Troubleshooting:** 일반적으로 get → describe → logs 순서로 진단을 진행함.



---

<details>
<summary><strong>정답 및 해설 보기</strong></summary>

#### **정답: 3번**

#### **풀이:**
1. **describe 명령어가 정답인 이유:**
   - **Events 섹션:** kubectl describe는 리소스의 상세 정보뿐만 아니라 하단의 **Events** 로그를 보여줍니다. **ImagePullBackOff** 발생 시 **"Failed to pull image..."** 같은 구체적인 에러 메시지를 확인할 수 있는 가장 확실한 방법입니다.
   - **인프라 정보:** 문제에서 요구한 **Node(어느 워커 노드에 배포됐는지)**, **IP(파드 주소)**, **Labels**, **Namespace** 정보를 모두 포함하고 있습니다.

2. **오답 분석:**
   - **1번 (logs)**: 컨테이너 내부 애플리케이션의 출력 로그를 봅니다. 지금처럼 이미지를 못 가져와서 컨테이너가 생성조차 되지 않은 단계(ImagePullBackOff)에서는 로그를 볼 수 없습니다.
   - **2번 (get)**: 파드의 상태(Status)만 요약해서 보여줍니다. 구체적인 에러 원인이나 노드 배치 정보까지는 나오지 않습니다. 물론 -o wide 옵션을 붙이면 IP와 노드는 보이지만, 에러 원인은 알 수 없습니다.
   - **4번 (exec)**: 실행 중인 컨테이너에 접속하는 명령입니다. 파드가 Running 상태가 아니므로 명령이 실패합니다.
</details>

<br>




