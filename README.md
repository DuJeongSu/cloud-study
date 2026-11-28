# Cloud Infrastructure Study

## 1. 소개

클라우드 기술과 인프라에 관심을 가지고 공부한 내용을 정리하는 개인 학습 저장소입니다.

클라우드 서비스가 어떤 구조로 동작하는지 이해하고, 직접 환경을 구성하고 실습하면서 배운 내용을 기록하고 있습니다.

Linux와 네트워크 기초부터 시작해 AWS, 인프라 자동화, 가상화, OpenStack, 컨테이너 등 클라우드 인프라와 관련된 기술을 단계적으로 학습합니다.


## 2. 학습 계획

**참고:** 아래 학습 계획은 본격적인 학습과 실습을 시작하기 전, 사전 조사와 검토를 바탕으로 작성한 초기 계획입니다. 
          실제 학습 과정에서 이해도, 실습 결과, 환경 및 기술 변화에 따라 학습 순서와 기간, 세부 내용은 변경될 수 있습니다.

| 구분 | 기간 | 학습 내용 |
|---|---|---|
| Day 0 | 환경 설정 | 개발 환경 및 GitHub 구성 |
| Stage 1 | Day 1~20 | Linux 기초 및 시스템 관리 |
| Stage 2 | Day 21~40 | 네트워크 기초 및 동작 원리 |
| Stage 3 | Day 41~60 | AWS 클라우드 서비스 |
| Stage 4 | Day 61~80 | Terraform 및 Ansible |
| Stage 5 | Day 81~110 | 가상화 및 OpenStack |
| Stage 6 | Day 111~120 | Docker 및 Kubernetes |
| Stage 7 | Day 121~140 | 인프라 통합 프로젝트 |
| Stage 8 | Day 141~160 | 학습 내용 정리 및 프로젝트 마무리 |

## 3. 실습 환경

### Windows 데스크톱
- CPU: AMD Ryzen 7 7700X
- RAM: 64GB
- Linux 환경: WSL2 Ubuntu 24.04 LTS

### MacBook Pro
- CPU: Apple M4 Pro
- OS: macOS
- 터미널: Ghostty

Windows와 macOS 환경을 함께 활용하며 실습하고, GitHub를 통해 학습 내용을 관리합니다.

## 4. 학습 기술

- **운영체제:** Linux, Ubuntu
- **스크립트:** Bash, Python
- **네트워크:** TCP/IP, Wireshark
- **클라우드:** AWS
- **인프라 자동화:** Terraform, Ansible
- **가상화:** KVM, QEMU, Libvirt
- **프라이빗 클라우드:** OpenStack
- **컨테이너:** Docker, Kubernetes
- **버전 관리:** Git, GitHub

## 5. 저장소 구성

| 디렉터리 | 내용 |
|---|---|
| `stage1-linux/` | Linux 명령어, 시스템 관리 및 트러블슈팅 |
| `stage2-network/` | 네트워크 기초, 프로토콜 및 패킷 분석 |
| `stage3-aws/` | AWS 서비스 및 클라우드 인프라 실습 |
| `stage4-iac/` | Terraform 및 Ansible을 활용한 자동화 |
| `stage5-openstack/` | 가상화 환경 및 OpenStack 실습 |
| `stage6-container/` | Docker 및 Kubernetes 실습 |

## 6. 학습 기록

각 학습 단계에서 다음 내용을 정리합니다.

- 학습한 기술의 개념과 동작 원리
- 실습 환경 구성 과정
- 사용한 명령어 및 설정 파일
- 실습 결과와 검증 방법
- 발생한 오류 및 해결 과정
- 추가로 공부할 내용

## 7. 마무리

이론을 공부하는 것에 그치지 않고 직접 실습하면서 기술의 동작 원리를 이해하는 데 중점을 두고 있습니다.

앞으로도 클라우드와 인프라 관련 기술을 꾸준히 학습하고, 그 과정과 결과를 이 저장소에 기록해 나갈 예정입니다.
