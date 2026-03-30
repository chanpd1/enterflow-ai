---
[Angle Mapping Data]
- UI Previews: ./camera_rigs/previews/
- Rigging Data (Basic): ./camera_rigs/rigs_basic/
- Rigging Data (Custom): ./camera_rigs/rigs_custom/
- Rule: 3D_Preview_ID == Rigging_Image_ID (Extension Matching: .jpg -> .png)
---

## [Section 1: Hybrid Path Settings]
- **Core Database:** [GitHub Repo Link]
- **User Local Assets:** `/User_Assets/characters/`, `/User_Assets/backgrounds/`
- **Rule:** 캐릭터 및 배경 추가 시 로컬 하드 내 정해진 ID 폴더 구조를 준수할 것.

---

## [Section 2: Camera Angles & Lens Mapping]

## 1. Camera Angles (구도 및 앵글)

### ### Eye Level (아이 레벨)
- **Aliases**: eye level shot, eye-level, 아이레벨
- **Description**: 일반적이고 자연스러운 시선. 피사체와 동등한 높이에서 촬영하여 안정감 부여.
- **Prompt**: `(eye level shot:1.6), (natural perspective:1.5), (neutral camera angle:1.4), (50mm standard lens:1.2)`

### ### High Angle (하이 앵글 / MZ 앵글)
- **Aliases**: high angle shot, 하이앵글, looking down, MZ angle
- **Description**: 위에서 아래로 내려다보는 구도. 피사체를 작게 보이게 하거나 트렌디한 감각(MZ샷) 연출.
- **Prompt**: `(high angle shot:1.8), (camera looking down:1.7), (24mm wide angle lens:1.4), (youthful aesthetic:1.3)`

### ### Low Angle (로우 앵글)
- **Aliases**: low angle shot, 로우앵글, looking up, 올려다보기
- **Description**: 아래에서 위로 올려다보며 인물의 권위, 웅장함 또는 극적인 긴장감을 표현.
- **Prompt**: `(low angle shot:1.8), (camera looking up:1.7), (powerful perspective:1.6), (35mm wide lens:1.3), (heroic framing:1.5)`

### ### front left quarter view Low Angle shot medium shot (왼쪽 45도 로우 미디엄 샷)
- **Aliases**: front left 3/4 view, left low medium, 왼쪽 로우 미디엄
- **Description**: 왼쪽 45도 방향에서 위로 올려다보는 구도. 인물의 입체감과 위엄을 강조.
- **Prompt**: `(front left quarter view:1.5), (low angle shot:1.4), (medium shot:1.3), (35mm standard lens:1.3), (sharp focus on face:1.4)`

### ### front right quarter view Low Angle shot medium shot (오른쪽 45도 로우 미디엄 샷)
- **Aliases**: front right 3/4 view, right low medium, 오른쪽 로우 미디엄
- **Description**: 오른쪽 45도 방향에서 위로 올려다보는 구도. 역동적인 원근감 확보.
- **Prompt**: `(front right quarter view:1.5), (low angle shot:1.4), (medium shot:1.3), (35mm standard lens:1.3), (dynamic perspective:1.3)`

### ### Over The Shoulder shot_view of screen left to right (왼쪽 오버 더 숄더 샷)
- **Aliases**: left OTS, over the shoulder left, 왼쪽 어깨 너머
- **Description**: 왼쪽 인물의 어깨를 걸치고 오른쪽 인물에 초점을 맞춤. 대화나 대치 상황 연출.
- **Prompt**: `(over the shoulder shot:1.6), (view from left to right:1.5), (focus on person on the right:1.4), (85mm telephoto lens:1.5), (blurred foreground shoulder:1.5), (deep bokeh:1.4)`

### ### Over The Shoulder shot_view of screen right to left (오른쪽 오버 더 숄더 샷)
- **Aliases**: right OTS, over the shoulder right, 오른쪽 어깨 너머
- **Description**: 오른쪽 인물의 어깨를 걸치고 왼쪽 인물에 초점을 맞춤. 심도 깊은 시네마틱 룩.
- **Prompt**: `(over the shoulder shot:1.6), (view from right to left:1.5), (focus on person on the left:1.4), (85mm telephoto lens:1.5), (blurred foreground shoulder:1.5), (deep bokeh:1.4)`

### ### Bird's Eye View (버드 아이 뷰)
- **Aliases**: bird eye view, overhead shot, top down, 탑뷰, 버드아이뷰
- **Description**: 아주 높은 곳에서 수직으로 내려다보는 장면. 전체 상황 조감.
- **Prompt**: `(bird's eye view:1.9), (overhead shot:1.8), (16mm ultra wide lens:1.4), (top-down perspective:1.7)`

### ### Dutch Angle (더치 앵글)
- **Aliases**: dutch angle, canted angle, 기울어진 앵글
- **Description**: 카메라를 비스듬히 기울여 불안함, 긴장감, 심리적 혼란을 표현.
- **Prompt**: `(dutch angle:1.8), (tilted camera:1.7), (canted perspective:1.6), (unbalanced composition:1.5)`

### ### Cowboy Shot (카우보이 샷)
- **Aliases**: cowboy shot, american shot, mid-thigh shot, 카우보이샷
- **Description**: 허벅지 중간부터 머리까지 촬영. 인물의 위엄과 액션감을 동시에 보여줌.
- **Prompt**: `(cowboy shot:1.6), (mid-thigh shot:1.5), (35mm standard lens:1.3), (cinematic action pose:1.4)`

