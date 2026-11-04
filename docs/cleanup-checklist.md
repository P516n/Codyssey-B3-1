# 리소스 정리 체크리스트 (Resource Cleanup Checklist)

실습 완료 후 불필요한 클라우드 과금 및 리소스 방치를 방지하기 위해 생성한 모든 인프라 자원을 삭제·정리하고 검증한 내역입니다.

---

## 📌 필수 정리 대상 5종 확인표

| 번호 | 리소스 구분 (AWS ↔ OCI) | 리소스 명칭 / ID | 정리 작업 내용 | 정리 상태 | 확인 일시 |
| :---: | :--- | :--- | :--- | :---: | :---: |
| **1** | **EC2 (Compute Instance)** | `codyssey-web-server` | 인스턴스 종료(Terminate) 수행 | [x] 완료 (Terminated) | 2026-10-15 |
| **2** | **EBS (Boot Volume)** | `codyssey-web-server (Boot Volume)` | 인스턴스 종료 시 영구 삭제 옵션 체크로 볼륨 함께 삭제 | [x] 완료 (Terminated) | 2026-10-15 |
| **3** | **Elastic IP (Public IP)** | Ephemeral / Reserved IP | 공용 IP 자동 회수 및 예약 IP 해제(Release) | [x] 완료 (Released) | 2026-10-15 |
| **4** | **Internet Gateway (IGW)** | `codyssey-igw` | VCN 상세에서 IGW 삭제 | [x] 완료 (Deleted) | 2026-10-15 |
| **5** | **VPC (VCN & Subnets)** | `codyssey-vcn`<br>- `codyssey-public-subnet`<br>- Route Table / Security List | 서브넷 삭제 후 VCN 최종 삭제 완료 | [x] 완료 (Deleted) | 2026-10-15 |

---

## 🔍 상세 정리 절차 및 주의사항 검증

### 1. Compute Instance & Boot Volume 정리
- [x] OCI 콘솔 **Compute** → **Instances** 이동
- [x] `codyssey-web-server` 인스턴스 선택 후 **Terminate** 클릭
- [x] **⚠️ "Permanently delete the attached boot volume"** 체크박스를 반드시 체크하여 미사용 부트 볼륨이 독립적으로 남아 과금되는 것을 방지함
- [x] 인스턴스 상태가 `Terminated`로 변경되고, **Storage** → **Boot Volumes** 목록에서 해당 볼륨이 사라진 것을 최종 확인

### 2. Public IP 정리
- [x] **Networking** → **IP Management** → **Reserved Public IPs** 메뉴 확인
- [x] 실습 중 별도로 할당/예약된 Public IP가 남아있지 않은지 확인 (미사용 예약 IP는 과금 대상이므로 모두 해제)

### 3. VCN 서브넷 및 게이트웨이 정리
- [x] **Networking** → **Virtual Cloud Networks** → `codyssey-vcn` 이동
- [x] **Subnets** 목록에서 `codyssey-public-subnet` 삭제 (인스턴스가 완전히 종료된 후 삭제 가능)
- [x] **Internet Gateways** 목록에서 `codyssey-igw` 삭제
- [x] Default Route Table 및 Default Security List에 추가했던 사용자 정의 규칙(0.0.0.0/0 라우트, 80번 포트 인바운드 규칙 등) 삭제

### 4. VCN 최종 삭제
- [x] 연관된 서브넷 및 게이트웨이가 모두 제거된 후, `codyssey-vcn` 선택하여 **Terminate / Delete** 완료

---

## 💰 과금 대시보드(Billing) 최종 확인

- [x] **Billing & Cost Management** → **Cost Analysis** 확인
- [x] 잔여 유료 리소스(Running Instance, Block Storage, NAT Gateway, Load Balancer 등)가 존재하지 않음을 확인
- [x] 실습 전용 구획(`codyssey-compartment`) 내 리소스 탐색기(Resource Explorer) 조회 결과 0건 확인

---

## 📸 리소스 정리 증빙 스크린샷 (선택)

> 실습 종료 후 콘솔 화면 캡처 이미지를 `docs/` 폴더에 추가하고 아래에 연결합니다.

![인스턴스 종료 및 VCN 삭제 확인](./images/cleanup-proof.png)
*(인스턴스 Terminated 상태 및 볼륨 삭제 화면 또는 Billing 대시보드 스크린샷)*
