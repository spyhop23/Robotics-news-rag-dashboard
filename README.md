# Robotics News RAG Dashboard

한국과 일본의 로봇 산업 뉴스를 수집해 검색 가능한 지식 베이스로 만들고, RAG(Retrieval-Augmented Generation) 기반 질의응답과 LLM 요약 기능을 갖춘 대시보드로 제공하는 프로젝트입니다.

- 수집 기간: 2024.09 ~ 2025.08 (12개월)
- 수집 규모: 한국 5,143건, 일본 1,691건 (총 6,834건)
- 실행 환경: Google Colab

## 저장소 구성

이 저장소는 단계별 Colab 노트북 3개로 구성되어 있습니다. 위에서부터 순서대로 실행합니다.

| 노트북 | 내용 |
|---|---|
| [`crawlers.ipynb`](crawlers.ipynb) | 뉴스 사이트별 기사 수집, 본문·메타데이터 파싱, URL·본문 해시 기반 중복 제거 |
| [`rag.ipynb`](rag.ipynb) | 문장 단위 청킹, 다국어 임베딩(`paraphrase-multilingual` 계열 SentenceTransformer), FAISS 인덱스 구축, RAG 질의응답 파이프라인, RAGAS 평가 |
| [`dashboard.ipynb`](dashboard.ipynb) | Streamlit 대시보드 생성 및 실행 (Colab에서 ngrok 터널로 접속) |

수집 데이터(`data/`)와 벡터 인덱스는 용량과 저작권 문제로 저장소에 포함하지 않았습니다 (`.gitignore` 참고).

## 파이프라인

```
뉴스 수집 → 정제·중복 제거 → 청킹 → 다국어 임베딩 → FAISS 인덱스
                                                        ↓
                         사용자 질문 → 유사 청크 Top-k 검색 → GPT-4o-mini 응답 생성
```

- 한국어와 일본어 기사를 하나의 의미 공간에 매핑해, 한 언어로 질문해도 두 나라의 기사를 함께 검색할 수 있게 했습니다.

## 대시보드 주요 기능

1. 기간·국가·토픽 기반 뉴스 필터링
2. 월별 토픽 트렌드와 문서량 변화 시각화
3. 토픽별·국가별 문서 분포 비교
4. 특정 토픽의 대표 문장과 LLM 요약 조회
5. RAG 기반 질의응답
6. 현재 필터 조건을 반영한 자동 분석 보고서 생성

## RAG 평가 결과 (RAGAS, 20개 샘플)

| 지표 | 점수 | 해석 |
|---|---|---|
| Context Utilization | 0.998 | 검색된 컨텍스트를 거의 빠짐없이 활용 |
| Answer Relevancy | 0.849 | 질문 의도에 맞는 답변 |
| Faithfulness | 0.670 | 생성 과정의 요약·재구성으로 근거 충실도는 상대적으로 낮음 |

Faithfulness를 높이려면 답변 생성 시 인용 근거를 함께 출력하거나, 요약 단계를 줄이는 방향의 개선이 필요합니다.

## 실행 방법

1. 노트북을 Google Colab에서 엽니다.
2. OpenAI API 키를 환경변수로 설정합니다. 키를 코드에 직접 적지 않습니다.
   ```python
   import os
   os.environ["OPENAI_API_KEY"] = "발급받은 키"  # 또는 Colab의 보안 비밀(Secrets) 기능 사용
   ```
3. `crawlers.ipynb` → `rag.ipynb` → `dashboard.ipynb` 순서로 실행합니다.
4. `dashboard.ipynb` 마지막 셀에서 출력되는 ngrok 주소로 대시보드에 접속합니다.

## 기술 스택

Python, BeautifulSoup, pandas, SentenceTransformers, FAISS, OpenAI API (GPT-4o-mini), RAGAS, Streamlit, ngrok
