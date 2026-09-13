# 2단계 — TOPIK {{BAND_NAME}} {{UNIT}}권 작성

너는 영어권 학습자를 위한 TOPIK 어휘 교재 저자다. 아래 **배정된 100개 표제어만** 사용해 앱이 읽는 단어책 파일 하나를
`{{TARGET_PATH}}`에 쓴다. 표제어를 바꾸거나 빼거나 더하지 않는다 — 다른 권과의 중복은 이미 제거되어 있다.
앱의 화면 언어는 **영어**다. 뜻·TIP·번역은 모두 자연스러운 미국 영어로 쓴다.

## 이 권
- band_id `{{BAND_ID}}` ({{BAND_LABEL}}) · unit {{UNIT}} · theme: {{THEME}}
- 급 성격: {{BAND_BRIEF}}

## 파일 형식 — 한 글자도 다르지 않게
```
---
id: {{UNIT_ID}}
type: voca
level: {{LEVEL}}
difficulty: {{DIFFICULTY}}
tags: [{{TAGS}}, unit-{{UNIT}}]
source: {{SOURCE}}
version: 1
updated_at: {{DATE}}T00:00:00Z
score_band_id: {{BAND_ID}}
score_min: {{SCORE_MIN}}
score_max: {{SCORE_MAX}}
---

# {{BAND_NAME}} · Book {{UNIT}} — {{THEME}}

- 표제어 | English meaning | romanization | TIP | 한국어 예문 | English translation
```
단어 줄은 배정 순서대로 **정확히 100줄**, 필드 6개, 구분자는 ` | `. 어떤 필드에도 `|`를 추가로 쓰지 않는다.

## 카드 구성 — 앞면은 한글 표제어만, 뒷면은 표제어·romanization·뜻·TIP·예문·번역
1. **표제어**: 배정표 그대로.
2. **English meaning**: the sense TOPIK tests, at most two, comma-separated. Verbs/adjectives start with "to": `to overcome`, `to be serious, severe`.
3. **romanization**: 배정표 그대로 (Revised Romanization).
4. **TIP** (English, ≤ 90 characters) — one line that makes the word stick. Pick the most useful of:
   - Hanja root: `發展: growth of skills, tech or a country` · `부 (副) = secondary + 작용 (effect)`
   - Pattern / particle: `Takes 에: 행사에 참여하다` · `Always with a negative: 비단 ~만의 문제가 아니다`
   - Pair or contrast: `줄다 (decrease) ↔ 줄이다 (reduce)` · `지양 (avoid) vs 지향 (aim for)`
   - Pronunciation trap: `Pronounced 트키 — the ㄱ and ㅎ merge`
   - Register: `Formal — news and essays; say 돕다 in conversation`
   Never restate the meaning.
5. **한국어 예문**: natural sentence at this level's grammar, 8–40 syllables. Polite 해요체 for levels 1–3, formal 합니다체 or written style for 4–6.
   The headword must appear (conjugated is fine).
6. **English translation**: natural, not word-for-word.

## 배정된 표제어 (이 순서대로 100줄)
{{HEADWORDS}}

## 저장 전 스스로 확인
① 100줄 ② 모든 줄 6필드 ③ 표제어·romanization이 배정표와 같음 ④ 뜻·TIP·번역이 영어 ⑤ TIP 90자 이내 ⑥ 예문에 표제어가 들어 있음.
저장 후 `python3 tools/validate_content.py --unit {{BAND_ID}}/{{UNIT}}` 가 0 errors가 될 때까지 해당 줄만 고친다.
