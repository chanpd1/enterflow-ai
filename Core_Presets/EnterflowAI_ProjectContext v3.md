---
[Storage Path Configuration]
- Project Root: /00_Enterflow AI/
- Core Database (GitHub Sync): ./EnterflowAI_Storage/Core_Presets/
- User Assets (Local Storage): ./EnterflowAI_Storage/User_Assets/
- Asset Mapping: 
  - PREVIEWS: Core_Presets/camera_rigs/previews/
  - RIGS_BASIC: Core_Presets/camera_rigs/rigs_basic/
  - RIGS_CUSTOM: Core_Presets/camera_rigs/rigs_custom/
  - CHARACTERS: User_Assets/characters/{id}/
  - BACKGROUNDS: User_Assets/backgrounds/{id}/
---

# Enterflow AI — 프로젝트 컨텍스트 문서 (v3)
> 새 대화 시작 시 이 파일을 첨부하면 Claude PM이 즉시 맥락을 파악합니다.
> 최종 업데이트: 2026-03-29 (v3)

너는 Enterflow AI 프로그램의 Claude PM.
수정 시 항상 JS 문법 검사, tab-database depth=2 검증,
db-section 중복 확인
납품 파일: EnterflowAI.html

# Enterflow AI: Hybrid Storage & Path Logic

## 1. Storage Overview
- **Core DB (Git):** 표준 카메라 앵글, 렌즈 세팅, 리깅 데이터베이스. (공통 배포용)
- **Local Assets (HDD):** 캐릭터 ID 카드, 배경 소스, 개인화된 고용량 이미지 데이터. (사용자 확장용)

## 2. Path Configuration (환경 변수)
- `ROOT_CORE_PATH`: `https://github.com/YourRepo/EnterflowAI_Core`
- `ROOT_LOCAL_PATH`: `C:/EnterflowAI_Storage/User_Assets`

## 3. Dynamic Asset Loading Logic
- **Character 호출:** 사용자가 캐릭터 ID(`ariin`) 선택 시
  - 경로: `{ROOT_LOCAL_PATH}/characters/{character_id}/`
  - 우선순위: `metadata.json` 읽기 -> `portrait` 폴더 로드
- **Camera 호출:** 사용자가 앵글(`low_angle`) 선택 시
  - 경로: `{ROOT_CORE_PATH}/camera_rigs/`
  - 파일 매핑: `{angle_id}_ui.jpg` & `{angle_id}_rig.png`

---

## 1. 프로그램 개요

| 항목 | 내용 |
|---|---|
| 프로그램명 | **Enterflow AI** (구: 찬피디 AI 스튜디오) |
| 파일명 | `EnterflowAI.html` (단일 HTML 파일) |
| 납품 경로 | `/mnt/user-data/outputs/EnterflowAI.html` |
| 백업 기준 | `chanpd_ai_studio_v33.html` |
| 총 줄 수 | 약 5,100줄 |
| 아키텍처 | 단일 HTML + Vanilla JS + localStorage |

### 목적
카메라 앵글, 캐릭터 표정, 행동, 상황 등 간단한 장면 설명만 입력하면 database의 고도화된 프롬프트를 참고하고 라이브러리 속 레퍼런스 이미지를 활용해서 Claude PM × Gemini 검수를 통해 **Nano Banana 2 (Imagen 4)** 이미지 생성으로 영화, MV의 한 장면을 만드는 과정을 자동화하는 AI 이미지, 영상 제작 프롬프트 생성 도구.

