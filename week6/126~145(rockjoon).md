# Q.130

## 1. 문제 요구 사항 정리

- **현재 아키텍처:**
    - 애플리케이션이 **여러 가용 영역(AZ)에서 EC2 인스턴스로 실행**.
    - **EC2 Auto Scaling 그룹 + Application Load Balancer(ALB) 사용**.

- **목표:**
    - **CPU 사용률을 40% 근처로 유지하면서 최적의 성능 유지**.
    - **Auto Scaling 그룹을 적절하게 확장/축소해야 함**.

---

## 2. 주요 개념

### ✅ **Target Tracking Scaling Policy (목표 추적 확장 정책)**
- **CPU 사용률과 같은 지표를 기준으로 Auto Scaling 그룹을 동적으로 조정**.
- **목표 값(예: CPU 40%)을 유지하도록 자동 조정**.
- **AWS에서 권장하는 Auto Scaling 방식으로 운영 부담 최소화**.

### ✅ **Simple Scaling Policy (단순 확장 정책)**
- CPU 사용률이 특정 임계값을 넘으면 인스턴스를 추가하고, 일정 기준 이하가 되면 축소하는 방식.
- 하지만 **Target Tracking에 비해 반응 속도가 느리고 최적의 상태 유지가 어려움**.

### ✅ **Scheduled Scaling (예약 확장)**
- **트래픽 패턴이 명확할 때** 특정 시간에 Auto Scaling을 수행하는 방식.
- 트래픽이 일정하지 않은 경우 비효율적.

### ✅ **Lambda를 사용한 수동 업데이트**
- **Lambda를 사용하여 Auto Scaling 그룹 크기를 조정할 수도 있지만, 운영 부담이 증가**.
- **AWS 관리형 정책보다 비효율적**.

---

## 3. 해설

### **A. Simple Scaling Policy 사용 (오답)**
- **문제점:**
    - 단순 확장 정책은 **지표 기반으로만 작동하여 반응 속도가 느림**.
    - CPU 사용률을 **40% 근처에서 유지하기 어려움**.
- **결론:** ❌ **오답** (자동 최적화 부족)

---

### **B. Target Tracking Scaling Policy 사용 (정답)**
- **장점:**
    - **Auto Scaling 그룹이 CPU 사용률을 자동으로 40% 근처에서 유지**.
    - **AWS 관리형 방식으로 운영 부담 최소화**.
    - **지표 기반으로 빠르게 확장 및 축소 가능**.
- **결론:** ✅ **정답** (가장 효율적인 확장 방식)

---

### **C. Lambda를 사용하여 Auto Scaling 그룹 크기 업데이트 (오답)**
- **문제점:**
    - Lambda 함수로 Auto Scaling 크기를 변경하는 것은 **비효율적이며 운영 부담 증가**.
    - AWS의 **자동 확장 기능을 활용하는 것이 더 효율적**.
- **결론:** ❌ **오답** (운영 오버헤드 증가)

---

### **D. Scheduled Scaling 사용 (오답)**
- **문제점:**
    - 트래픽이 **예측 가능한 패턴을 따를 경우 유용하지만, 동적인 워크로드에는 적합하지 않음**.
    - CPU 사용률이 항상 40%로 유지되도록 보장할 수 없음.
- **결론:** ❌ **오답** (트래픽 변동에 실시간 대응 불가능)

---

## ✅ **최종 정답: B. Target Tracking Scaling Policy 사용**
- **CPU 사용률을 자동으로 40% 근처에서 유지**.
- **AWS 관리형 확장 정책으로 운영 부담 최소화**.
- **Auto Scaling 그룹을 실시간으로 최적화**.
- **AWS 모범 사례(Best Practice) 준수** ✅.  

---

# Q 131

## 1. 문제 요구 사항 정리

- **파일 저장:** Amazon S3 버킷 사용.
- **파일 제공:** Amazon CloudFront를 통해 제공.
- **보안 요구사항:**
  - **S3 URL을 통한 직접 접근 차단.**
  - **CloudFront를 통해서만 파일을 제공.**

---

## 2. 주요 개념

### ✅ **Origin Access Identity (OAI)**
- CloudFront가 **S3 버킷을 안전하게 접근할 수 있도록 하는 기능**.
- **OAI를 사용하면 S3 객체가 CloudFront를 통해서만 제공되도록 제한 가능**.
- **S3 버킷 정책을 사용하여 OAI만 접근할 수 있도록 설정**.

