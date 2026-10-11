# AI Second-Brain Study

AI와 충분히 질문하고 → 대화를 개인화된 학습 노트로 정리하고 → 개념의 위계와 연결을 재구성하는 **파일 기반 에이전트 스킬**입니다.

**가장 중요한 원칙: AI가 설명이나 노트를 만들었다는 사실은 학습자가 이해했다는 증거가 아닙니다.** `verified` 판정에는 학습자 자신의 원리 설명과 새로운 상황에 대한 독립 적용이 필요합니다.

영상 다운로드 프로그램, 성적 예측기, 자동 채점 앱, NotebookLM 자동화 도구가 아닙니다. 영상에서 가져온 학습 흐름과 별도로 이해 확인·힌트 관리·복습 규칙을 보완했습니다.

## 필요한 환경

- 프로젝트 파일을 읽고 쓸 수 있는 AI 에이전트. 아래 설치·호출 안내는 Codex의 프로젝트 스킬 경로를 기준으로 합니다.
- 복제에 사용할 Git. GitHub에서 ZIP으로 받아도 됩니다.
- NotebookLM은 선택 사항입니다. 기본 학습에는 계정이나 외부 업로드가 필요하지 않습니다.
- 별도 Python/Node 패키지, API 키, Flucto 설치는 필요하지 않습니다. 모델 이용 조건과 비용은 사용하는 에이전트 서비스에 따릅니다.

일반 웹 채팅에 저장소 주소만 붙인다고 스킬이 설치되지는 않습니다. 파일 접근이나 스킬 자동 발견을 지원하지 않는 환경에서는 해당 환경의 문서 첨부·스킬 등록 방식을 사용해야 합니다.

## 1. 빠른 시작: 이 저장소에서 공부하기

```sh
git clone https://github.com/DeclanJeon/ai-second-brain-study.git
cd ai-second-brain-study
codex
```

Codex를 따로 설치한 환경에서 실행하세요. 저장소에 `.agents/skills/ai-second-brain-study/`가 포함되어 있어 별도 복사가 필요하지 않습니다. Codex에서 `/skills` 또는 `$` 입력으로 스킬을 선택하고 다음처럼 요청합니다.

```text
$ai-second-brain-study
선형대수를 공부하고 싶다. 고등학교 수학까지 알고 있고 하루 30분씩 쓸 수 있다.
목표는 PCA에서 고유벡터가 왜 필요한지 내 말로 설명하고 새로운 예제에 적용하는 것이다.
공식부터 외우기보다 맥락과 원리부터 설명해줘.
```

주제·배경·목표·시간은 자신의 상황에 맞게 바꾸세요. 에이전트는 목표와 현재 이해를 확인하고 필요한 설명 또는 한 개의 이해 확인 과제로 진행합니다. 학습자가 답하지 않은 내용을 대신 만들어 정답으로 기록해서는 안 됩니다.

