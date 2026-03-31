# ✍️ 쿠버네티스 핵심 개념 확인 퀴즈


### [문제] 파드 배포 오류 분석 및 정보 확인

**상황 설명:** 클라우드 엔지니어인 당신은 **fisa-dev**라는 이름의 네임스페이스(Namespace)에 웹 서버 파드(Pod)를 배포하기 위해 YAML 파일을 작성하고 **kubectl apply -f web-server.yaml** 명령을 실행했습니다.

하지만 몇 분이 지나도 서비스가 정상 작동하지 않아 상태를 확인해 보니, 파드의 상태가 **ImagePullBackOff**로 표시되고 있습니다. 당신은 이 문제가 **이미지 이름 오타** 때문인지, 아니면 **컨테이너 런타임이 이미지를 찾지 못해서** 발생한 것인지 구체적인 원인을 파악하고자 합니다.

또한, 이 파드가 클러스터 내의 **어떤 노드(Node)에 스케줄링 되었는지**와 파드의 **IP 주소**도 함께 확인해야 합니다. 이때 가장 적절한 조치를 취할 수 있는 명령어는 무엇입니까?

#### **📖 관련 개념 키워드**
- **Pod Lifecycle:** Pending → ContainerCreating → Running  
   - 오류 시 ImagePullBackOff, CrashLoopBackOff 등이 발생할 수 있음.
- **Control Plane:** Scheduler가 파드를 적절한 Node에 배치하는 과정을 이해해야 함.
- **Troubleshooting:** 일반적으로 get → describe → logs 순서로 진단을 진행함.

---

### [선택지]

1. kubectl logs web-server -n fisa-dev
2. kubectl get pod web-server -n fisa-dev
3. kubectl describe pod web-server -n fisa-dev
4. kubectl exec -it web-server -n fisa-dev -- /bin/bash

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




