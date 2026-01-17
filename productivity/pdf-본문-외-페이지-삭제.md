# PDF 본문 외 페이지 삭제

## Description

PDF 파일에서 광고, 안내 등 본문과 무관한 페이지를 자동으로 감지하여 삭제하는 프롬프트

## Tags

pdf, automation, cleanup, pymupdf

## Usage

1. 대상 PDF 파일 경로 지정
2. 삭제할 페이지의 키워드 패턴 정의
3. PyMuPDF 라이브러리로 처리

```bash
pip install pymupdf
```

## Prompt

```
대상: /Users/elon/Downloads/longblack/*.pdf

삭제 기준 (뒤에서부터 검사, 아래 텍스트 포함 시 삭제):
- "타임앤코" + "사업자" + "정보"
- "공개" + "예정" + "노트"
- "전체" + "노트" + "보러가기"
- "읽은" + "노트에" + "평점"

PyMuPDF 사용, 정규식으로 키워드 사이 특수문자 허용
```