Codex의 경로·명시 호출·자동 발견 규칙: [공식 스킬 문서](https://developers.openai.com/codex/skills/). 설치 직후 보이지 않으면 에이전트를 재시작하세요.

## 2. 기존 프로젝트에 설치하기

**`SKILL.md`만 복사하지 말고 `references/`까지 폴더 전체를 복사하세요.** 기존 프로젝트의 루트에서 아래 명령을 실행합니다. 같은 이름의 설치 경로가 이미 있으면 덮어쓰지 않고 중단합니다.

### Windows PowerShell

```powershell
git clone https://github.com/DeclanJeon/ai-second-brain-study.git ai-second-brain-study
$destination = Join-Path (Get-Location) '.agents/skills/ai-second-brain-study'
if (Test-Path -LiteralPath $destination) { throw '기존 스킬이 있습니다. 백업·비교 후 설치하세요.' }
New-Item -ItemType Directory -Path '.agents/skills' -Force | Out-Null
Copy-Item -LiteralPath 'ai-second-brain-study/.agents/skills/ai-second-brain-study' -Destination '.agents/skills/' -Recurse
```

### macOS / Linux

```sh
git clone https://github.com/DeclanJeon/ai-second-brain-study.git ai-second-brain-study
test ! -e .agents/skills/ai-second-brain-study \
  && test ! -L .agents/skills/ai-second-brain-study \
  && mkdir -p .agents/skills \
  && cp -R ai-second-brain-study/.agents/skills/ai-second-brain-study .agents/skills/
```

설치 결과:

```text
.agents/skills/ai-second-brain-study/
├── SKILL.md
└── references/
    ├── assessment.md
    ├── storage.md
    └── source-method.md
```

다른 파일 접근형 에이전트에서는 자동 발견을 가정하지 말고 직접 요청할 수 있습니다.

```text
.agents/skills/ai-second-brain-study/SKILL.md를 읽고, 해당 스킬이 요구하는 references 문서도 읽어라.
이 스킬대로 미적분을 공부하자. 내 목표는 미분의 의미를 설명하고 최적화 문제에 적용하는 것이다.
```

전역 설치와 프로젝트 설치에 같은 스킬을 중복 등록하지 않는 편이 명확합니다. 업데이트할 때는 복제본에서 `git pull`한 뒤 변경 내용을 확인하고 스킬 폴더만 교체하세요. 개인 학습 노트는 교체 대상이 아닙니다. 재배포 시 이 저장소의 `LICENSE`도 유지하세요.

## 3. 목적별 사용 예시

### 이해될 때까지 질문하기

```text
$ai-second-brain-study
왜 고유벡터의 방향은 행렬을 곱해도 바뀌지 않는지 모르겠다.
기하학적인 맥락 → 원리 → 작은 예시 → PCA에서의 용도 순서로 설명해줘.
```

### 대화를 노트로 정리하기 — 퀴즈 없이

먼저 실제 대화 파일 `dialogue.md`를 프로젝트에 두고 요청합니다. 입력 파일은 이 저장소가 제공하지 않습니다.

```text
$ai-second-brain-study
dialogue.md의 대화를 개인화된 학습 노트로 정리해줘. 이번에는 퀴즈를 내지 마.
내가 실제로 한 질문, 설명 선호, 아직 해결되지 않은 혼동을 보존해줘.
AI의 주장과 내 답변을 구분하고, 노트를 만들었다고 verified로 올리지 마.
```

### 최소 힌트로 이해 확인하기

```text
$ai-second-brain-study
방금 배운 개념을 새로운 상황에 적용해볼 문제 하나를 내줘.
정답이나 풀이를 먼저 보여주지 말고 평가 기준을 먼저 정해줘.
막히면 관점 힌트 하나만 주고 내가 다시 답할 때까지 기다려줘.
```

### 저장한 세션 이어가기·복습하기

```text
$ai-second-brain-study
learning/linear-algebra/session.md를 읽고 학습을 이어가자.
이전 답변과 힌트 기록을 확인하고, 정답을 먼저 공개하지 않는 회상 문제 하나로 시작해줘.
```

위 경로는 해당 주제의 노트가 실제로 저장된 경우에 사용합니다. 다른 주제나 사용자 지정 경로라면 그 경로를 지정하세요. 복습 날짜가 지났다는 이유만으로 이해 상태를 낮추거나 장기 기억을 확인했다고 기록하지 않습니다.

## 4. 저장되는 내용

기본 저장 위치는 현재 프로젝트의 `learning/<topic-slug>/`입니다. 사용자 지정 경로가 있으면 우선합니다. 계획만 세웠을 때 빈 파일을 만들거나 아래 파일을 모두 무조건 생성하지 않습니다.

| 파일 | 역할 |
|---|---|
| `session.md` | 목표·배경·제약·현재 증거·다음 행동. 세션 재개 진입점 |
| `concepts.md` | 맥락·원리·예시·용도, 개념 간 관계의 이유·조건·근거 |
| `log.md` | 실제 답변, 평가 기준, 힌트·정답 노출, 판단 근거와 다음 개입 |
| `sources/` | 사용자가 제공한 전체 대화 등 원문 보존. 필요한 경우에만 생성 |
| `notebooklm/sources.md` | 외부 도구에 가져갈 출처별 실제 본문·발췌·불확실성 |
| `notebooklm/prompts.md` | 위계·연결을 재구성하고 출처를 확인할 질문 |

세부 파일 계약은 [storage.md](.agents/skills/ai-second-brain-study/references/storage.md)를 따릅니다. 원문의 틀린 주장도 원문 자체를 고치지 않고 별도 노트에서 정정합니다. 출처의 신뢰도와 학습자의 이해 상태도 별개입니다.

### 이해 상태의 의미

| 상태 | 의미 |
|---|---|
| `unassessed` | 이해를 판단할 실제 학습자 증거가 부족함 |
| `learning` | 설명·질문·연습 중이며 독립 이해를 아직 확인하지 못함 |
| `review-needed` | 실제 답변에서 재학습·복습이 필요한 부분이 확인됨 |
| `verified` | 해당 개념의 원리 설명과 새 상황 독립 적용을 실제 답변으로 확인함 |

힌트나 정답을 본 답변은 그 지원을 기록해야 합니다. 정답을 그대로 재현한 것은 독립 적용이 아닙니다. `verified`도 모든 주제의 숙달이나 장기 기억을 보장하지 않습니다. 구체적인 판정 규칙은 [assessment.md](.agents/skills/ai-second-brain-study/references/assessment.md)를 참고하세요.

## 5. NotebookLM으로 위계·연결 재구성하기

```text
$ai-second-brain-study
현재 학습 노트에서 NotebookLM용 sources.md와 prompts.md를 만들어줘.
개인정보와 개인 대화 원문은 제외하고, 각 주장에 출처와 불확실성을 표시해줘.
업로드하지 말고 로컬 내보내기만 해줘.
```

1. 생성된 출처 패키지를 직접 검토합니다. 개인 대화나 개인정보를 포함하려면 별도로 범위를 승인해야 합니다.
2. NotebookLM에서 노트북을 만들고 승인한 `sources.md` 내용을 소스로 가져갑니다. 서비스가 Markdown 파일을 받지 않는 환경에서는 지원하는 텍스트 형식으로 가져갑니다.
3. `prompts.md`의 질문을 채팅에 사용합니다. 결과를 다시 가져와 실제 원문과 비교하고 잘못된 연결·출처 밖 주장을 정정합니다.

이 스킬에는 NotebookLM 커넥터가 없습니다. **로컬 파일 생성은 업로드·노트북 생성 성공이 아닙니다.** 실제 연동 도구와 사용자 승인이 없는 한 결과는 `export-only`입니다. AI나 NotebookLM이 만든 구조만으로 이해 상태를 올리지 않습니다.

## 6. 개인정보와 공개 범위

이 저장소는 스킬 4개 문서, README, 라이선스, Git 배포 설정만 공개합니다. 원영상·자막 전체, 개인 학습 노트, 개발 평가 산출물은 배포하지 않습니다.

- 저장소의 `.gitignore`는 배포 파일을 **허용 목록**으로 관리하여 나머지 파일을 기본 제외합니다. 강제 추가나 외부 서비스 업로드까지 막는 보안 장치는 아닙니다.
- 스킬을 다른 프로젝트에 복사하면 이 `.gitignore`가 따라가지 않습니다. 그 프로젝트에서 `learning/` 또는 자신의 노트 경로를 공개 대상에서 제외하세요.
- 외부 도구에 보낼 내용은 직접 검토하세요. 학습 파일 저장 권한과 외부 업로드 승인은 별개입니다.
- 문제를 보고할 때는 에이전트명·버전, 익명화한 입력, 기대 결과와 실제 결과만 공유하세요. 개인 대화 원문이나 API 키는 올리지 마세요.

## 출처와 한계

방법의 출발점: [MIT 출신 CEO의 AI 독학법, 직접 해보니 기초 없던 수학 A+](https://www.youtube.com/watch?v=aBu09M0Cov4), 평균밖성원 | AI 시대를 사는 법.

영상의 핵심은 충분한 AI 대화, 개인화된 세컨드 브레인, 개념의 위계·연결입니다. 이해 상태·힌트 추적·독립 적용·복습 규칙은 이를 실행하기 위해 추가한 보완 장치이며 영상의 원래 규칙으로 주장하지 않습니다. [출처와 설계 경계](.agents/skills/ai-second-brain-study/references/source-method.md)에 구분을 기록했습니다.

Flucto CLI를 이용한 원영상 다운로드·Markdown 변환은 원래 개발 과정의 자료 수집이었습니다. 배포 스킬의 실행 전제나 포함 기능이 아닙니다. 자신의 교재·질문·대화 파일로도 사용할 수 있습니다.

AI 판정은 틀릴 수 있습니다. 이 저장소는 성적 향상, 특정 점수, 장기 기억 또는 학습 효과를 보장하지 않습니다.

## 라이선스

[MIT](LICENSE). 원영상과 외부 자료의 권리는 해당 권리자에게 있으며 이 라이선스로 이전되지 않습니다.
