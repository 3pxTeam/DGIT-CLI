# DGIT-CLI

디자인 파일(.psd, .ai, .sketch, .fig 등)을 위한 버전 관리 시스템입니다.

## 특징

- **Go 언어 기반** CLI 도구
- **3단계 캐시 시스템**: Hot Cache (LZ4) → Warm Cache (Zstd) → Cold Cache (Archive)
- **디자인 파일 메타데이터 추적**: PSD, AI 파일의 레이어, 크기, 색상 모드 변경 감지
- **지능형 압축 전략**: 파일 특성에 따른 최적 알고리즘 자동 선택

## 지원 파일 형식

- ✅ **Adobe Photoshop (.psd)** - 레이어, 크기, 색상 모드, 비트 깊이 추적
- ✅ **Adobe Illustrator (.ai)** - 아트보드, 레이어, 벡터 콘텐츠, 색상 공간 추적
- 🔶 **Sketch (.sketch)** - 파일 버전 관리
- 🔶 **Figma (.fig)** - 파일 버전 관리
- 🔶 **Adobe XD (.xd)** - 파일 버전 관리

## 설치

### 요구사항
- Go 1.21 이상

### 빌드
```bash
git clone https://github.com/3pxTeam/DGIT-CLI.git
cd DGIT-CLI
go build -o dgit
```

## 사용법

### 기본 명령어

```bash
# 저장소 초기화
dgit init [directory]

# 디자인 파일 스캔
dgit scan [folder]

# 파일 추가
dgit add logo.psd           # 특정 파일
dgit add .                  # 현재 디렉토리 모든 파일
dgit add *.psd              # 패턴 매칭
dgit add designs/ icons/    # 여러 디렉토리

# 커밋
dgit commit "Logo design completed"
dgit commit -m "Updated color scheme"
dgit commit                 # 대화형 메시지 입력

# 상태 확인
dgit status

# 히스토리 보기
dgit log
dgit log --oneline          # 압축 형태
dgit log -n 5               # 최근 5개만

# 파일 복원
dgit restore 1              # 버전 1의 모든 파일
dgit restore c3a5f7b8       # 해시로 복원
dgit restore 2 my_design.psd    # 특정 파일만
dgit restore 2 designs/     # 디렉토리
```

### 실제 출력 예시

**커밋:**
```bash
$ dgit commit "Logo design completed"
Creating commit with 2 design files...
Analyzing design file metadata...
Creating snapshot archive...

Created commit a1b2c3d4
Logo design completed
Author: DGit User
Design files (2):
   [PSD] logo.psd (12 layers, 1920×1080, RGB)
   [AI] banner.ai (8 layers, 1200×800, CMYK)
Snapshot: v1.zip
Ready for collaboration!
```

**상태 확인:**
```bash
$ dgit status
On version 2

Changes to be committed:
  [PSD] new file: updated_logo.psd

Changes not staged for commit:
  modified: banner.ai (Layers: 8→10, ColorMode: CMYK→RGB)

Untracked files:
  [SKETCH] new_design.sketch
```

**스캔:**
```bash
$ dgit scan
Scanning design files in: .
Found 3 design files (45.2 MB total)

File Types:
   PSD files: 2
   AI files: 1

Design Files Analysis:
[PSD] logo.psd
   1920×1080 • RGB • Adobe Photoshop 2023
   12 layers

[AI] banner.ai  
   1200×800 • CMYK • Adobe Illustrator 2023
   8 layers • 3 artboards
```

## 프로젝트 구조

```
dgit/
├── main.go                 # 애플리케이션 진입점
├── cmd/                    # CLI 명령어
│   ├── initCmd.go         # 저장소 초기화
│   ├── addCmd.go          # 파일 스테이징
│   ├── commitCmd.go       # 버전 생성
│   ├── statusCmd.go       # 저장소 상태
│   ├── logCmd.go          # 히스토리 조회
│   ├── restoreCmd.go      # 파일 복원
│   └── scanCmd.go         # 파일 발견
└── internal/              # 핵심 로직
    ├── init/              # 저장소 설정
    ├── staging/           # 파일 스테이징 관리
    ├── commit/            # 압축 및 버전 관리
    ├── log/               # 히스토리 추적
    ├── restore/           # 파일 복원
    ├── status/            # 변경 감지
    └── scanner/           # 파일 분석
        ├── photoshop/     # PSD 파일 파서
        └── illustrator/   # AI 파일 파서
```

## 기술 스택

### 기술 스택

**핵심 라이브러리:**
- `github.com/spf13/cobra` - CLI 프레임워크
- `github.com/pierrec/lz4/v4` - LZ4 고속 압축
- `github.com/klauspost/compress/zstd` - Zstandard 압축
- `github.com/kr/binarydist` - 바이너리 델타 압축
- `github.com/fatih/color` - 터미널 컬러 출력

**압축 전략:**
- **Hot Cache**: LZ4 압축으로 즉시 접근
- **Warm Cache**: Zstd 압축으로 백그라운드 최적화
- **Cold Cache**: 최대 압축률로 장기 저장
- **Delta 압축**: PSD 스마트 델타, bsdiff 바이너리 델타

## 설정

`.dgit/config` 파일로 동작 설정:

```json
{
  "user": {
    "name": "Your Name",
    "email": "your.email@example.com"
  },
  "compression": {
    "level": "balanced",
    "background_optimization": true
  }
}
```

## 테스트

```bash
# 전체 테스트
go test ./...

# 상세 출력
go test -v ./...

# 특정 모듈 테스트
go test ./internal/commit/
go test ./internal/scanner/

# 성능 벤치마크
go test -bench=. ./internal/commit/
go test -bench=. ./internal/restore/
```

## 라이선스

MIT License