### 현재 목표 (Phase 3)
- 간단한 상황 설명 → 사전 저장된 database를 활용해 미드저니, 힉스필드급 퀄리티로 완성된 이미지 자동 생성
- Prompt STEP 1~5 백그라운드 처리, 최종 이미지만 노출
- 프로그램 자체에서 레퍼런스 이미지를 **Nano Banana 2 (Imagen 4)** upload하고 다운로드 받을 수 있는 기능 추가.
- image scale up 기능 추가.
- Create Character sheet Step1 배우(캐릭터)를 등록하거나 생성합니다. 이때 각 배우들에 대한 생김새, 역할, 피지컬 등 프롬프트도 데이터 베이스에 저장합니다. 영화, 뮤직비디오, 특정 상황에 등장하는 주인공 캐릭터의 무표정 초상화(Portrait)를 만듭니다.
- Create Character sheet Step2 전방위 얼굴 초상화를 만듭니다. 최소 10개(정면부터 좌우측면, 좌우옆면 등), Sheet A Master에 들어갈 가장 중립적인 조명과 골격, 비율이 되는 얼굴 레퍼런스 이미지의 표본을 만듭니다.
- Create Character sheet Step3-1. 배우의 바디 프로필 만들기. 배우의 정면 얼굴과 프롬프트를 통해 바디 피지컬을 세팅합니다. 신체 실루엣이 잘 드러나는 레깅스와 타이트한 반팔, 스포츠 브라와 레깅스를 입힌 바디 프로필을 만듭니다.
- Create Character sheet Step3-2. 배우의 일상복, 연출복 만들기. 의상 정보만 담긴 Lookbook img를 통해 배우에게 의상을 입히고 전후좌우 모습을 만듭니다. 이때 의상에 대한 프롬프트를 데이터 베이스에 등록합니다.
- Create Character sheet Step4. Sheet B 바리에이션 데이터 생성. Sheet A Master를 이용해 배우 특유의 표정들을 만듭니다. 웃는 모습, 우는 모습, 화난 모습 등, 이 배우 만의 특징을 적용한 결과물을 정리하고 역할에 따른 동작들을 만듭니다.
- Create Character sheet Step5. Sheet C 라이팅 데이터 생성. 배우가 어두운 곳에 있을 때, 연출된 조명에 있을 때, 밝은 곳에 있을 때, 패션 잡지 같은 사진 등 다양한 빛의 상황 속 모습을 담습니다.
- Create Character sheet Step6. 위의 과정을 간단한 프롬프트 지시만으로도 만들 수 있도록 Sys에 저장하여 자동화 시스템을 만듭니다. 각 Step 별로 감독(사용자)는 퀄리티 검수를 할 수 있지만 오토(Auto) 모드도 존재합니다.
- Create Character sheet Step7. 위의 과정을 통해 생성을 한 배우, 조연들을 일괄 진행하여 라이브러리에 모두 저장하고 영화, 뮤직비디오 장소가 될 배경을 만듭니다. 배경을 묘사하는 프롬프트와 각도별 이미지를 생성한 BG Sheet를 만들고 등록합니다.
- Create Character sheet Step8. Scene을 만듭니다. 한 명 혹은 두 사람 이상이 함께 등장해 한 장면을 만듭니다. 사전에 등록했던 각 프롬프트 정보와 img Sheet를 활용해 크로드PM과 재미나이가 미드저니, 힉스필드급 일관된 배우의 생김새와 영화 같은 Look의 장면을 만들어 냅니다. 한 번에 10개의 장면을 만들 수 있으며 자동화가 가능합니다. 10개의 장면은 3개의 바리에이션을 두어 감독(사용자)가 선택할 수 있도록 합니다.
- 배우(캐릭터) 생성부터 Sheet A Master(기본), Sheet B Variation(표정&포즈), Sheet C lighting(조명 환경), Scene 제작(MV 장면)까지 고퀄리티의 이미지 생성 자동화
- Electron 설치형 앱 (.exe) 배포

