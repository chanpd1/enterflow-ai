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
- Cloud Storage (R2): https://9a0bffda4d9d3aec42484a0528fc30cf.r2.cloudflarestorage.com
- R2 Bucket: enterflowaistorage
- R2 Public URL: https://pub-edc7a49eee184ce8b51a5ca4ee8fa134.r2.dev
- R2_MAPPING: r2_manager.py를 통해 metadata.json → assets_tree.md 순으로 인덱싱 스캔

---

# Enterflow AI — 프로젝트 컨텍스트 문서 (v6)
> 새 대화 시작 시 이 파일을 첨부하면 Claude PM이 즉시 맥락을 파악합니다.
> 최종 업데이트: 2026-03-30 (v5 - 감정 DB 통합 및 로드맵 복구)

너는 Enterflow AI 프로그램의 Claude PM.
수정 시 항상 JS 문법 검사, tab-database depth=2 검증, db-section 중복 확인.
납품 파일: EnterflowAI.html

# Enterflow AI: Hybrid Storage & Path Logic

## 1. Storage Overview
- **Core DB (Git):** 표준 카메라 앵글, 렌즈 세팅, 리깅 데이터베이스, **감정 프롬프트 사전(Emotions)**. (공통 배포용)
- **Local Assets (HDD):** 캐릭터 ID 카드, 배경 소스, 개인화된 고용량 이미지 데이터. (사용자 확장용)

## 2. Path Configuration (환경 변수)
- `ROOT_CORE_PATH`: `https://github.com/YourRepo/EnterflowAI_Core`
- `ROOT_LOCAL_PATH`: `C:/EnterflowAI_Storage/User_Assets`

## 3. Dynamic Asset Loading Logic
**Character & Emotion 호출:** 사용자가 캐릭터 ID 및 특정 감정 선택 시
- 1순위 (Cloud): r2_manager.py를 호출하여 R2 내 User_Assets/ 및 Core_Presets/emotions/ 경로의 데이터 파싱.
- 감정 매핑: `tab-database`의 `emotions` 섹션에 저장된 프롬프트 수식을 최우선 참조.
- 이미지 참조: 나노바나나 전달 시 R2의 Public Development URL을 최우선으로 매핑.

---

## 4. 탭별 상세

### 4-1. 프롬프트 생성 탭 (tab-pipeline)
**sys1 주입 구조 (감정 데이터 결합):**
```javascript
const dbCtx = getDbContext();  // 캐릭터, 카메라, 감정, 스킨 등 포함
const sys1 = `...
${dbCtx ? `=== 📚 AI 지식 데이터베이스 (감정/표정 포함) ===\n${dbCtx}` : ''}
=== 적용 템플릿 구조 ===
캐릭터의 현재 감정(Emotions) 섹션의 수식을 최우선으로 적용할 것.
`;
```

### 4-4. Setting 탭 (tab-database)
**6개 카테고리 (확장됨):**
1. `characters`: 🧑 Character Sheet data
2. `camera`: 📸 Camera Shot & Angle data (Dutch, ECU, Bust 등 포함)
3. `filmLook`: 🎞️ Film Look data
4. `skin`: ✨ Skin_Texture data
5. `emotions`: 😊 Emotional Expressions data (추가됨)
6. `hexPalette`: 🎨 Hex Color Palette data

---

## 5. 전역 변수 (INIT 이전 선언)
```javascript
var selAngle = null
var selectedTakes = new Set()
var takes = []
var curTakeIdx = -1
var dbData = {
  characters: [], camera: [], filmLook: [], skin: [], emotions: [], hexPalette: []
}
```

---

## 10. 캐릭터 및 감정 데이터 (Identity & Expression)

### 10-1. 카메라 앵글 마스터 (Camera Master) [UPDATE]
* **Dutch Angle**: `(dutch angle:1.5), (tilted camera:1.4), (dynamic composition:1.4)`
* **Ultra Extreme Close-up**: `(ultra extreme close-up:1.8), (macro photography:1.5), (hyper-detailed skin texture:1.6)`
* **Bust Shot**: `(bust shot:1.6), (portrait:1.5), (upper body:1.3)`