### ### Close up shot_portrait (정면 초상화 클로즈업)
- **Aliases**: close up, portrait shot, 어깨 샷, 프로필 사진
- **Description**: 어깨부터 얼굴까지 집중 촬영. 표정 묘사에 최적화.
- **Prompt**: `(close up shot:1.5), (portrait:1.4), (85mm portrait lens:1.3), (sharp focus on eyes:1.5), (creamy bokeh:1.4)`

### ### Extream close up shot_face portrait (익스트림 얼굴 클로즈업)
- **Aliases**: extreme close up, ECU, 얼굴 만 촬영
- **Description**: 얼굴 전체를 프레임에 가득 채움. 디테일한 피부 질감과 눈동자 강조.
- **Prompt**: `(extreme close up:1.7), (100mm macro lens:1.5), (70-200mm telephoto compression:1.4), (hyper-detailed face:1.5)`

### ### Full body shot (정면 전신 샷)
- **Aliases**: full body shot, 전신샷
- **Description**: 인물의 머리부터 발끝까지 전체를 담는 구도.
- **Prompt**: `(full body shot:1.6), (35mm wide lens:1.3), (standing pose:1.4), (environmental context:1.2)`

### ### Two shot / Group shot (투 샷 / 그룹 샷)
- **Aliases**: two shot, group shot, ensemble shot, 단체 샷
- **Description**: 두 명 이상의 인물을 한 화면에 담음. 상호작용과 전체적인 분위기 강조.
- **Prompt**: `(two shot:1.6), (group shot:1.7), (35mm cinematic lens:1.3), (all subjects in focus:1.4), (balanced group composition:1.4)`

### ### Dutch Angle (더치 앵글)
- **Aliases**: dutch angle, tilted frame, slanted shot, 사선 앵글, 기울어진 각도
- **Description**: 카메라를 수평에서 비스듬하게 기울여 촬영. 불안정함, 긴장감, 혹은 역동적이고 감각적인 분위기를 연출할 때 사용.
- **Prompt**: `(dutch angle:1.5), (tilted camera:1.4), (canted angle:1.3), (dynamic composition:1.4), (cinematic tension:1.2), (off-kilter frame:1.3)`

### ### Ultra Extreme Close-up Shot (울트라 익스트림 클로즈업)
- **Aliases**: ultra extreme close-up, ECU, macro shot, detail shot, 눈/피부 초밀착 샷
- **Description**: 특정 부위(눈동자, 입술, 피부 질감 등)를 화면 가득 채움. 미세한 표정 변화나 극도의 디테일한 텍스처를 강조.
- **Prompt**: `(ultra extreme close-up:1.8), (macro photography:1.5), (hyper-detailed skin texture:1.6), (detailed eyes and eyelashes:1.5), (sharp focus on iris:1.4), (8k resolution:1.2), (pore detail:1.3)`

### ### Bust shot (바스트 샷)
- **Aliases**: bust shot, chest-up shot, portrait shot, 가슴 상단 샷
- **Description**: 인물의 가슴 윗부분부터 머리 끝까지 화면에 담음. 표정과 의상, 메이크업의 디테일을 강조하며 일반적인 인터뷰나 프로필 사진에 가장 많이 사용됨.
- **Prompt**: `(bust shot:1.6), (portrait:1.5), (upper body:1.3), (eye level:1.2), (depth of field:1.3), (soft lighting:1.2), (professional photography:1.4)`

### ### === Tight shoulder-up shot (시네마틱 인물 포커스 구도) ===
* **Prompt:** "Tight shoulder-up portrait, camera positioned at eye level, focusing intensely on the facial features. The frame starts from the mid-chest to just above the head, creating a sense of intimacy and emotional depth. Extremely shallow depth of field with creamy background bokeh."

### ### === Top of the head slightly cropped (시네마틱 인물 포커스 구도) ===
* **Prompt:** "Tight cinematic close-up with the top of the head slightly cropped by the upper edge of the frame. Focus is pinned to the eyes. This framing maximizes the intensity of the character's expression, mimicking 70mm Hollywood cinematography. Minimal headspace to create a pressurized, immersive atmosphere."

---

## 2. Lens Reference (렌즈 레퍼런스)

- **200mm**: `(200mm extreme telephoto lens:1.4)`, `(heavy background compression:1.5)` | 극단적인 배경 압축과 인물 클로즈업.
- **85mm**: `(85mm prime portrait lens:1.3)`, `(natural background compression:1.4)`, `(creamy bokeh:1.4)` | 정석적인 시네마틱 인물 초상화.
- **50mm**: `(50mm standard prime lens:1.2)`, `(natural human eye perspective:1.3)` | 왜곡 없는 인간의 시야 재현.
- **35mm**: `(35mm wide-angle lens:1.3)`, `(environmental portrait:1.4)` | 배경과 인물이 조화로운 서사적 구도.
- **24mm**: `(24mm wide angle:1.3)`, `(dynamic perspective distortion:1.4)` | 광각의 시원한 공간감과 역동적 왜곡.
- **16mm**: `(16mm ultra wide-angle lens:1.4)`, `(top-down view:1.3)` | 극단적 광각, 버드 아이 뷰 및 웅장한 풍경.
- **Anamorphic**: `(anamorphic lens:1.5)`, `(horizontal blue lens flare:1.4)`, `(oval bokeh:1.3)` | 가로 플레어와 타원형 보케가 특징인 영화적 질감.