### 영상 제작 생성 기능 추가 (Phase 4)
- 이미지 생성으로 영화의 한 장면을 생성하면 해당 이미지를 활용해 고퀄리티 영상 제작 자동화 하는 것이 목표
- 이미지 자동화가 완성된 후 적용되는 신기능으로 워크 플로우는 점검할 예정
- 프로그램 자체에서 레퍼런스 이미지를 **Kling**에 upload하고 영상 제작 후 다운로드 받을 수 있는 기능 추가.
- Create Character sheet Step9. Video&Movie를 만듭니다. 위 과정을 통해 얻은 각 장면들을 가지고 클로드PM과 클링의 협업으로 적합한 프롬프트를 생성해 영상화 합니다. 한 번에 장면을 모두 등록하고 장면 설명을 넣어 순차적으로 영상화 진행이 가능합니다.
- **Fallback 로직:** 직접 API 연동 불가 시, 영상 소스 경로 및 연출 가이드를 담은 별도 문서(txt/md) 자동 생성 기능 지원.

### 편집 프로그램 연동, 자동 정리 기능 추가 (Phase 5)
- 어도비 프리미어 MCP 연결 기능 추가
- Create Character sheet Step10. 프리미어 편집 작업 및 랜더링. MCP 연결을 통해 어도비 프리미어와 연동하여 클로드가 클링으로 만든 영상을 프리미어에 가져와 씬의 순서별로 나열하고 정해둔 뮤비 음악을 넣어 정리합니다.
- **XML 또는 EDL 파일 출력** 및 타임라인 구성 지원.

---

## 2. 브랜딩 & 디자인 시스템

```css
/* 컬러 */
--bg:  #0d0d14
--sf:  #12121a  (서피스)
--sf2: #1a1a26
--sf3: #20202e
--bd:  #2a2a3e  (보더)
--ac:  #4956ff  (포인트 - 파란계열)
--tx:  #e8e8f0  (텍스트)
--mu:  #6b6b8a  (뮤트)
--ok:  #34d399
--wn:  #fbbf24
--er:  #f87171
```

- 버튼: 단색 심플 (그라데이션 금지)
- 탭: 크롬 스타일 (활성 탭이 배경에서 튀어나오는 형태)
- 섹션 박스: `.ls` 스타일 — `border-radius:8px`, `border:1px solid var(--bd)`
- 로고: EF 이미지 (base64 내장) + "Enterflow AI" 텍스트

---

## 3. 전체 탭 구조

```
[프롬프트 생성] [이미지 등록] [Scene 만들기] [Setting]
  tab-pipeline   tab-library   tab-batch     tab-database
```

### ⚠️ 핵심 구조 규칙
**모든 탭은 `.ct` div 안에서 depth=2여야 함**
- 패치 후 반드시 이진탐색으로 `tab-database depth=2` 검증
- db-section 5개 각 1회씩만 존재해야 함

---

## 4. 탭별 상세

### 4-1. 프롬프트 생성 탭 (tab-pipeline)

**서브모드 북마크 바** (tab-pipeline 직속):
```
[프롬프트 연구] [캐릭터 연구] [BG img 분석] [장면 합성] [이미지 분석]
id="modeBookmarkBar"
```

**AI 파이프라인 (STEP 1~5):**
```
STEP 1: Claude PM 1차 프롬프트 (sys1 + allImgs)
STEP 2: Gemini 검수 (gemini-2.5-flash)
STEP 3: Claude 보강 → s3Final (앵글 JS 후처리)
STEP 4: Claude 더블체크 → ensureAngle → 영문/한글 분리
STEP 5: 저장 & 기록
```

**sys1 주입 구조:**
```javascript
const dbCtx = getDbContext();  // 데이터베이스 탭 데이터
const sys1 = `...
${dbCtx ? `=== 📚 AI 지식 데이터베이스 ===\n${dbCtx}` : ''}
${presets ? `=== 캐릭터 프리셋 ===\n${presets}` : '⚠️ 프리셋 없음'}
=== 적용 템플릿 구조 ===
${tpl[mode]}...
`;
```

**width 규칙:** `.ta`, `.ps`, `#actionBar` 모두 `max-width:950px`, `border-radius:8px`

### 4-2. 이미지 등록 탭 (tab-library)