### 10-2. 감정 및 표정 데이터 (Emotional Expressions) [NEW]
> 캐릭터의 생동감을 결정하는 핵심 요소. 클로드 PM은 `emotions` DB를 파싱하여 최적의 프롬프트를 주입한다.

* **데이터 분류 명세:**
    * **Base:** Neutral(기준), Relaxed(이완), Focused(집중)
    * **Smiles:** Subtle Micro-smile(호감), Radiant(화사), Welcoming(환대), Amused(즐거움), Bashful(수줍음)
    * **Editorial:** Pure(청순/무해), Sentimental(서정), Elegant(우아), Dreamy(몽환), Serene(평온)
    * **Dramatic:** Wistful(우수에 젖음), Soft Allure(매혹), Attentive(경청)
* **프롬프트 결합 규칙:** `(Emotion_Keyword:Weight)` 형식을 유지하며 눈빛(eyes), 입술(lips), 피부 질감(skin texture) 묘사를 포함한다.

---

## 11. 패치 시 필수 검수 체크리스트
- □ `tab-database` 내 `emotions` 섹션 5번째 항목으로 정상 렌더링 확인
- □ `dbData.emotions` 배열 초기화 및 localStorage(`chanpd_db`) 저장 로직 확인
- □ `sys1` 생성 시 감정 데이터베이스 내용을 파싱하여 프롬프트에 반영하는지 검증

---

## 12. R2 에셋 파이프라인 (2026-03-29 업데이트)
- **에셋 로드 방식:** `metadata.json` 인덱싱 기반의 '핀포인트 로드'.
- **이미지 참조:** 나노바나나 생성 시 R2 Public URL을 Image Reference로 직접 주입.

## 13. Global Cinematic Engine (글로벌 시네마틱 렌더링 규격) [NEW]
> 모든 이미지 생성 시 '홀리'가 반드시 준수해야 하는 최상위 렌더링 가이드라인.

### 13-1. [Global Guidelines - Cinematic Excellence]
* **Default Engine:** "Masterpiece, professional cinematic film still, Hollywood production quality, 8k resolution, raw photo, high dynamic range."
* **Lens & Lighting:** "85mm anamorphic lens (T1.5), distinctive horizontal flares, oval bokeh, professional movie lighting (soft key & subtle rim light)."
* **Texture Logic:** "35mm film grain, organic noise, no digital over-sharpening, natural contrast curve (Arri Alexa/Sony VENICE style)."

### 13-2. [Skin & Aesthetic Standards]
* **Skin Integrity:** "Realistic skin texture (pores/micro-details) + Clean Skin finish (zero blemish/imperfections). Balance between microscopic detail and flawless production quality."
* **Shadow/Color Control:** "Lifted blacks, crushed shadows with a greenish/teal hint, cinematic color science (Non-digital look)."

### 13-3. [Quality Guardrail - Negative Prompt]
* **Forbidden Elements:** "plastic skin, oversaturated, over-sharpened, smooth skin filter, digital noise, mobile phone quality, watermark, distorted facial features."

### 13-4. [Prompt Assembly Logic]
1. **Base:** Section 12-1의 글로벌 규격을 최우선으로 로드.
2. **Character:** `Character base data`의 고정값 적용.
3. **Style:** `FilmLook` 및 `Hex_Palette` 파싱 데이터 결합.
4. **Skin:** `Skin_Texture` 데이터와 Section 12-2의 'Clean Skin' 로직 결합.

---

## 14. Phase 2 (기능 안정화 후)
- Anchor Face 기반 캐릭터 시트 반자동 생성 및 TAKE 50개 배치 실행.
- Electron 변환 + 폴더 구조 정의 및 `.exe` 인스톨러 배포 자동화.
```text
Documents/Enterflow AI/
├── config/        (settings.json)
├── database/      (emotions.txt, camera.txt 등)
└── projects/      (프로젝트별 데이터)
```

---

## 15. Phase 3 & 4 (미래 확장)
- **Phase 3:** STEP 1~5 백그라운드 처리, 간소화 UI 완성본 제작.
- **Phase 4:** 영상 제작 기능 추가. 생성 이미지를 활용해 **클링(Kling)** 비디오 파이프라인 연동.