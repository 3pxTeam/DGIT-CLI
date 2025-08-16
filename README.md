# DGit CLI

디자인 파일 전용 버전 관리 시스템의 핵심 엔진

## 개요

DGit CLI는 디자인 파일(.psd, .ai, .sketch, .fig 등)을 위해 특별히 설계된 버전 관리 시스템입니다. 기존 Git의 한계를 극복하고, 대용량 바이너리 디자인 파일을 위한 지능적인 압축과 메타데이터 추적을 제공합니다.

## 주요 기능

- **압축**: 고급 압축으로 최대 88% 저장공간 절약  
- **디자인 인식**: 레이어 수, 크기, 색상 모드 등 의미있는 변경사항 추적
- **사용자 경험**: 디자이너 친화적 인터페이스와 시각적 피드백

### 지능적 압축 시스템

```
사용자 작업 → 빠른 압축 → 즉시 응답
           ↓
      백그라운드 최적화 (유휴 시간)
           ↓
      장기 저장소 (자동화)
```

### 디자인 파일 인식

```bash
📝 상태 확인:
🔄 수정됨: logo.psd
📐 크기: 1920×1080 → 2560×1440
🎨 레이어: 57 → 59 (+2)
🎯 색상 모드: RGB → CMYK
💾 파일 크기: 56MB → 61MB
```

## 설치 방법

### 필요 사항
- Go 1.21 이상
- 명령줄 접근 권한

### 빌드 및 설치

```bash
# 저장소 복제
git clone https://github.com/3pxTeam/DGIT-CLI.git
cd DGIT-CLI

# 애플리케이션 빌드
go build -o dgit

# 전역 설치 (선택사항)
sudo mv dgit /usr/local/bin/
```

### 설치 확인

```bash
dgit --help
```

## 사용법

### 저장소 초기화

```bash
# 현재 디자인 프로젝트 폴더에서
dgit init

# 또는 특정 디렉토리 지정
dgit init /path/to/project
```

### 파일 스캔 및 추가

```bash
# DGit이 관리할 수 있는 디자인 파일 확인
dgit scan .

# 특정 파일 추가
dgit add logo.psd banner.ai

# 현재 디렉토리의 모든 디자인 파일 추가
dgit add .

# 패턴으로 파일 추가
dgit add *.psd
```

### 커밋 생성

```bash
dgit commit -m "초기 디자인 파일"
```

예제 출력:
```
🎨 2개의 디자인 파일로 커밋 생성 중...
📊 파일 메타데이터 분석 중...
📦 압축 적용 중...
✨ 커밋 생성됨: a1b2c3d4
📝 초기 디자인 파일
🎨 커밋된 파일:
🔵 logo.psd (12 레이어, 1920×1080, RGB)
🟠 banner.ai (8 레이어, 1200×800, CMYK)
```

### 상태 확인 및 히스토리

```bash
# 현재 상태 확인
dgit status

# 커밋 히스토리 보기
dgit log
```

### 파일 복원

```bash
# 특정 버전에서 파일 복원
dgit restore v2 logo.psd

# 특정 버전의 모든 파일 복원
dgit restore v1
```

## 지원 파일 형식

### 완전 지원 (메타데이터 추출 포함)
- ✅ **Adobe Photoshop** (.psd) - 레이어, 크기, 색상 모드, 비트 깊이
- ✅ **Adobe Illustrator** (.ai) - 아트보드, 레이어, 벡터 콘텐츠, 색상 공간

### 기본 지원 (파일 추적 및 버전 관리)
- 🔶 **Sketch** (.sketch)
- 🔶 **Figma** (.fig) 
- 🔶 **Adobe XD** (.xd)
- 🔶 **Affinity Designer** (.afdesign)
- 🔶 **Blender** (.blend)

### 향후 지원 예정
- Sketch: 심볼 라이브러리, 아트보드 분석
- Figma: 컴포넌트 시스템, 디자인 토큰
- Adobe XD: 프로토타입 플로우, 컴포넌트 상태

## 설정

`.dgit/config` 파일을 생성하여 동작 방식을 커스터마이징할 수 있습니다:

```json
{
  "user": {
    "name": "Your Name",
    "email": "your.email@example.com"
  },
  "compression": {
    "level": "balanced",
    "background_optimization": true
  },
  "ui": {
    "show_file_details": true,
    "color_output": true
  }
}
```

## 명령어 참조

| 명령어 | 설명 | 예제 |
|-------|------|------|
| `dgit init` | 저장소 초기화 | `dgit init` |
| `dgit scan` | 디자인 파일 찾기 | `dgit scan .` |
| `dgit add` | 파일 스테이징 | `dgit add *.psd` |
| `dgit commit` | 버전 생성 | `dgit commit -m "메시지"` |
| `dgit status` | 변경사항 확인 | `dgit status` |
| `dgit log` | 히스토리 보기 | `dgit log` |
| `dgit restore` | 파일 복원 | `dgit restore v2 file.psd` |

### 저장공간 절약
- **기존 방식**: 560MB (10개 버전 × 56MB 각각)
- **DGit 스마트 압축**: 190MB (66% 공간 절약)

## 시스템 요구사항

- **운영체제**: macOS 10.14+, Linux, Windows 10+
- **메모리**: 최소 4GB RAM
- **저장공간**: 100MB 이상
- **Go**: 1.21 이상 (빌드 시)

## 개발

### 테스트 실행

```bash
# 모든 테스트 실행
go test ./...

# 상세 출력으로 실행
go test -v ./...

# 특정 컴포넌트 테스트
go test ./internal/commit/
go test ./internal/scanner/

# 성능 테스트 실행
go test -bench=. ./internal/commit/
go test -bench=. ./internal/restore/
```

### 프로젝트 구조

```
dgit/
├── main.go                 # 애플리케이션 진입점
├── cmd/                    # 명령줄 인터페이스
│   ├── initCmd.go         # 저장소 초기화
│   ├── addCmd.go          # 파일 스테이징
│   ├── commitCmd.go       # 버전 생성
│   ├── statusCmd.go       # 저장소 상태
│   ├── logCmd.go          # 히스토리 보기
│   ├── restoreCmd.go      # 파일 복원
│   └── scanCmd.go         # 파일 발견
└── internal/              # 핵심 비즈니스 로직
    ├── init/              # 저장소 설정
    ├── staging/           # 파일 스테이징 관리
    ├── commit/            # 압축 및 버전 관리
    ├── log/               # 히스토리 추적
    ├── restore/           # 파일 복원
    ├── status/            # 변경사항 감지
    └── scanner/           # 파일 분석
        ├── photoshop/     # PSD 파일 파서
        └── illustrator/   # AI 파일 파서
```

## 기여하기

커뮤니티 기여를 환영합니다!

### 기여 방법
1. 저장소 포크
2. 포크한 저장소 복제: `git clone https://github.com/yourusername/DGIT-CLI.git`
3. 기능 브랜치 생성: `git checkout -b new-feature`
4. 변경사항 구현 및 테스트 추가
5. 테스트 통과 확인: `go test ./...`
6. 변경사항 커밋 및 푸시
7. Pull Request 제출

### 기여 가이드라인
- Go 코딩 표준 준수
- 새로운 기능에 대한 테스트 추가
- 필요시 문서 업데이트
- 커밋은 집중적이고 설명적으로 작성

## 라이선스

MIT License

## 관련 프로젝트

- [DGit macOS](https://github.com/3pxTeam/DGIT-MAC) - 네이티브 macOS GUI 애플리케이션
- [3pxTeam](https://github.com/3pxTeam) - 조직 메인 페이지