내부 서브탭 (`.lw` 안):
```
Master ID Sheet / Character Sheet / Lookbook Sheet /
BG Sheet / Cam Angle & Shot Ref / Film Look
```

**lib 객체 구조:**
```javascript
lib = {
  chars: [],   // 캐릭터 (슬롯: ff/fs/hl/hr/bl/br)
  lbs:   [],   // Lookbook
  mss:   [],   // Master ID Sheet
  bgs:   [],   // BG Sheet
  angles:[],   // Cam Angle (그룹 구조)
  films: []    // Film Look (localStorage: chanpd_films 별도)
}
```

### 4-3. Scene 만들기 탭 (tab-batch)

**레이아웃:**
```
[왼쪽: 씬 콘티 목록 260px] | [오른쪽: TAKE 편집 + 결과]
```

**⚠️ 구조 규칙:**
- 왼쪽(260px) div가 먼저 닫힌 후 오른쪽 패널이 시작되어야 함
- 오른쪽이 왼쪽 안에 중첩되면 TAKE 편집이 아래로 들어가는 버그 발생

**TAKE 객체 구조:**
```javascript
{
  id, title, scene, filmId, angleId,
  refItems: [], prompt: '', image: null,
  status: 'pending', score: null, keepDecision: null
}
```

**TAKE 편집 섹션 (`#takeEditWrap`):**
- Film Look 선택 (세로 배치)
- 카메라 앵글 선택 (커스텀 픽커)
- 레퍼런스 선택 (카테고리 드롭다운)
- 이미지 미리보기 (`max-height:220px`)

### 4-4. Setting 탭 (tab-database)

**5개 카테고리:**
```
🧑 Character Sheet data      → characters
📸 Camera Shot & Angle data  → camera
🎞️ Film Look data            → filmLook
✨ Skin_Texture data         → skin
🎨 Hex Color Palette data    → hexPalette
```

**형식 검증 (DB_FORMAT_KEYS):**
```javascript
characters: ['Age:', 'Hair:']
camera:     ['Prompt:']
filmLook:   ['===']
skin:       ['===']
hexPalette: null  // 제한 없음 (어떤 txt든 허용)
```

**드래그앤드롭 업로드 지원**
**dbData는 JSON 백업/복원에 포함됨** (buildBackupPayload/applyImportData)

---

## 5. 전역 변수 (INIT 이전 선언)

```javascript
var selAngle = null
var selectedTakes = new Set()
var takes = []
var curTakeIdx = -1
var transContainer = null
var transSegs = []
var transMode = 'en'
var lastFinalPrompt = ''
var dbData = {
  characters: [], camera: [], filmLook: [], skin: [], hexPalette: []
}
var _histMinimized = false
var _histMaxRect = null
var _histMinRect = null
var batchRunning = false
```

---

## 6. localStorage 키

```javascript
const KEYS = {
  lib:      'chanpd_lib',
  hist:     'chanpd_hist',
  ver:      'chanpd_ver',
  saveP:    'chanpd_saveP',
  saveFile: 'chanpd_saveFile',
  histFile: 'chanpd_histFile',
  films:    'chanpd_films',  // Film Look 별도 저장
  db:       'chanpd_db'      // 데이터베이스
}
// + 'chanpd_takes' (배치 TAKE)
```

---

## 7. 네비게이션 바

```
[파일] [편집] [프롬프트] [스토리지] [설정]
```

- `파일 → 저장`: `exportSave()` / Ctrl+S
- `파일 → 다른 이름으로 저장`: `exportSaveAs()` / Ctrl+Shift+S
- 저장 파일명: `EnterflowAI_database_날짜.json`
- `설정 → API 연결`: `openApiModal()` → 실제 input 모달 팝업
- `설정 → 히스토리`: ON/OFF 토글 + 히스토리 창 열기

---

## 8. 히스토리 플로팅 패널

