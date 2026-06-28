# Cyberpunk Globe // Three.js

> 🌃 네오 도쿄 사이버펑크 무드의 인터랙티브 와이어프레임 지구본 — 단일 HTML 파일, 의존성은 Three.js CDN 하나.

An interactive cyberpunk wireframe globe rendered with Three.js, with neon-blue **beacons** erupting from random surface points and a **bloom**-driven glow. Everything ships as a single `index.html` — no build step.

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

Deployed via Vercel — see the repo's *About* panel for the latest production URL.
