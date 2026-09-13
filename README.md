# SamGongGong Content — TOPIK Korean vocabulary content

> **SamGongGong(삼공공)** — TOPIK II 만점 300에서 따온 이름. 앱 화면 언어는 영어(미국 학습자 대상).
> [NINE90](https://github.com/jsonpassion/NINE90)(TOEIC 트랙)과 동일한 콘텐츠 파이프라인을 쓰는
> TOPIK 트랙 리포지토리 — 앱은 manifest URL 하나로 이 리포의 콘텐츠를 통째로 동기화합니다.

**현재 상태: 규격·도구·생성 파이프라인만 존재.** 단어는 [OVERNIGHT.md](OVERNIGHT.md) 절차로 밤새 병렬 생성한다.

## 구조

```
content.config.json               ← 트랙 규격: 밴드·권 수·언어·표기 (도구가 모두 이것을 읽는다)
plan/curriculum.json              ← 밴드별 10권 테마
prompts/wordlist.md               ← 1단계: 밴드별 후보 표제어 프롬프트
prompts/unit.md                   ← 2단계: 배정된 100단어로 권 파일 쓰기 프롬프트
tools/plan.py                     ← 후보 병합·전역 중복 제거·100개 배정·brief 생성·todo
tools/validate_content.py         ← 형식·표기·중복·배정 일치 검증 (0 errors 필수)
tools/build_manifest.py           ← manifest.json 생성
content/voca/{band}/unit-NNN.md   ← 1파일 = 1권 = 100단어 (10단어 = 1챕터)
OVERNIGHT.md                      ← 밤샘 병렬 생성 런북 + 붙여넣기용 오케스트레이션 프롬프트
```

## 급 (TOPIK I = 1–2급 200점, TOPIK II = 3–6급 300점)

| band_id | 급 | 기준 점수 | 권 수 | 성격 |
|---|---|---|---|---|
| `topik-1` | Level 1 · Beginner | 80/200 | 8 | 생존 한국어 |
| `topik-2` | Level 2 · Elementary | 140/200 | 8 | 일상·공공장소 |
| `topik-3` | Level 3 · Intermediate | 120/300 | 10 | 대학 입학 기준, 한자어·하다 동사 |
| `topik-4` | Level 4 · Upper-int. | 150/300 | 10 | 뉴스·직장·사회 |
| `topik-5` | Level 5 · Advanced | 190/300 | 12 | 학술·전문 |
| `topik-6` | Level 6 · Mastery | 230/300 | 12 | 사설·연구·사자성어 |

앱은 `score_max`(200/300)를 급마다 쓰고, 기준 점수는 앱 Track.json의 `passMark`가 갖는다.

## 단어 줄 형식 — 앞면은 한글만, 로마자·뜻·TIP은 뒷면(영어)

```
- 표제어 | English meaning | romanization | English TIP | 한국어 예문 | English translation
```

샘플: [content/voca/topik-3/unit-001.md](content/voca/topik-3/unit-001.md) — 앱 확인용 더미(급당 20단어, `dummy: true`).
본 생성 전에 `python3 tools/plan.py clear-dummy`로 지운다.

## 콘텐츠 규칙

- 유닛당 **정확히 100단어**, 10단어 = 1챕터 (앱의 회독 단위)
- **트랙 전체에서 표제어 중복 = 오류** (급이 달라도 같은 단어는 한 번만)
- 필드 안에 파이프(`|`) 금지 (구분자 전용)
- 카드 ID = `{파일 id}-{표제어 slug}` — 줄 순서와 무관하지만, **출시 후 표제어 철자 변경·삭제는 금지**
  (사용자 학습 진도가 카드 ID에 매여 있음). 추가는 새 유닛 파일로.
- `manifest.json`의 `profile.free_chapters`(기본 10 = 1권) = 밴드마다 무료로 열리는 챕터 수
  (앱이 원격 설정으로 읽음)

## 워크플로

단어 생성은 [OVERNIGHT.md](OVERNIGHT.md) 한 곳에 정리돼 있다 (후보 목록 → 전역 중복 제거·배정 → 권별 병렬 작성 → 검증).

```bash
python3 tools/plan.py status         # 진행 상황
python3 tools/validate_content.py    # 0 errors 필수
python3 tools/build_manifest.py
git add content plan manifest.json && git commit && git push
```

앱은 raw.githubusercontent.com의 manifest.json 버전 변경을 감지해 바뀐 파일만 내려받습니다
(sha256 검증 포함). raw CDN 캐시 특성상 push 후 매니페스트 반영까지 ~5분 걸릴 수 있습니다.

## 상표 고지

한국어능력시험(TOPIK)은 대한민국 국립국제교육원이 주관하는 시험입니다. 이 리포지토리와 관련 앱은
국립국제교육원와 무관하며, 국립국제교육원의 제휴·보증·승인을 받지 않았습니다. 모든 콘텐츠는 자체 제작이며
실제 기출문제를 포함하지 않습니다.

© 2026 ForgeLab