- 드래그 이동 가능 (`makeDraggable`)
- 최소화: `histInnerWrap` 숨기고 `height:auto` (헤더만 표시)
- 최대화: 90vw × 85vh
- 버튼: `─ □ ✕` (투명 배경 심플 스타일)
- 탭: 📋 히스토리 / ⭐ 검증된 프롬프트

---

## 9. 이미지 생성 (Imagen)

```javascript
// ListModels API로 사용 가능 모델 동적 조회
// safetySetting: 'block_low_and_above'
// 현재 키: imagen-4.0 계열만 지원
// referenceImages 파라미터 시도 포함
```

---

## 10. 캐릭터 데이터

## 10. 캐릭터 데이터 (Character Identity)

> ⚠️ [AI SYSTEM COMMAND: CHARACTER MASTER REFERENCE]
> 캐릭터의 구체적인 프롬프트 수식 및 네거티브 프롬프트는 본 문서에 기록하지 않는다.
> 클로드 너는 Scene 만들기 및 캐릭터 관련 코드를 작성하거나 렌더링을 준비할 때, **반드시 깃허브에 동기화된 `Character_Master.md` 파일을 참조**하여 해당 캐릭터의 수식을 100% 원형 그대로 가져와야 한다. 
> 새로운 캐릭터를 DB에 추가할 때도 `Character_Master.md`의 구조(Formula 1, 2)를 표준 템플릿으로 삼아 규격화하라.

* 등록된 메인 캐릭터: Ariin (21세), Luccy (23세), 외 추가될 것.
* 상세 프롬프트 위치: `Character_Master.md` 파일 참조


---

## 11. 패치 시 필수 검수 체크리스트

```
□ JS 문법 검사 (node --check)
□ onclick 미정의 함수 없음
□ tab-database depth=2 (이진탐색 자동 보정)
□ db-section 5개 각 1회씩
□ 오른쪽 TAKE 패널이 왼쪽 260px div 밖에 위치
□ buildBackupPayload ↔ applyImportData 항목 동기화
□ 기존 함수 (addTake, saveBatchData, renderTakeList 등) 유지
□ Film Look (chanpd_films) 유지
□ selAngle 전역 선언 위치 (INIT 이전)
```

---

## 12. 개발 원칙

1. **새 기능 = 새 네임스페이스** — 기존 함수 수정 최소화
2. **패치 전 백업** — 수정 전 이전 버전 저장
3. **단계 분리** — HTML/CSS 수정 ↔ JS 로직 수정 분리
4. **depth 이진탐색** — 모든 패치 후 tab-database depth 자동 보정
5. **한 번에 처리** — 분산 패치 금지, 분석 완료 후 일괄 수정

### [시스템 명령: 업데이트 및 회귀 방지 프로토콜]
너는 지금부터 코드를 수정하거나 기능을 업데이트할 때 아래의 **'3단계 검증 루틴'**을 반드시 준수해야 한다.

#### 1단계: 영향도 분석 (Impact Analysis)
새로운 코드를 제안하기 전, 다음 질문에 답하라.
- 이 수정이 기존의 핵심 로직(특히 [A기능], [B기능])에 어떤 영향을 미치는가?
- 수정되는 함수가 의존하고 있는 다른 모듈이 있는가?

#### 2단계: 단위 테스트 및 회귀 테스트 제안
업데이트된 코드와 함께, 기존 기능이 여전히 작동함을 증명할 수 있는 '검증 코드'를 반드시 포함하라.
- **기존 기능 확인:** 기존 로직의 Input/Output이 변하지 않았음을 보여주는 테스트 케이스.
- **신규 기능 확인:** 신규 업데이트가 의도대로 작동하는지 확인하는 테스트 케이스.

#### 3단계: 점진적 배포 방식 (Side-by-Side)
기존 코드를 완전히 삭제하고 덮어쓰는 대신, 아래 방식을 우선 고려하라.
- 기존 함수는 유지하되, 새로운 버전의 함수를 생성(예: `function_v2`)하여 비교 가능하게 할 것.
- 변경 사항이 클 경우, '변경 전(Legacy)'과 '변경 후(Updated)' 코드를 나란히 배치하여 차이점을 설명할 것.

