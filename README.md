# Cyberpunk Globe // Three.js

> 🌃 네오 도쿄 사이버펑크 무드의 인터랙티브 와이어프레임 지구본 — 단일 HTML 파일, 의존성은 Three.js CDN 하나.

**🌐 네오 도쿄 사이버펑크 무드의 인터랙티브 3D 와이어프레임 지구본** — Three.js + UnrealBloomPass 기반. 표면의 랜덤 좌표에서 네온 블루 빛 기둥(Beacon)이 솟아오르고 파동이 퍼져나가는 블룸 효과가 살아있는 단일 HTML 파일 데모.

An interactive cyberpunk wireframe globe rendered with Three.js, with neon-blue **beacons** erupting from random surface points and a **bloom**-driven glow. Everything ships as a single `index.html` — no build step.

🔗 **라이브 데모**: https://cyberpunk-globe-pink.vercel.app

---

## 📑 목차 / Contents

- [✨ Features / 기능](#-features--기능)
- [🚀 Quick Start / 빠른 시작](#-quick-start--빠른-시작)
- [🎨 기본 사용 — 5분 따라하기](#-기본-사용--5분-따라하기)
- [🎛️ 샘플 옵션 — 색상/밀도/속도 프리셋](#️-샘플-옵션--색상밀도속도-프리셋)
- [🧠 직접 작성 — 커스텀 효과 추가하기](#-직접-작성--커스텀-효과-추가하기)
- [🗺️ 로드맵](#-로드맵)
- [🎛️ Customization / 커스터마이즈](#️-customization--커스터마이즈)
- [🧠 How it works / 동작 원리](#-how-it-works--동작-원리)
- [🛠 Troubleshooting / 트러블슈팅](#-troubleshooting--트러블슈팅)
- [📂 Files / 파일 구조](#-files--파일-구조)
- [📜 License / 라이선스](#-license--라이선스)

---

## ✨ Features / 기능

- **Wireframe globe** — `SphereGeometry` → `WireframeGeometry`, vertex-colored cyan→pink latitude gradient.
- **Neon beacons** — `CylinderGeometry` light pillars that grow out of random surface points, with a pulsing halo and an expanding wave ring.
- **Star field** — 2,600 procedural stars on a surrounding sphere (mostly white/cyan, occasional pink sparks).
- **UnrealBloom** — `UnrealBloomPass` for the signature neon halo (strength 1.45, radius 0.42, threshold 0.08).
- **OrbitControls** — drag to rotate, scroll to zoom, shift+drag to pan. Auto-rotate on by default.
- **HUD** — `Share Tech Mono` overlay, scanline texture, and a `CYBERPUNK GLOBE` corner badge.
- **Responsive** — handles window resize, caps `devicePixelRatio` at 2.
- **No build step** — drop `index.html` on any static host.

---

## 🚀 Quick start / 빠른 시작

```bash
# 1. Clone
git clone https://github.com/sigco3111/cyberpunk-globe.git
cd cyberpunk-globe

# 2. Serve (any static server works)
python3 -m http.server 8765
#   → open http://localhost:8765
```

That's it. / 끝. No `npm install`, no bundler.

### Deploy to Vercel / Vercel 배포

The repo already includes a `vercel.json` for plain static hosting:

```json
{
  "buildCommand": null,
  "outputDirectory": ".",
  "installCommand": null,
  "framework": null,
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

Manual deploy:

```bash
npm i -g vercel
vercel --prod
```

---

## 🎨 기본 사용 — 5분 따라하기

브라우저에서 지구본이 자동으로 회전하는 게 보이면 성공!

1. **회전시켜보기** — 마우스 드래그로 지구본을 자유롭게 회전
2. **줌 인/아웃** — 마우스 휠로 가까이/멀리
3. **Beacon 관찰** — 지구 표면 어디서든 네온 빛 기둥이 솟아오르는 걸 기다리기 (2~4초마다 새로 등장)
4. **시점 이동** — `Shift + 드래그`로 카메라 위치 평행이동
5. **자동 회전 끄기** — 첫 드래그하면 자동 회전이 멈춤 (OrbitControls 기본 동작)

### 🖱️ 마우스 단축키 요약

| 입력 | 동작 |
|---|---|
| 좌클릭 드래그 | 회전 |
| 휠 | 줌 인/아웃 |
| Shift + 드래그 | 팬(이동) |
| 우클릭 드래그 | 동일 (폴백) |

---

## 🎛️ 샘플 옵션 — 색상/밀도/속도 프리셋

`index.html` 상단의 `CFG` 객체를 통째로 교체하면 분위기를 완전히 바꿀 수 있어. **클릭해서 복사 → 그대로 붙여넣기** 만 하면 됨.

<details>
<summary>🌃 <b>네오 도쿄 (기본값)</b> — 시안/마젠타 네온</summary>

```js
const CFG = {
  BG_COLOR:         0x03030a,
  NEON_BLUE:        0x00d4ff,
  NEON_PINK:        0xff2bd6,
  GLOBE_RADIUS:     1.0,
  GLOBE_SEGMENTS:   48,
  BEACON_COUNT:     6,
  BEACON_LIFETIME:  [2.4, 4],
  STAR_COUNT:       2600,
  BLOOM_STRENGTH:   1.45,
  BLOOM_RADIUS:     0.42,
  BLOOM_THRESHOLD:  0.08,
  AUTO_ROTATE:      true,
  AUTO_ROTATE_SPEED:0.45,
};
```
</details>

<details>
<summary>💚 <b>매트릭스 코드</b> — 네온 그린 + 화이트 스파크</summary>

```js
const CFG = {
  BG_COLOR:         0x000500,
  NEON_BLUE:        0x39ff14,   // 매트릭스 그린
  NEON_PINK:        0xffffff,   // 흰 스파크
  BEACON_COUNT:     10,
  BEACON_LIFETIME:  [3, 5],
  BLOOM_STRENGTH:   1.8,
  BLOOM_RADIUS:     0.5,
  STAR_COUNT:       3500,
};
```
</details>

<details>
<summary>🌅 <b>블레이드 러너 2049</b> — 오렌지 + 틸</summary>

```js
const CFG = {
  BG_COLOR:         0x0a0612,
  NEON_BLUE:        0x00ffd1,   // 틸
  NEON_PINK:        0xff7a1a,   // 선셋 오렌지
  BEACON_COUNT:     5,
  BLOOM_STRENGTH:   1.6,
  BLOOM_RADIUS:     0.5,
};
```
</details>

<details>
<summary>🛸 <b>HAL 9000</b> — 단일 빨강 액센트, 미니멀</summary>

```js
const CFG = {
  BG_COLOR:         0x050000,
  NEON_BLUE:        0xff1a1a,   // HAL 빨강
  NEON_PINK:        0x550000,   // 어두운 마룬
  BEACON_COUNT:     2,
  BEACON_LIFETIME:  [6, 10],
  BLOOM_STRENGTH:   2.2,
  BLOOM_RADIUS:     0.7,
  STAR_COUNT:       800,
};
```
</details>

<details>
<summary>🌌 <b>사이버펑크 고밀도</b> — 빔콘 20개, 빠른 갱신</summary>

```js
const CFG = {
  BG_COLOR:         0x02020a,
  NEON_BLUE:        0x00d4ff,
  NEON_PINK:        0xff2bd6,
  BEACON_COUNT:     20,
  BEACON_LIFETIME:  [1.2, 2.0],
  BLOOM_STRENGTH:   1.2,        // 빔 많으면 bloom 약하게
  BLOOM_RADIUS:     0.35,
  GLOBE_SEGMENTS:   64,
};
// ⚠️ 성능 주의: 통합 GPU에서는 프레임 드랍 가능. STAR_COUNT도 1200 이하 권장.
```
</details>

---

## 🧠 직접 작성 — 커스텀 효과 추가하기

### 패턴 1: Beacon 클릭하면 도시 이름 표시하기

`index.html`의 `spawnBeacon()` 함수 안에 hover 라벨 추가:

```js
function spawnBeacon() {
  const surface = randSurfacePoint();
  // ... 기존 코드 ...

  // 도시 데이터 (랜덤 선택)
  const cities = ['Seoul', 'Tokyo', 'NYC', 'London', 'Sydney', 'Mars'];
  const name = cities[Math.floor(Math.random() * cities.length)];

  // HTML 라벨 추가 (Three.js Sprite보다 가벼움)
  const label = document.createElement('div');
  label.textContent = name;
  label.style.cssText = `
    position: absolute;
    color: #00d4ff;
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    text-shadow: 0 0 8px #00d4ff;
    pointer-events: none;
  `;
  document.body.appendChild(label);

  // 매 프레임 위치 업데이트
  const update = () => {
    if (!beacon.mesh.parent) {
      label.remove();
      return;
    }
    const v = beacon.mesh.position.clone();
    v.project(camera);
    label.style.left = `${(v.x * 0.5 + 0.5) * window.innerWidth}px`;
    label.style.top  = `${(-v.y * 0.5 + 0.5) * window.innerHeight}px`;
    requestAnimationFrame(update);
  };
  update();

  // ... 기존 코드 ...
}
```

### 패턴 2: 마우스 hover 시 해당 좌표 강조

```js
window.addEventListener('mousemove', (e) => {
  const ndc = new THREE.Vector2(
    (e.clientX / window.innerWidth) * 2 - 1,
    -(e.clientY / window.innerHeight) * 2 + 1
  );
  raycaster.setFromCamera(ndc, camera);
  const hit = raycaster.intersectObject(globeMesh);
  if (hit.length > 0) {
    // hit[0].point: 표면 좌표
    // hit[0].uv: 위경도 (-1 ~ 1)
  }
});
```

### 패턴 3: JSON 데이터로 Beacon 좌표 고정

`data/cities.json` 추가:

```json
[
  { "name": "Seoul",  "lat": 37.5665, "lon": 126.9780 },
  { "name": "Tokyo",  "lat": 35.6762, "lon": 139.6503 },
  { "name": "Berlin", "lat": 52.5200, "lon": 13.4050 }
]
```

`spawnBeacon()` 수정:

```js
import cities from './data/cities.json';

function spawnBeacon() {
  const c = cities[Math.floor(Math.random() * cities.length)];
  const phi   = THREE.MathUtils.degToRad(90 - c.lat);
  const theta = THREE.MathUtils.degToRad(c.lon);
  const surface = new THREE.Vector3(
    CFG.GLOBE_RADIUS * Math.sin(phi) * Math.cos(theta),
    CFG.GLOBE_RADIUS * Math.cos(phi),
    CFG.GLOBE_RADIUS * Math.sin(phi) * Math.sin(theta)
  );
  // ... 나머지는 동일 ...
}
```

> ⚠️ JSON import는 `<script type="module">`에서 직접 가능 (Vercel 정적 호스팅 OK)

---

## 🗺️ 로드맵

- [x] v1.0 — 와이어프레임 + Beacon + Bloom (2026-06-29)
- [ ] v1.1 — 🎵 오디오 리액티브 (Web Audio API로 Beacon height가 BPM에 반응)
- [ ] v1.2 — 🛰️ ISS 실시간 위치 + 궤도 트레일 (무키 API: wheretheiss.at)
- [ ] v1.3 — 🖱️ hover/click 시 도시 정보 패널 (JSON 데이터 통합)
- [ ] v1.5 — 🎮 키보드 컨트롤 (WASD 카메라 + Space 줌 리셋)
- [ ] v2.0 — 🌐 WebXR (VR 헤드셋 지원)

기여 환영 — PR 보내주세요!

---

## 🎛 Customization / 커스터마이즈

All knobs live in a single `CFG` object at the top of the `<script type="module">` block in `index.html`:

| Constant           | Default     | Purpose                                                |
| ------------------ | ----------- | ------------------------------------------------------ |
| `BG_COLOR`         | `0x03030a`  | Deep cosmic background                                 |
| `NEON_BLUE`        | `0x00d4ff`  | Primary accent (beacons, equator)                      |
| `NEON_PINK`        | `0xff2bd6`  | Secondary accent (variation beacons, meridian)         |
| `GLOBE_RADIUS`     | `1.0`       | Globe radius (in scene units)                          |
| `GLOBE_SEGMENTS`   | `48`        | Sphere subdivision — higher = denser wireframe         |
| `BEACON_COUNT`     | `6`         | Active beacons at any moment                           |
| `BEACON_LIFETIME`  | `[2.4, 4]`  | Lifespan range in seconds                              |
| `STAR_COUNT`       | `2600`      | Number of background stars                             |
| `BLOOM_STRENGTH`   | `1.45`      | Bloom intensity                                        |
| `BLOOM_RADIUS`     | `0.42`      | Bloom blur radius                                      |
| `BLOOM_THRESHOLD`  | `0.08`      | Pixel brightness above which bloom kicks in            |
| `AUTO_ROTATE`      | `true`      | Auto-rotate the camera                                 |
| `AUTO_ROTATE_SPEED`| `0.45`      | Auto-rotation speed                                    |

### Change the color palette / 색상 팔레트 바꾸기

Edit the `NEON_BLUE` and `NEON_PINK` constants. For a *Matrix*-style look:

```js
NEON_BLUE: 0x39ff14,   // neon green
NEON_PINK: 0xffffff,   // white sparks
```

For a *Blade Runner 2049* orange/teal look:

```js
NEON_BLUE: 0x00ffd1,
NEON_PINK: 0xff7a1a,
```

### Add more beacons / 빔콘 더 띄우기

```js
BEACON_COUNT:    12,
BEACON_LIFETIME: [3, 5],
```

Bloom is a heavy post-process — if you raise `BEACON_COUNT` much past ~15, consider lowering `BLOOM_RADIUS` to keep frame-rate smooth on integrated GPUs.

### Replace the procedural beacons with real data / 실제 데이터로 교체

`spawnBeacon()` reads a random point from `randSurfacePoint()`. To use real coordinates, replace its body:

```js
// e.g. capitals
const points = [
  { lat:  37.5665, lon: 126.9780, name: 'Seoul' },
  { lat:  35.6762, lon: 139.6503, name: 'Tokyo' },
  { lat: -33.8688, lon: 151.2093, name: 'Sydney' },
];
function spawnBeacon() {
  const p = points[Math.floor(Math.random() * points.length)];
  const phi   = THREE.MathUtils.degToRad(90 - p.lat);
  const theta = THREE.MathUtils.degToRad(p.lon);
  const surface = new THREE.Vector3(
    CFG.GLOBE_RADIUS * Math.sin(phi) * Math.cos(theta),
    CFG.GLOBE_RADIUS * Math.cos(phi),
    CFG.GLOBE_RADIUS * Math.sin(phi) * Math.sin(theta)
  );
  // ... rest of the function unchanged ...
}
```

---

## 🧠 How it works / 동작 원리

1. **Globe**: `SphereGeometry(1, 48, 28)` is wrapped in `WireframeGeometry`. We attach a vertex color attribute that lerps cyan → pink by latitude, giving the wireframe depth.
2. **Beacons**: a shared thin `CylinderGeometry` is instanced as a `Mesh` per beacon. A `Quaternion.setFromUnitVectors(yAxis, surfaceNormal)` orients the cylinder so it pokes straight out of the globe. We fade the `MeshBasicMaterial.opacity` in over 0.35s and back out near end-of-life.
3. **Wave ring**: a `RingGeometry(0.01, 0.022)` with `AdditiveBlending` is scaled from ~0.4× → 1.3× over 1.6s while opacity fades — that's the ripple.
4. **Star field**: 2,600 random points on a sphere of radius 22, colored mostly white/blue with a sprinkle of pink for visual variety.
5. **Bloom**: `EffectComposer` chains `RenderPass → UnrealBloomPass → OutputPass`. ACES tonemapping + sRGB output color space keeps the highlights from clipping to pure white.
6. **Animation loop**: clock-based delta time, `controls.update()` every frame (required when `enableDamping` is on), `composer.render()` instead of `renderer.render()`.

---

## 🛠 Troubleshooting / 트러블슈팅

### ❗ "Failed to resolve module specifier 'three'"

You're opening `index.html` with `file://` instead of through a server. **Use a static server**:

```bash
python3 -m http.server 8765
# or
npx serve .
```

`file://` blocks the ESM `import` from the unpkg CDN.

### ❗ Bloom isn't visible / 블룸이 안 보임

UnrealBloomPass only blooms pixels **brighter than the threshold**. If your scene looks dim:

- Lower `BLOOM_THRESHOLD` to `0.05`.
- Raise `BLOOM_STRENGTH` to `1.8`–`2.2`.
- Make sure `MeshBasicMaterial` colors are using `THREE.Color` instances of full intensity (0xffffff base) — dim colors won't pass the threshold.

### ❗ "THREE.WebGLProgram: shader error" / 셰이더 에러

You're probably mixing `three.min.js` (UMD globals) and ESM imports. **Pick one pattern.** This project uses ESM + importmap, so *all* scripts that touch `THREE` must be `<script type="module">` blocks.

### ❗ Lines look jagged / 라인이 계단처럼 보임

The renderer already has `antialias: true`. If lines are still ugly:

- Raise `GLOBE_SEGMENTS` to `64` or `80`.
- Cap `setPixelRatio` to at least `2` on high-DPI displays (already done).
- Avoid using `LineDashedMaterial` — it doesn't antialias.

### ❗ Frame-rate drops on mobile / 모바일 프레임 드랍

Bloom is expensive at 1080p. Drop resolution:

```js
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 1.25));
composer.setPixelRatio(Math.min(window.devicePixelRatio, 1.25));
```

…and lower `STAR_COUNT` to `1200`.

### ❗ Want to embed inside a page, not a fullscreen globe

Add `controls.enablePan = false` and `controls.minDistance = controls.maxDistance = 2.4` to lock the camera. Wrap `#app` in a `position: relative; max-width: 600px; aspect-ratio: 1;` container and the canvas will adapt automatically because of the `resize` handler.

---

## 📂 Files / 파일 구조

```
cyberpunk-globe/
├── index.html      # The entire app (CSS + ES module inlined)
├── vercel.json     # Static-hosting manifest for Vercel
├── LICENSE         # MIT
└── README.md       # You are here
```

---

## 📜 License / 라이선스

MIT — do whatever, no warranty. / 마음대로 쓰세요, 보증 없음.

---

## 🌐 Live demo / 라이브 데모

- **Vercel (Production)**: https://cyberpunk-globe-pink.vercel.app
- **GitHub Pages**: 미설정 (필요시 Settings → Pages 활성화)

배포 자동화 원하면 `vercel.json` + GitHub Actions 워크플로 추가 가능 — [issue로 요청](https://github.com/sigco3111/cyberpunk-globe/issues/new).

---

## 🙏 Credits / 크레딧

- **Three.js** — [mrdoob/three.js](https://github.com/mrdoob/three.js) (MIT)
- **UnrealBloomPass** — Three.js examples (MIT)
- **Share Tech Mono** — Google Fonts (OFL)
- **AI 생성**: [MiniMax-M3](https://www.minimax.io/) (via [oh-my-openagent](https://github.com/code-yeongyu/oh-my-openagent) on [OpenCode](https://opencode.ai/))
- **아이디어**: 사용자의 "Three.js 사이버펑크 지구본" 프롬프트 (2026-06-29)

---

## 🤖 생성 방법 / How it was generated

이 프로젝트는 AI 에이전트 워크플로로 처음부터 만들어졌습니다.

### 🛠️ 도구 스택

| 단계 | 도구 |
|---|---|
| 사용자 아이디어 프롬프트 | Telegram (chat with [Hermes Agent](https://github.com/sigco3111/cyberpunk-globe)) |
| 코드 생성 | **OpenCode** CLI + **oh-my-openagent** 플러그인 |
| LLM | **[MiniMax-M3](https://www.minimax.io/)** (소속사: minimax) |
| GitHub 푸시 + Vercel 배포 | 자동 (GitHub webhook + Vercel 자동 빌드) |

### 📝 사용자 프롬프트 (원문)

> Three.js 라이브러리를 CDN으로 불러와서 우주를 배경으로 스스로 회전하는 사이버펑크 스타일의 3D 와이어프레임 지구본을 만들어주는데, 마우스로 드래그하여 자유롭게 회전과 줌인/아웃이 가능해야 하고, 지구 표면의 랜덤한 좌표에서 네온 블루 색상의 빛 기둥(Beacon)이 솟아오르며 주변으로 파동이 퍼져나가는 블룸(Bloom) 효과가 적용된 고퀄리티 인터랙티브 웹페이지를 단일 HTML 파일로 작성해줘.
>
> **Implementation Advice**: Use Three.js for 3D rendering. Use EffectComposer and UnrealBloomPass for the neon bloom effects. The globe can be created using WireframeGeometry. Beacons can be implemented as simple CylinderGeometry with shader materials for the pulse effect. 모든 의존관계의 코드를 하나의 HTML에 담는 형태로 코드 작성.

### 🔄 생성 워크플로

1. **아이디어 입력** — Telegram에서 "Three.js 사이버펑크 지구본 만들어줘" 한 줄 프롬프트
2. **브레인스토밍** — 디자인 결정 4가지 (컨셉/데이터/인터랙션/배포) A/B/C 옵션 제시
3. **OpenCode 위임** — `delegate_task`로 풀 자동 워크플로 (구현 → push → Vercel 배포)
4. **Live URL 검증** — `curl -sI`로 HTTP 200 확인 후 사용자에게 보고
5. **README 보강** — 한/영 병기 다층 옵션 + 생성 이력 명시

### 📝 AI 도구 워크플로 노트 (다른 프로젝트에 재현 시)

- **OpenCode 풀 자동 위임** 시 결정사항 4개를 A/B/C 옵션 + 추천으로 미리 정리해서 넘기는 게 핵심
- **importmap + ESM 패턴**이 2026년 Three.js 단일 HTML에서 가장 안정적 (Chrome 89+, Safari 16.4+, Firefox 108+)
- **UnrealBloomPass는 색이 진해야 bloom이 보임** — threshold 낮게(0.1), strength 1.5 정도
- **Vercel 단일 HTML 배포** 시 `vercel.json`의 `outputDirectory: "."` 명시 필수 (Vite 같은 빌드 도구가 없으면 Vercel이 빈 dist를 찾으려 함)

---

<p align="center">
  <sub>🌃 Built with neon glow + late-night vibes · sigco3111 · MIT · AI-generated by MiniMax-M3</sub>
</p>
