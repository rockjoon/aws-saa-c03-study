## Q.26

### 요구사항

- AWS 클라우드 배포를 검토하여 S3 버킷에 무단 구성 변경이 없는지 확인

---

### 정답

✅ **A. AWS Config**

### 설명

- **AWS Config**: AWS 리소스의 설정을 관리, 기록 및 평가하는 서비스
  - 감사 및 리소스 변경 사항 확인 용도로 사용됨
  - 리소스 설정 변경을 기록하며, 규칙을 준수하지 않으면 "미준 (Noncompliant)"으로 기록됨

---

### 오답

❌ **B. AWS Trusted Advisor**

- AWS 계정의 비용, 성능, 보안 개선 사항을 분석하고 제안하지만, 리소스 설정 변경 추적 기능은 없음

❌ **C. Amazon Inspector**

- EC2, ECS, Lambda 등의 보안 상태를 평가하는 서비스로, S3 버킷 설정 변경을 감시하는 기능이 없음

❌ **D. Amazon EventBridge (Amazon CloudWatch)**

- 리소스 상태 변화 감지 및 특정 이벤트 실행 가능하지만, 구성 변경을 지속적으로 모니터링하는 기능은 부족함

---

## Q.27

### 요구사항

- Amazon CloudWatch 대시보드에 애플리케이션 지표 표시
- 제품 관리자에게 해당 대시보드에 액세스할 수 있는 계정 제공

---

### 정답

✅ **A. CloudWatch 대시보드 공유 기능**

### 설명

- **CloudWatch**: AWS 리소스 및 애플리케이션을 실시간 모니터링하는 서비스
- CloudWatch 대시보드는 AWS 계정이 없는 사용자와도 공유 가능
  - 최대 5개의 이메일 주소 지정하여 공유 가능
  - 링크를 통해 공개적으로 공유 가능
  - Amazon Cognito와 연계하여 SSO 방식으로 공유 가능

---

### 오답

❌ **B. CloudWatch ReadOnlyAccess 정책**

- 읽기 액세스를 부여하지만, 대시보드뿐만 아니라 로그 및 인사이트 등 다른 읽기 권한도 포함하여 최소 권한 원칙을 위배함

---

## Q.28

### 요구사항

- AWS Organizations를 사용하여 중앙에서 계정 관리
- 모든 계정에 SSO 솔루션 필요
- 사내 Microsoft Active Directory에서 사용자 및 그룹을 계속 관리

### 정답

✅ **B. AWS Single Sign-On (SSO) + Active Directory 연동**

### 설명

- **AWS SSO**: 여러 AWS 계정 및 애플리케이션에 대한 단일 로그인 제공
- **Active Directory 연동**: 온프레미스 AD와 AWS 계정을 연결하여 기존 사용자 및 그룹 관리 가능

---

### 오답

❌ **A**

- SSO는 양방향 포리스트 트러스트가 필요함.

❌ **C**

- SSO를 언급하지 않음.

❌ **D**

- 문제에서는 회사가 온프레미스 방식이 아닌 기존 AD 를 계속 사용하려고 한다고 설명함.

---

## Q.29

### 요구사항

- 회사는 UDP 연결을 사용하는 VoIP(Voice over Internet Protocol) 서비스를 제공
- 지연 시간이 가장 짧은 리전으로 사용자를 라우팅
- 지역 간 자동 장애 조치 필요

---

### 개념

- UDP 연결을 사용하는 VolP 서비스 : UDP(사용자 데이터그램 프로토콜)는 연결 지향적이지 않고, 빠르고 효율적인 데이터 전송을 제공하는 프로토콜입니다. VoIP(Voice over Internet Protocol, 인터넷 전화) 서비스에서 UDP를 사용하는 이유는 주로 속도와 지연 시간에 민감한 음성 통화를 최적화하기 위해서임.
- AWS Global Accelerator : AWS의 글로벌 네트워크 인프라를 통해 사용자 트래픽을 전송하여 인터넷 사용자 성능을 최대 60% 개선하는 네트워킹 서비스
- NLB vs ALB : ALB는 주로 HTTP/HTTPS 트래픽을 처리하는 데 사용, NLB는 TCP/UDP 트래픽 처리
- Auto Scaling : 클라우드의 유연성을 돋보이게 하는 핵심기술로 CPU, 메모리, 디스크, 네트워크 트래픽과 같은 시스템 자원들의 메트릭(Metric) 값을 모니터링하여 서버 사이즈를 자동으로 조절 하는 서비스
- Amazon Route 53 지연 시간 레코드 : 지연 시간이 가장 짧은, 즉 가장 가까운 리소스로 리다이렉팅을 하는 정책
- Amazon Route 53 가중치 레코드 : 다수의 리소스를 단일 도메인 이름(example.com) 또는 하위 도메인 이름(acme.example.com)과 연결하고 각 리소스로 라우팅되는 트래픽 비율을 선택할 수 있음.

---

### 정답

✅ **A. AWS Global Accelerator + NLB**

### 설명

- **AWS Global Accelerator**: 글로벌 네트워크 경로 최적화를 통해 지연 시간 감소
- **NLB (Network Load Balancer)**: UDP 기반 트래픽을 지원하며 VoIP 서비스에 적합

---

### 오답

❌ **C**

