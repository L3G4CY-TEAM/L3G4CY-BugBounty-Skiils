# 기여 가이드라인

L3G4CY Bug Bounty Skills 저장소에 기여해주셔서 감사합니다.
이 가이드를 따라 Skill을 추가하거나 수정할 수 있습니다.

---

## 1. 시작하기

저장소를 Fork하고 로컬에 클론합니다.

```bash
gh repo fork L3G4CY-TEAM/L3G4CY-BugBounty-Skiils --clone
cd L3G4CY-BugBounty-Skiils
```

> `gh` CLI가 없다면 [GitHub CLI 설치 가이드](https://cli.github.com/)를 참고하세요.

---

## 2. Skill 생성 절차

### 1단계: 자신의 디렉토리 및 Skill 폴더 생성

```bash
mkdir -p skills/{github-username}/{skill-name}
```

예시:

```bash
mkdir -p skills/lrtk/xss-hunter
```

### 2단계: SKILL.md 작성

`skills/{github-username}/{skill-name}/SKILL.md` 파일을 만듭니다.
작성 방법은 `templates/skill-template/SKILL.md`를 참고하세요.

```
skills/
└── lrtk/
    └── xss-hunter/
        └── SKILL.md        ← 필수
```

### 3단계: 선택 파일 추가 (필요한 경우)

| 디렉토리 | 용도 |
|----------|------|
| `scripts/` | 자동화 스크립트 (Python, Bash 등) |
| `references/` | 참고 자료, 치트시트 |
| `assets/` | 페이로드, 워드리스트, 보고서 템플릿 등 |

```
skills/
└── lrtk/
    └── xss-hunter/
        ├── SKILL.md
        ├── scripts/
        │   ├── scanner.py
        │   └── requirements.txt
        ├── references/
        │   └── cheatsheet.md
        └── assets/
            └── xss_payloads.txt
```

> Python 스크립트에 외부 패키지가 필요하면 `requirements.txt`를 함께 추가하세요.

---

## 3. 네이밍 규칙

| 대상 | 규칙 | 예시 |
|------|------|------|
| 팀원 디렉토리 | GitHub username 그대로 (대소문자 유지) | `lrtk`, `MyUser123` |
| Skill 폴더명 | kebab-case (소문자 + 숫자 + 하이픈) | `xss-hunter`, `subdomain-recon` |
| `SKILL.md` `name` 필드 | 폴더명과 동일 | `xss-hunter` |

### `name` 필드 상세 규칙

- 1~64자
- 소문자, 숫자, 하이픈(`-`)만 허용
- 하이픈으로 시작하거나 끝날 수 없음 (`-xss`, `xss-` 불가)
- 연속 하이픈 금지 (`xss--hunter` 불가)

---

## 4. Pull Request 가이드

### PR 제목 형식

```
[{github-username}] {skill-name} 추가/수정
```

예시: `[lrtk] xss-hunter 추가`

### PR 본문

Skill이 어떤 역할을 하는지 간단히 설명하면 충분합니다.

### 머지 규칙

| 상황 | 처리 방식 |
|------|----------|
| 자신의 디렉토리만 수정 + GitHub Actions 통과 | **셀프 머지 허용** |
| 다른 사람의 디렉토리 수정 | Actions에서 자동 차단 |
| 공통 영역 수정 (README, `.github/` 등) | 관리자 리뷰 필수 |

> **반드시 Fork에서 PR을 보내야 합니다.** 원본 저장소에서 직접 브랜치를 만들어 PR하는 것은 Actions에서 차단됩니다.

---

## 5. Upstream 동기화

원본 저장소에 업데이트가 있을 때 내 Fork를 최신 상태로 유지하는 방법입니다.

```bash
git remote add upstream https://github.com/L3G4CY-TEAM/L3G4CY-BugBounty-Skiils.git
git fetch upstream
git merge upstream/main
```

처음 한 번만 `remote add`를 실행하면 이후에는 `fetch` + `merge`만 반복하면 됩니다.

---

## 6. 품질 기준 (권장)

1. **자체 완결성** - Skill 폴더 하나만 복사해도 바로 사용 가능해야 합니다.
2. **설명 포함** - 무엇을 하는 Skill인지, 어떻게 사용하는지 설명이 있어야 합니다.
3. **작성자 표시** - `metadata`에 `author` 필드를 포함하세요.
4. **실사용 검증** - 최소 1회 이상 실제로 사용해본 Skill을 공유하세요.
5. **의존성 명시** - 스크립트에 외부 패키지가 필요하면 `requirements.txt` 등을 포함하세요.

---

## 7. SKILL.md 제약 조건 요약

GitHub Actions가 다음 항목을 자동으로 검증합니다. PR 전에 미리 확인하세요.

| 항목 | 제약 조건 |
|------|----------|
| `name` | 1~64자, 소문자+숫자+하이픈, 폴더명과 동일, 시작/끝 하이픈 금지, 연속 하이픈 금지 |
| `description` | 1~1,024자, 비어있으면 안 됨 |
| `tags` | 쉼표로 구분된 **문자열**로 작성 (YAML 배열 금지) |
| SKILL.md 줄 수 | **500줄 이하** |
| SKILL.md 본문 크기 | **5,000 토큰 이하 권장** |
| `assets/` 파일 크기 | 개별 파일 5MB 이하 |
| 폴더 구조 | `scripts/`, `references/`, `assets/` 외 비표준 폴더는 경고 |

> **tags 형식 주의:**
> ```yaml
> # O (올바른 형식)
> tags: "xss, web, automated"
>
> # X (YAML 배열 — 사용 금지)
> tags: [xss, web, automated]
> ```

> **왜 500줄 / 5,000토큰인가?** LLM은 Skill 외에도 시스템 프롬프트, 대화 이력 등과 context window를 공유합니다. Skill이 길수록 실제 작업 공간이 줄어들어 에이전트 성능이 저하됩니다. 긴 내용은 `references/`로 분리하세요.

### 보안 필수 사항

- API 키, 비밀번호, 토큰 등 민감 정보 하드코딩 금지
- 실제 도메인, URL, IP 주소를 Skill에 포함하지 말 것 — 예시에는 `example.com` 사용
- 컴파일된 바이너리, 난독화된 코드 업로드 금지
