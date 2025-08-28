# Rebellions Result Collection Tool

서버 하드웨어 정보를 자동으로 수집하고 정리하는 도구입니다. SSH를 통해 OS 레벨에서 하드웨어 정보를 수집하고, BMC(IPMI) 웹 인터페이스를 통해 추가 정보를 수집합니다.

## 🚀 주요 기능

### OS 정보 수집 (`os_collect.py`)

- **SSH 연결**: 다중 서버에 SSH로 접속하여 하드웨어 정보 수집
- **자동 권한 상승**: sudo를 통한 root 권한 획득
- **하드웨어 정보 수집**:
  - CPU 정보 (`lscpu`)
  - 메모리 정보 (`free -h`)
  - 디스크 정보 (`lsblk`)
  - RAID 정보 (`storcli`)
  - NPU 정보 (`rbln-stat`)
- **스크린샷 캡처**: 각 명령어 실행 결과를 PNG 이미지로 저장
- **Excel 연동**: 수집된 정보를 Excel 파일에 자동 저장

### BMC 정보 수집 (`bmc_collect.py`)

- **웹 자동화**: Selenium을 사용한 BMC 웹 인터페이스 자동 접근
- **요소별 스크린샷**: 특정 XPath 요소의 스크린샷을 개별적으로 캡처
- **SSL 인증서 무시**: 자체 서명된 인증서가 있는 BMC에도 접근 가능

## 📋 요구사항

### 시스템 요구사항

- Python 3.7+
- Windows 10/11 (pyautogui 사용)
- Chrome 브라우저 (BMC 수집용)

### Python 패키지

```bash
pip install openpyxl paramiko pyautogui selenium webdriver-manager pillow
```

## 🛠️ 설치 및 설정

1. **저장소 클론**

```bash
git clone <repository-url>
cd rebellions-result
```

2. **의존성 설치**

```bash
pip install -r requirements.txt
```

3. **Excel 파일 설정**
   - `inventory.xlsx` 파일을 프로젝트 루트에 배치
   - OS 시트: 호스트명, 사용자명, 비밀번호, 이름, 체크여부
   - BMC 시트: IP, ID, 비밀번호, 이름

## 📖 사용법

### 1. OS 정보 수집

```bash
python os_collect.py
```

**수집 과정:**

1. Excel 파일에서 서버 목록 읽기
2. 각 서버에 SSH 연결
3. sudo 권한 획득
4. 하드웨어 정보 수집 명령어 실행
5. 결과를 스크린샷으로 캡처
6. 폴더별로 정리하여 저장

**수집되는 정보:**

- `dmidecode`: 제조사, 제품명, 시리얼 번호
- CPU, 메모리, 디스크, RAID, NPU 상태

### 2. BMC 정보 수집

```bash
python bmc_collect.py
```

**수집 과정:**

1. Chrome 브라우저 자동 실행
2. BMC 웹 인터페이스 접속
3. 로그인 및 인증
4. 지정된 요소들의 스크린샷 캡처
5. 결과를 폴더별로 정리

## 📁 프로젝트 구조

```
rebellions-result/
├── os_collect.py          # OS 레벨 하드웨어 정보 수집
├── bmc_collect.py         # BMC 웹 인터페이스 정보 수집
├── inventory.xlsx         # 서버 정보 및 결과 저장 Excel
├── requirements.txt       # Python 패키지 의존성
└── README.md             # 프로젝트 문서
```

## 🔧 주요 클래스 및 함수

### SSHExpect 클래스

- **expect()**: 특정 패턴이 나타날 때까지 대기
- **send()**: 명령어 전송
- **execute_and_wait()**: 명령어 실행 후 결과 수집

### ResultProcess 클래스

- **capture_element_screenshot()**: 특정 요소의 스크린샷 캡처
- **get_result()**: BMC 웹 인터페이스에서 정보 수집

### 유틸리티 함수

- **cmd_and_capture()**: 명령어 실행 및 스크린샷 촬영
- **ssh_connect()**: SSH 연결 설정

## 📊 결과 저장

### 폴더 구조

```
{제조사}_{제품명}_{시리얼번호}/
├── os/
│   ├── cpu.png
│   ├── memory.png
│   ├── disk.png
│   ├── raid1.png
│   ├── raid2.png
│   ├── npu1.png
│   ├── npu2.png
│   └── npu3.png
└── bmc/
    ├── system.png
    ├── cpu.png
    ├── memory.png
    ├── fan_speed.png
    ├── psu.png
    ├── raid.png
    └── nic.png
```

### Excel 파일 업데이트

- 수집된 정보가 자동으로 `inventory.xlsx`에 저장
- 제조사, 제품명, 시리얼 번호 정보 업데이트

## ⚠️ 주의사항

1. **권한**: 대상 서버에 SSH 접근 권한이 필요
2. **네트워크**: 서버와 BMC IP에 네트워크 접근 가능해야 함
3. **보안**: 비밀번호가 평문으로 저장되므로 보안에 주의
4. **화면 해상도**: pyautogui 사용 시 적절한 화면 해상도 필요

## 🐛 문제 해결

### SSH 연결 문제

- 방화벽 설정 확인
- SSH 서비스 상태 확인
- 사용자 권한 확인

### BMC 접근 문제

- Chrome 드라이버 버전 확인
- SSL 인증서 설정 확인
- 네트워크 연결 상태 확인

### 스크린샷 문제

- 화면 해상도 설정 확인
- pyautogui 권한 확인

## 📝 라이선스

이 프로젝트는 내부 사용을 위한 도구입니다.

---

**개발팀**: 아이클라우드(주) AI-Infra  
**최종 업데이트**: 2025.08.28