- Route 53 대기 시간 기반 라우팅은 도움이되지만 Global Accelerator와 같은 완벽한 자동 장애 조치를 제공하지는 않음.

❌ **D**

- ALB는 UDP 트래픽에 적합하지 않으며, 가중 라우팅은 지연 시간이 가장 짧지 않음.

---

## Q.30

### 요구사항

- Amazon RDS에서 매월 48시간 동안 리소스 집약적 테스트 실행
- 비용 절감을 원함

---

### 개념

- Amazon RDS : 관계형 데이터베이스 서비스스
- 스냅샷 : 데이터 전체를 복사하는 것이 아니라 특정 시점의 데이터 이미지를 생성하여 저장하는 방식

---

### 정답

✅ **C. RDS 스냅샷 활용**

### 설명

- **RDS 스냅샷**: 특정 시점의 데이터를 저장하여 필요할 때 복원 가능
- 스냅샷 보관 시 비용이 절감됨

---

### 오답

❌ **A**

- DB 인스턴스를 중지해도 DB 인스턴스가 돌아가는 EBS 볼륨이나 이런 건 사용하지
  않아도 보유 중인 용량에 따라 요금이 부과됨.

❌ **B**

- Auto Scaling 을 사용하게 되면 사용하지 않을 때에도 인스턴스가 실행 상태가 되므로
  스냅샷 보관보다 비용이 더 부과됨

❌ **D**

- 사용중이 아닐 때도도 인스턴스가 실행 상태이므로 스냅샷 보관보다 비용이 더 부과됨

---

## Q.31

### 요구사항

- Amazon EC2, RDS, Redshift에 특정 태그가 설정되었는지 자동으로 확인
- 운영 노력 최소화

### 정답

✅ **A. AWS Config**

### 설명

- AWS Config는 리소스의 태그 설정 여부를 자동으로 확인하고 평가 가능

---

## Q.32

### 요구사항

- 정적 웹사이트 호스팅
- 비용 효율적 방법 필요

---

### 정답

✅ **B. Amazon S3**

### 설명

- 정적 웹사이트는 서버가 필요 없으며, S3 버킷에 직접 배포 가능
- React 정적 번들도 S3에 배포 가능

---

## Q.33

### 요구사항

- 실시간으로 금융 거래 데이터를 여러 내부 애플리케이션과 공유
- 데이터베이스 저장 전에 민감한 정보 제거 필요

---

### 개념

- 트랜잭션 데이터 : 시간과 함께 생성되는 데이터를 기록한 것
- Amazon DynamoDB : nosql 데이터베이스
- Amazon Kinesis Data Firehose : 주목적은 미리 정의된 목적지(Destination)에 데이터를 안전하게 전달(Deliver)하는 것입니다. 여기서 목적지라 함은 S3 bucket, ElasticSearch, Amazon Redshift 등이 data lake의 역할을 할 수 있는 다양한 저장소를 말함.
- Amazon Kinesis Data Streams : 실시간으로 data 들을 받아들일 수 있는 입구이자 저장소로서의 역할을 함.

---

### 정답

✅ **C. Amazon Kinesis Data Streams**

### 설명

- 실시간 데이터 처리를 위한 스트리밍 서비스
- 금융 거래 데이터 처리에 적합
- Amazon Kinesis Data Firehose vs Streams : Streams은 저지연 스트리밍 서비스이고, Firehose는 데이터 전송 서비스. (관련 문서 : https://jaeyeong951.medium.com/aws-kinesis-data-stream-vs-data-firehose-10102d949741)

---

## Q.34

### 요구사항

- AWS 리소스의 구성 변경 사항 추적 및 API 호출 기록 유지

---

### 정답

✅ **B. AWS CloudTrail**

### 설명

- 모든 API 호출 및 변경 사항을 기록하는 서비스

---

## Q.35

### 요구사항

- ELB > VPC > EC2 구성
- DNS는 타사 서비스 사용
- 대규모 DDoS 공격 탐지 및 보호 필요

---

### 개념

- Amazon GuardDuty : AWS 계정 및 워크로드에서 악의적 활동을 모니터링하고 상세한 보안 결과를 제공하여 가시성 및 해결을 촉진하는 위협 탐지 서비스입니다.
- Amazon Inspector : 지속적으로 스캔하는 취약성 관리 서비스입니다.
- AWS Shield Advanced : 정교한 대규모 DDoS 공격에 대한 추가 보호 및 완화, 실시간에 가까운 공격에 대한 가시성, 웹 애플리케이션 방화벽 AWS WAF 와의 통합을 제공
- DDoS 공격(Distributed Denial of Service)은 네트워크 서비스를 중단시켜 웹 사이트나 서버를 공격하는 사이버 공격

---

### 정답

✅ **D. AWS Shield Advanced**

### 설명

- **AWS Shield Advanced**: 정교한 DDoS 공격을 방어하는 서비스
- 실시간 보호 및 AWS WAF와 연계 가능

---

### 오답

❌ **C**

- AWS Shield Advanced 와 AWS Shield standard는 지원하는 수준에 차이가 있음.
  - standard는 무료 서비스이며, Layer 3/4 공격으로 부터 보호해준다.
  - Advanced는 유료 서비스이며, 여러 가지 지원이 있지만 24 X 7(연중무휴)로 DRT(DDoS Response Team) 지원하여 DDos 공격 방어에 더 적합하다.
