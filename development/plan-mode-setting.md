# Plan Mode 설정

## Description

코드를 작성하기 전에 항상 계획을 제시하도록 AI 에이전트를 설정하는 방법

## Tags

ai, agent, configuration, planning, claude-code, cursor, codex

## Usage

각 AI 코딩 도구의 설정 파일에 아래 내용을 추가

## Prompt

````
### Claude Code

```bash
mkdir -p ~/.claude && cat >> ~/.claude/CLAUDE.md << 'EOF'

## Plan Mode

- Before writing any code, present your implementation plan in bullet points.
- Keep the plan extremely concise. Prioritize clarity over grammar.
- At the end of each plan, list any unresolved questions you need answered.
EOF
```

### Cursor / Codex / Gemini / Cline

각 도구의 설정 폴더에 동일한 내용 추가:
- `.cursor/AGENTS.md`
- `.codex/AGENTS.md`
- `.gemini/AGENTS.md`
- `.cline/AGENTS.md`

### 왜 이렇게 설정하나요?

- **코드 작성 전 계획 제시** - 방향이 틀어지기 전에 먼저 확인할 수 있어요
- **극도로 간결하게** - 장황한 설명은 핵심을 흐립니다. 문법보다 의도 전달이 우선이에요
- **미해결 질문 명시** - 에이전트가 모르는 걸 숨기지 않고 드러내게 만듭니다. 추측 대신 확인하는 습관을 심어주는 거죠

### 참조

[Cursor, Codex, Gemini, Cline… 모든 AI 코딩 에이전트에 통하는 간편한 Plan Mode 설정법]()
````
