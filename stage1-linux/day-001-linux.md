# Day 1 — Linux OS Architecture and Operating Principles

## 1. 학습 목표
- 운영체제(OS)와 Linux 배포판의 차이 이해
- Kernel의 주요 역할 이해
- User Space와 Kernel Space의 분리 목적 이해
- System Call의 역할 이해
- Linux 기본 시스템 정보 확인 및 Load Average 해석

## 2. 핵심 개념

### 운영체제와 Linux
- **Linux Kernel**: 하드웨어 자원과 시스템 핵심 기능을 관리하는 운영체제의 핵심
- **Ubuntu**: Linux Kernel을 기반으로 GNU 도구, 시스템 서비스, 패키지 관리자, 응용 프로그램 등을 포함한 배포판
- **WSL2 환경 차이**: WSL2는 Microsoft가 빌드한 전용 커널과 경량 가상화 위에서 동작하므로, 일반적인 물리 Linux 서버와 커널 및 가상화 환경에서 차이가 있다.

### Kernel의 주요 역할
- **Process Management**: 프로세스 생성, 실행, 스케줄링
- **Memory Management**: 메모리 할당 및 보호
- **Filesystem Management**: 파일시스템과 파일 접근 제어
- **Device Management**: 장치 및 드라이버 관리
- **Network Stack**: 네트워크 패킷 송수신 처리
- **Security**: 권한 및 접근 제어

### User Space와 Kernel Space
- **User Space**: 일반 응용 프로그램과 셸이 실행되는 영역
- **Kernel Space**: 커널이 하드웨어 권한을 가지고 실행되는 영역
- 두 영역을 분리하여 메모리 보호, 프로세스 격리, 시스템 안정성 및 보안을 확보한다.

### System Call
- 사용자 프로그램(User Space)이 커널(Kernel Space)의 기능을 요청하기 위한 표준 인터페이스
- 파일 읽기, 프로세스 생성, 네트워크 통신 등은 반드시 System Call을 거쳐 커널에 요청한다.
- 예: `ls` 명령어는 커널의 파일시스템 인터페이스를 통해 디렉터리 정보를 얻는다.

### Shell Builtin과 External Command
- **Bash(Shell)**: 사용자의 명령어를 해석하고 실행하는 인터페이스
- **Shell Builtin**: Bash 내부에 직접 구현된 명령어 (예: `cd`, `export`, `alias`, `exit`)
- **External Command**: 별도의 컴파일된 실행 파일로 존재하는 명령어 (예: `/usr/bin/ls`)
- **Alias**: 기존 명령어를 다른 형태로 치환하는 기능
  - Ubuntu의 `ls`는 `ls --color=auto`로 Alias가 설정되어 있으며, 실제 바이너리는 `/usr/bin/ls`에 위치한다.

### PID 1
- Linux 프로세스 계층의 최상위에 위치하는 최초의 프로세스
- 주요 역할: 시스템 초기화, 서비스 관리, 고아 프로세스 인계, 종료된 자식 프로세스 상태 회수
- 현재 WSL2 환경에서는 `systemd`가 PID 1로 동작한다.

### Load Average
- `uptime`으로 확인하는 시스템 부하 지표 (순서대로 최근 1분 / 5분 / 15분 평균)
- CPU 실행 대기(`R` 상태)뿐 아니라 디스크·스토리지 I/O 대기인 **Uninterruptible Sleep(`D` 상태)** 작업도 부하에 포함된다.
- 서버의 **Logical CPU 코어 수**를 기준으로 해석해야 하며, CPU 사용률과 다른 지표이므로 반드시 함께 확인한다.

## 3. 실습 명령어

```bash
# OS 및 커널 정보
cat /etc/os-release
uname -a
uname -r
uname -m

# 하드웨어 및 리소스 확인
lscpu
free -h
df -h
uptime

# 사용자 및 PID 1 프로세스 확인
whoami
hostname
ps -p 1 -o pid,ppid,comm,args

# 명령어 유형(Builtin / Alias / External) 확인
type cd
type ls
which ls
```

## 4. 실습 환경 및 확인 결과
- **OS**: Ubuntu 24.04.5 LTS
- **Kernel**: `6.6.87.2-microsoft-standard-WSL2`
- **Architecture**: `x86_64`
- **CPU**: AMD Ryzen 7 7700X (Logical CPU: 16)
- **Memory (WSL2 인식)**: 약 30 GiB
- **PID 1**: `systemd`
- **Shell**: Bash
- **명령어 확인**: `cd`(Shell Builtin), `ls`(Alias: `ls --color=auto`, 실제 경로: `/usr/bin/ls`)

## 5. 핵심 검증 Q&A

### Q1. `cd` 명령어는 왜 외부 실행 파일(`/usr/bin/cd`)이 아니라 Shell Builtin으로 구현되어 있는가?
- 외부 명령어는 실행 시 부모 셸에서 **자식 프로세스를 새로 생성(fork/exec)**하여 작업을 수행한 뒤 종료한다.
- 현재 작업 디렉터리(CWD)는 프로세스마다 독립적으로 관리되는 속성이므로, `cd`가 자식 프로세스에서 실행되면 자식 프로세스의 위치만 바뀌고 종료되어 **부모 셸의 위치는 변하지 않는다.**
- 따라서 현재 셸의 상태를 직접 변경해야 하는 `cd`는 반드시 Shell Builtin으로 구현되어야 한다.

### Q2. Logical CPU 8개 서버에서 Load Average가 `20.0`인데 CPU 사용률은 `10%` 미만이다. 무엇을 의심해야 하는가?
- Load Average에는 CPU 대기 작업뿐만 아니라 **Uninterruptible Sleep(`D` 상태)** 프로세스도 포함된다.
- CPU 사용률이 낮은데 부하만 높다면 연산 병목이 아니라 **디스크 I/O 지연이나 NFS/네트워크 스토리지 응답 대기**로 인해 다수의 프로세스가 `D` 상태로 쌓여 있을 가능성이 높다.
- **1차 진단 명령어**: `iostat -xz 1`, `vmstat 1`(`wa`, `b` 항목), `ps -eo stat,pid,comm | grep D`

## 6. Day 1 결론
- Linux는 커널이 하드웨어 자원을 독점 관리하고, 사용자 프로그램이 System Call을 통해 커널 기능을 호출하는 구조로 동작한다.
- WSL2는 물리 Linux 서버와 커널 및 가상화 환경에서 차이가 있으므로, 향후 시스템·네트워크 장애 분석 시 이 차이를 인지하고 지표를 해석해야 한다.