### ✅ **S3 Bucket Policy를 통한 보안 설정**
- S3 버킷 정책을 사용하여 **OAI를 통해서만 객체를 읽을 수 있도록 제한**.
- **S3 Public Access 차단 필요**.

---

## 3. 해설

### **A. 개별 S3 버킷 정책을 작성하여 CloudFront에만 접근 허용 (오답)**
- **문제점:**
  - CloudFront의 **OAI를 활용하지 않으면 S3 URL을 통한 직접 접근을 차단할 수 없음**.
  - **CloudFront 배포 ID를 S3 버킷 정책에서 직접 사용 불가능**.
- **결론:** ❌ **오답** (보안 요구사항 충족 불가)

---

### **B. IAM 사용자 생성 후 CloudFront에 할당 (오답)**
- **문제점:**
  - **IAM 사용자는 CloudFront에서 직접 사용할 수 없음**.
  - CloudFront가 S3 객체를 가져오려면 **OAI 또는 S3 버킷 정책 사용 필요**.
- **결론:** ❌ **오답** (IAM 사용자로 해결 불가)

---

### **C. S3 버킷 정책에서 CloudFront 배포 ID를 Principal로 설정 (오답)**
- **문제점:**
  - **CloudFront 배포 ID를 S3 버킷 정책에서 직접 Principal로 사용할 수 없음**.
  - **CloudFront OAI를 사용해야만 S3 객체에 대한 직접 접근 차단 가능**.
- **결론:** ❌ **오답** (CloudFront 배포 ID는 S3 버킷 정책에서 직접 사용 불가)

---

### **D. Origin Access Identity (OAI) 설정 후 CloudFront에 연결 (정답)**
- **해결 방법:**
  1. **CloudFront OAI 생성.**
  2. **CloudFront 배포 설정에서 S3 버킷을 원본(Origin)으로 지정하고 OAI를 할당.**
  3. **S3 버킷 정책을 수정하여 OAI만 읽기 권한을 부여.**
  4. **S3 Public Access 차단 활성화하여 직접 접근 방지.**
- **장점:**
  - **S3 URL을 통한 직접 접근 차단 가능.**
  - **CloudFront를 통한 안전한 배포 가능.**
- **결론:** ✅ **정답** (가장 보안성이 높은 해결책)

---

## ✅ **최종 정답: D. Origin Access Identity (OAI) 설정 후 CloudFront에 연결**
- **CloudFront를 통해서만 S3 파일 접근 가능하도록 설정**.
- **S3 Public Access 차단 + OAI를 통한 안전한 접근 보장**.
- **AWS 모범 사례(Best Practice) 준수** ✅.  


---

# Q 135

## 1. 문제 요구 사항 정리

- **시나리오:**
  - AWS에서 워크로드 실행 중인 회사가 **외부 제공자의 서비스**에 연결해야 함.
  - **해당 서비스는 외부 제공자의 VPC에 호스팅됨**.

- **보안 요구사항:**
  - **프라이빗 연결만 허용됨** (인터넷 경유 X).
  - **연결은 회사 VPC에서만 시작 가능 (단방향)**.
  - **해당 타겟 서비스에만 접근 가능하도록 제한**되어야 함.

---

## 2. 주요 개념

### ✅ **AWS PrivateLink**
- **VPC 간에 서비스에 대한 안전한 프라이빗 연결을 제공하는 기술**.
- **VPC endpoint (Interface 타입)**를 통해 **특정 서비스에만 연결 허용 가능**.
- **연결은 소비자(클라이언트) VPC에서 시작됨** → 보안 요건에 부합.
- **타겟 서비스 외에는 접근 불가** → 최소 권한 제어 가능.

### ❌ **VPC Peering**
- **VPC 간 완전한 네트워크 라우팅 허용** (모든 리소스 접근 가능).
- **세분화된 접근 제어 어려움** → 서비스 하나만 제한적으로 접근 불가.

### ❌ **NAT Gateway**
- **프라이빗 서브넷의 EC2 인스턴스가 인터넷에 나갈 수 있도록 해주는 게이트웨이**.
- **인터넷을 경유하므로 '프라이빗 연결' 요건 불충족**.

---

## 3. 해설

### **A. VPC Peering (오답)**
- **문제점:**
  - 전체 VPC 간 통신 허용됨 → 특정 서비스만 제한 불가.
  - 보안 요건(특정 서비스만 접근, 단방향 접근 제어) 충족 어려움.
