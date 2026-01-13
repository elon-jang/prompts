# SPEC 인터뷰 프롬프트

SPEC.md 파일을 기반으로 심층 인터뷰를 진행하여 요구사항을 구체화하는 프롬프트입니다.

## 파일 구조

- `prompt01.md` - AskUserQuestionTool을 활용한 심층 인터뷰 프롬프트

## 사용 방법

### 1. 직접 사용

프롬프트 내용을 AI 어시스턴트에게 전달합니다.

### 2. 슬래시 커맨드로 설정

```bash
cp askquestion/prompt01.md .claude/commands/spec-interview.md
```

이후 `/spec-interview`로 사용 가능합니다.

## 주요 기능

- SPEC.md 파일 분석
- 기술적 구현, UI/UX, 우려 사항, 트레이드오프 등 심층 질문
- 인터뷰 완료 후 스펙 파일 자동 작성

## 필수 요구사항

- SPEC.md 파일이 프로젝트에 존재해야 함
- AskUserQuestionTool 지원 AI 어시스턴트 필요