#### 4단계: 버그 발생 시 대조 및 롤백 (Diff & Rollback)
만약 네가 제안한 신규 코드를 적용한 후 기존 기능(특히 저장, UI 렌더링, API 호출 등)에 버그가 발생하거나 먹통이 된다면, 사용자에게 코드를 다시 요구하지 마라.
즉시 Project Knowledge에 동기화된 깃허브의 원본 코드(안전하게 작동하는 정답지)와 네가 방금 짠 코드를 스스로 대조(Diff)하라.
어떤 변수나 로직이 충돌했는지 정확히 파악하여 원인을 보고하고, 기존 기능이 망가지지 않도록 원상 복구 및 수정된 코드를 다시 제시하라.

**주의사항:** "더 효율적인 코드"라는 명목으로 기존의 예외 처리나 특수 케이스 로직을 임의로 삭제하지 마라.

---

## 13. 개발 로드맵

### Phase 1 (진행 중)
- ✅ 데이터베이스 탭 (5개 카테고리)
- ✅ Hex Color Palette 섹션
- ✅ character_base.txt 작성
- ✅ 네비게이션 바
- ✅ 히스토리 플로팅 패널
- ✅ 브랜딩 Enterflow AI + #4956ff
- 🔄 프롬프트 품질 고도화
- 🔄 txt 데이터 채우기 (홀리가 매일 Hex Palette 10개)

### Phase 2 (기능 안정화 후)
- Anchor Face 기반 캐릭터 시트 반자동 생성
- TAKE 50개 txt 일괄 업로드 → 배치 자동 실행
- Electron 변환 + 폴더 구조
- 자동 업데이트 (electron-updater)
- .exe 인스톨러

```text
Documents/Enterflow AI/
├── config/        (settings.json, library.json)
├── database/      (txt 파일 직접 편집 가능)
├── projects/      (프로젝트별 분리)
├── history/
└── auto-save/
```

### Phase 3 (1차 완성 단계)
- STEP 1~5 백그라운드 처리
- 최종 이미지만 노출 (간소화 UI)
- 다른 사람 배포 가능한 완성본
- 상위 버전 .exe로 업데이트

### Phase 4 영상 제작 생성 기능 추가
- 생성한 이미지를 활용해 클로드PM과 클링의 협업으로 Video&Movie 제작 프롬프트 생성
- 프로그램 자체에서 레퍼런스 이미지를 **Kling**에 upload하고 영상 제작 후 다운로드 받을 수 있는 기능 추가.
- Scene 만들기에 추가되는 섹션, [Scene 만들기] 내에 image/video 스위칭 아이콘 생성, 한 번에 장면을 모두 등록하고 장면 설명을 넣어 순차적으로 영상화 진행

### Phase 5 편집 프로그램 연동, 자동 정리 기능 추가
- 어도비 프리미어 MCP 연결 기능 추가
- 어도비 프리미어에 영상, Scene 순서에 맞게 올려 정리하고 bgm 얹어놓기

---

## 14. AI 팀 구성

| 역할 | AI | 역할 설명 |
|---|---|---|
| 총 감독/PD | 찬피디 | 방향 결정, 피드백 |
| 수석 프롬프트 PM | Claude PM | 프롬프트 생성, 코드 개발, 이미지&프롬프트 데이터 베이스 고도화 |
| 검수 비서 | 홀리 (Gemini) | 이미지&프롬프트 검수, 데이터 제공, **제공 데이터 규격 준수 1차 필터링** |
| 이미지 엔진 | Nano Banana 2 (Imagen 4) | 실제 이미지 생성 |
| 영상 엔진 | Kling | 실제 영상 생성 |

---

*이 파일을 Claude Projects에 등록하거나 새 대화 시작 시 첨부하면 맥락을 빠르게 복원할 수 있습니다.*