- **결론:** ❌ **오답** (보안 요구사항 위배)

---

### **B. Provider가 Virtual Private Gateway 생성 + PrivateLink 사용 (오답)**
- **문제점:**
  - Virtual Private Gateway는 **VPN 또는 Direct Connect**와 관련된 구성.
  - **PrivateLink에는 관련 없음**.
- **결론:** ❌ **오답** (잘못된 서비스 구성)

---

### **C. NAT Gateway 사용 (오답)**
- **문제점:**
  - **인터넷을 통한 아웃바운드 트래픽 발생** → "프라이빗 연결" 조건 불충족.
- **결론:** ❌ **오답** (프라이빗 연결 아님)

---

### **D. Provider가 VPC Endpoint 서비스 생성 → 회사 측에서 PrivateLink 연결 (정답)**
- **장점:**
  - Provider가 AWS PrivateLink를 통해 **Endpoint Service 생성**.
  - 회사 VPC에서 **Interface VPC Endpoint**를 통해 **프라이빗으로 해당 서비스에만 연결**.
  - **연결은 회사 쪽에서만 시작되며**, **인터넷 사용 없이 보안적으로 안전함**.
- **결론:** ✅ **정답** (모든 요구사항 충족)

---

## ✅ 최종 정답: D. Ask the provider to create a VPC endpoint for the target service. Use AWS PrivateLink to connect to the target service.

- **프라이빗 연결 제공 (인터넷 사용 X)**
- **서비스 단위로 연결 제한 가능**
- **연결은 클라이언트 VPC에서 시작 → 보안 팀 요구사항 충족**
- **AWS 권장 아키텍처 (Best Practice)** ✅  

---

# Q 137

## 1. 문제 요구 사항 정리

- **현재 구조:**
  - **AWS Organizations** 사용 중 → 각 사업 부서별로 별도 AWS 계정 운영.
  - 각 계정의 **루트 사용자 이메일 주소**에 알림이 전달됨.

- **문제점:**
  - **루트 이메일 수신자가 알림을 놓침** → 중요한 알림 누락 가능.

- **요구사항:**
  - **모든 향후 알림이 계정 관리자에게 전달되어야 함.**
  - **알림 수신자는 계정 관리자 수준으로 제한되어야 함.**
  - **루트 사용자에게만 의존하지 않는 구조 필요.**

---

## 2. 주요 개념

### ✅ **AWS Account Alternate Contacts**
- **Billing, Operations, Security 알림을 수신할 담당자를 계정마다 지정 가능**.
- **루트 사용자와는 별개로 설정 가능**.
- **AWS Organizations 콘솔 또는 API를 통해 중앙 관리 가능**.
- **계정 관리자에게 직접 알림이 전달되도록 구성 가능**.

### ❌ **루트 이메일 공유 / 단일화**
- 보안 및 관리 측면에서 **루트 이메일을 여러 계정에 재사용하거나 공유하는 것은 비추천**.
- 루트 사용자 이메일은 보안적으로 민감하므로 가능한 최소한으로만 사용해야 함.

---

## 3. 해설

### **A. 루트 사용자 이메일을 모든 사용자에게 전달 설정 (오답)**
- **문제점:**
  - 알림이 모든 사용자에게 전달되어 **보안 및 권한 최소화 원칙 위반**.
  - **알림 수신 대상이 너무 광범위함**.
- **결론:** ❌ **오답** (알림 범위가 너무 넓음)

---

### **B. 루트 이메일을 소수 관리자용 DL로 구성 + Alternate Contacts 설정 (정답)**
- **장점:**
  - 루트 이메일 수신자는 소수의 관리자만 포함된 **메일링 리스트(DL)**로 구성 → **중요 알림 누락 방지**.
  - **Alternate Contacts**로 Billing, Security, Operations 관련 알림을 **직접 관리자에게 전달**하도록 설정 → 루트 이메일에 의존하지 않음.
- **결론:** ✅ **정답** (요구 사항 완벽히 충족 + AWS Best Practice)

---

### **C. 루트 이메일을 한 명의 관리자에게만 전달 (오답)**
- **문제점:**
  - **단일 담당자 의존** → 해당 인원이 부재 시 알림 누락 가능성 있음.
  - **확장성과 고가용성 측면에서 비효율적**.
