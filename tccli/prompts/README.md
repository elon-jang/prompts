# Tencent Cloud VM 관리 프롬프트

이 디렉토리에는 Terminal MCP를 통해 Tencent Cloud CVM 인스턴스를 관리하기 위한 표준 프롬프트가 포함되어 있습니다.

## 파일 구조

- `vm-start.md` - VM 인스턴스 시작 전용 프롬프트
- `vm-stop.md` - VM 인스턴스 중지 전용 프롬프트
- `vm-manage.md` - VM 시작/중지 통합 프롬프트

## 사용 방법

### 1. 직접 프롬프트 사용
프롬프트 파일의 내용을 복사하여 AI 어시스턴트에게 전달합니다.

### 2. Terminal MCP와 함께 사용
```bash
# 프롬프트 내용을 AI에게 전달한 후, 간단히 요청
"VM 시작해줘"
"VM 중지해줘"
```

### 3. 슬래시 커맨드로 설정
Claude Code의 `.claude/commands/` 디렉토리에 복사하여 슬래시 커맨드로 사용할 수 있습니다.

```bash
# 예시
cp prompts/vm-start.md .claude/commands/vm-start.md
cp prompts/vm-stop.md .claude/commands/vm-stop.md
```

그 후:
```
/vm-start
/vm-stop
```

## 필수 요구사항

- tccli 설치: `pip install tccli`
- Tencent Cloud 인증 설정 완료
- 인스턴스 ID 확인 (현재: ins-1rqliwb5)

## 인스턴스 ID 변경

다른 인스턴스를 관리하려면 프롬프트 파일에서 `ins-1rqliwb5`를 해당 인스턴스 ID로 변경하세요.

## 추가 명령어

### 인스턴스 상태 확인
```bash
tccli cvm DescribeInstances --InstanceIds '["ins-1rqliwb5"]'
```

### 인스턴스 재시작
```bash
tccli cvm RebootInstances --InstanceIds '["ins-1rqliwb5"]'
```
