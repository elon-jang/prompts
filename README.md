# AI Prompts Collection

AI 어시스턴트와 함께 사용할 수 있는 프롬프트 템플릿 모음입니다.

## 📁 프로젝트 구조

```
prompts/
├── tccli/                    # Tencent Cloud CLI 관련 프롬프트
│   └── prompts/
│       ├── vm-start.md       # VM 인스턴스 시작
│       ├── vm-stop.md        # VM 인스턴스 중지
│       ├── vm-status.md      # VM 인스턴스 상태 조회
│       ├── vm-manage.md      # VM 시작/중지 통합 관리
│       └── README.md         # 상세 사용 가이드
└── CLAUDE.md                 # Claude Code용 저장소 가이드
```

## 🚀 프롬프트 카테고리

### Tencent Cloud VM 관리 (`/tccli`)

Tencent Cloud CVM(Cloud Virtual Machine) 인스턴스를 관리하기 위한 표준화된 프롬프트 템플릿입니다.

**주요 기능:**
- ✅ VM 인스턴스 시작/중지/상태 조회
- ✅ Terminal MCP와 통합 사용 가능
- ✅ Claude Code 슬래시 커맨드로 활용 가능
- ✅ 여러 인스턴스 동시 관리 지원

**사용 방법:**
```bash
# 프롬프트 파일 내용을 AI 어시스턴트에게 전달 후
"VM 시작해줘"
"VM 중지해줘"
"VM 상태 확인해줘"
```

**Claude Code 슬래시 커맨드로 사용:**
```bash
cp tccli/prompts/vm-start.md .claude/commands/vm-start.md
```
이후 `/vm-start` 명령으로 사용 가능합니다.

자세한 사용법은 [tccli/prompts/README.md](tccli/prompts/README.md)를 참고하세요.

## 🛠️ 사용 방법

### 1. 직접 프롬프트 복사

프롬프트 파일의 내용을 복사하여 AI 어시스턴트에게 전달합니다.

### 2. Claude Code 슬래시 커맨드

Claude Code를 사용하는 경우, 프롬프트를 `.claude/commands/` 디렉토리에 복사하여 슬래시 커맨드로 활용할 수 있습니다.

```bash
# 예시
cp tccli/prompts/vm-start.md .claude/commands/vm-start.md

# 사용
/vm-start
```

### 3. Terminal MCP 통합

Terminal MCP를 지원하는 AI 어시스턴트와 함께 사용하면 실제 시스템 명령을 실행할 수 있습니다.

## 📋 필수 요구사항

각 프롬프트별 요구사항은 해당 디렉토리의 README를 참고하세요.

### Tencent Cloud VM 관리
- `tccli` 설치: `pip install tccli`
- Tencent Cloud 인증 설정 완료
- 관리할 인스턴스 ID 확인

## 🎯 프롬프트 커스터마이징

프롬프트는 자유롭게 수정하여 사용할 수 있습니다:

1. 기본 인스턴스 ID 변경
2. 언어 변경 (현재 한국어 → 영어 등)
3. 추가 검증 로직 삽입
4. 출력 형식 조정

## 📚 추가 자료

- [CLAUDE.md](CLAUDE.md) - Claude Code로 이 저장소를 작업할 때의 가이드
- [tccli/prompts/README.md](tccli/prompts/README.md) - Tencent Cloud VM 관리 프롬프트 상세 가이드

## 🤝 기여

프롬프트 개선이나 새로운 프롬프트 추가는 언제든 환영합니다!

## 📄 라이선스

MIT License