- **결론:** ❌ **오답** (싱글 포인트 오브 페일러 발생 가능)

---

### **D. 모든 계정에 동일한 루트 이메일 사용 + Alternate Contacts 설정 (오답)**
- **문제점:**
  - **모든 계정에 동일한 루트 이메일은 보안상 매우 위험**.
  - 루트 사용자 공유는 **AWS 모범 사례에 위배됨**.
- **결론:** ❌ **오답** (보안 및 격리 측면에서 부적절)

---

## ✅ 최종 정답:
**B. Configure all AWS account root user email addresses as distribution lists that go to a few administrators who can respond to alerts. Configure AWS account alternate contacts in the AWS Organizations console or programmatically.**

- **루트 알림 누락 방지 (DL 사용)**
- **계정 관리자에게 직접 알림 전달 (Alternate Contacts)**
- **보안 및 운영 측면에서 모범 사례 준수** ✅


---

# Q 139


## 1. 문제 요구 사항 정리

- **현재 상황:**
  - 매일 여러 팀이 **Amazon S3 초기 버킷**에 파일 업로드.
  - 리포팅 팀이 **수동으로 분석용 S3 버킷**으로 복사.
  - **Amazon QuickSight**에서 분석 버킷의 파일 사용.

- **요구사항:**
  1. **파일이 초기 버킷에 업로드되면 자동으로 분석 버킷으로 이동.**
  2. **파일이 복사된 후 Lambda를 통해 패턴 매칭 실행.**
  3. **파일을 SageMaker Pipelines로 전달.**
  4. **운영 오버헤드 최소화.**

---

## 2. 주요 개념

### ✅ S3 Replication (버킷 간 복사 자동화)
- **S3 버킷 간 파일 복사를 자동화**하는 기능.
- **객체가 생성되면 대상 버킷에도 자동 복제됨.**
- **운영 오버헤드 매우 낮음.**

### ✅ S3 이벤트 알림 vs. EventBridge
- **S3 Event Notification**은 **단일 대상**만 지정 가능 (예: Lambda 하나).
- **Amazon EventBridge**는 **하나의 이벤트로 여러 서비스(Lambda, SageMaker 등)**를 **동시에 트리거** 가능.
- **S3 → EventBridge 연동은 분석용 버킷에서만 가능** (원본 버킷은 지원하지 않음).

---

## 3. 보기 해설

### **A. Lambda 함수로 직접 복사 + 분석 버킷 이벤트 알림 구성 (오답)**
- **문제점:**
  - 복사 과정을 직접 구현해야 하므로 **운영 오버헤드 증가**.
  - S3 이벤트로 여러 대상(Lambda + SageMaker)을 지정할 수 없음.
- **결론:** ❌ **오답**

---

### **B. Lambda 복사 + 분석 버킷에서 EventBridge 사용 (오답)**
- **문제점:**
  - 복사 로직이 여전히 수동(Lambda 복사) → 운영 부담 큼.
  - EventBridge 사용은 좋지만 복사 부분이 비효율적.
- **결론:** ❌ **오답**

---

### **C. S3 복제 + S3 이벤트 직접 사용 (오답)**
- **문제점:**
  - **S3 이벤트 알림은 여러 타겟(Lambda + SageMaker) 지정 불가**.
  - Lambda만 트리거하거나 다른 한 개의 대상만 지정 가능.
- **결론:** ❌ **오답**

---

### **D. S3 복제 + 분석용 S3 버킷에서 EventBridge 트리거 (정답)**
- **장점:**
  - **S3 복제**를 사용하여 초기 버킷 → 분석 버킷으로 자동 복사.
  - 분석용 S3 버킷에서 **EventBridge를 통해 Lambda + SageMaker Pipeline 둘 다 트리거 가능**.
  - **운영 오버헤드 최소 + 유연한 이벤트 처리 구조**.
- **결론:** ✅ **정답**

---

## ✅ 최종 정답: D
**Configure S3 replication between the S3 buckets. Configure the analysis S3 bucket to send event notifications to Amazon EventBridge (Amazon CloudWatch Events). Configure an ObjectCreated rule in EventBridge (CloudWatch Events). Configure Lambda and SageMaker Pipelines as targets for the rule.**

- **S3 복제로 복사 자동화**
- **분석용 버킷에서 EventBridge를 사용해 다중 타겟 트리거**
- **운영 오버헤드 최소화**
- **AWS 권장 아키텍처(Best Practice)** ✅

