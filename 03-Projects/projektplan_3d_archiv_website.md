# Technischer Projektplan: 3D-Raum-Archiv-Website

**Projekttitel:** Digitales Bildarchiv mit 3D-Raum-Interface
**Stack:** Polycam / Blender · Next.js 14 · Three.js · Vercel
**Stand:** Mai 2026

---

## Inhaltsverzeichnis

1. Gesamtprojektplan und Phasenübersicht
2. Tech-Stack-Architektur
3. Pipeline: Polycam → Nachbearbeitung → Webexport
4. Projektstruktur (Next.js / Three.js)
5. Entwicklungsschritte chronologisch
6. Performance-Optimierung
7. Archivstruktur und Content-Management
8. Verbindung 3D-Raum und Archivinhalte
9. Aufwandsschätzung
10. Risiken und Lösungen
11. Deployment-Strategie Vercel

---

## 1. Gesamtprojektplan und Phasenübersicht

Das Projekt teilt sich in fünf aufeinander aufbauende Phasen. Jede Phase hat einen klar definierten Output, der Voraussetzung für die nächste ist.

```
Phase 0: Scan & Asset-Produktion     [Woche 1]
Phase 1: Setup & Infrastruktur       [Woche 2]
Phase 2: 3D-Viewer-Core              [Woche 3–4]
Phase 3: Archiv-System               [Woche 4–5]
Phase 4: Verknüpfung & Hotspots      [Woche 5–6]
Phase 5: Finalisierung & Deployment  [Woche 6–7]
```

### Phase 0: Scan und Asset-Produktion

Ziel ist ein qualitativ hochwertiger, webbereiter GLB-Export des Raums.

**Inputs:**
- iPhone mit Polycam-App (LiDAR-Modus bevorzugt)
- Blender 4.x für Nachbearbeitung

**Outputs:**
- `room.glb` (optimiert, max. 15 MB)
- `room_preview.jpg` (Thumbnail für Loading-Screen)
- Hotspot-Koordinatenliste (JSON)

---

## 2. Tech-Stack-Architektur

### Kerntechnologien

| Schicht | Technologie | Begründung |
|---|---|---|
| Framework | Next.js 14 (App Router) | SSG für Archivseiten, API Routes für CMS |
| 3D-Rendering | Three.js (vanilla, via npm) | Volle Kontrolle, kein Overhead |
| 3D-Loader | GLTFLoader + DRACOLoader | GLB-Import mit Draco-Kompression |
| Styling | CSS Modules + CSS Variables | Kein Framework-Overhead |
| Archiv-Daten | JSON-Dateien (lokal) oder Notion API | Flat-File bevorzugt für Simplizität |
| Bilder | Next.js Image + Vercel Blob | Automatische Optimierung |
| Deployment | Vercel | Zero-Config, CDN, Preview-URLs |
| Asset-Hosting | Vercel Blob / direkt im `/public` | GLB unter 20 MB direkt in `/public` |

### Architekturentscheidung: Vanilla Three.js vs. React Three Fiber

**Empfehlung: Vanilla Three.js** in einer isolierten React-Komponente.

Begründung:
- Der bestehende Panorama-Code ist Vanilla Three.js, die Portierung ist direkt
- Mehr Kontrolle über den Render-Loop beim Raycasting für Hotspots
- React Three Fiber (R3F) wäre eleganter, aber fügt Komplexität hinzu, die bei diesem Scope nicht gerechtfertigt ist
- Die Three.js-Instanz lebt in einem `useEffect`, der Canvas wird direkt in den DOM gemountet

### Datenfluss-Architektur

```
[Polycam GLB] --> /public/models/room.glb
[Bild-Assets] --> /public/archive/images/
[Archiv-Daten] --> /content/archive/*.json
                        |
                        v
[Next.js Static Generation]
  - Archiv-Indexseite  /archive
  - Einzel-Entries     /archive/[slug]
  - API Route (optional) /api/archive
                        |
                        v
[Three.js Canvas Component]
  - Lädt room.glb via GLTFLoader
  - OrbitControls (invertiert für Innenraum)
  - Raycaster für Hotspot-Klicks
  - Hotspot-Overlays als HTML-Elemente über Canvas
```

---

## 3. Pipeline: Polycam → Nachbearbeitung → Webexport

### Schritt 1: Polycam-Scan (iPhone)

**Einstellungen für besten Scan:**
- Modus: LiDAR-Room-Scan (nicht Photo-Mode, falls LiDAR-iPhone vorhanden)
- Beleuchtung: Gleichmäßig, kein direktes Gegenlicht
- Bewegung: Langsam, überlappend, alle Winkel abdecken
- Ziel: 200-500 Frames für einen Raum (~20-40 m²)

**Export aus Polycam:**
- Format: `OBJ + MTL + Texturen` oder direkt `GLTF/GLB`
- Texturauflösung: 4096x4096 px (höchste verfügbare Qualität)
- Polycam Pro erforderlich für GLB-Export

**Exportvarianten und Empfehlung:**

| Format | Größe (typisch) | Web-tauglich | Empfehlung |
|---|---|---|---|
| OBJ | 50-200 MB | Nein (kein Bundle) | Nur als Zwischenschritt |
| GLB (unkomprimiert) | 20-80 MB | Schlecht | Nur für Blender-Import |
| GLB + Draco | 5-20 MB | Ja | Ziel-Format |
| USDZ | variabel | Nein | Nur für AR |

**Empfehlung:** Export als `GLB` aus Polycam, dann Optimierung in Blender.

---

### Schritt 2: Nachbearbeitung in Blender 4.x

```
Polycam GLB (roh, ~40MB)
        |
        v
[Import in Blender: File > Import > glTF 2.0]
        |
        v
[Mesh-Bereinigung]
  - Edit Mode: Merge by Distance (0.001m)
  - Normals: Recalculate Outside
  - Remove doubles, clean topology
        |
        v
[Mesh-Decimation]
  - Modifier: Decimate (Ratio 0.3-0.5)
  - Ziel: unter 200.000 Faces
  - Visuell prüfen: keine sichtbaren Artefakte
        |
        v
[Textur-Baking optional]
  - Nur wenn Polycam-Texturen zu fragmentiert
  - Bake Diffuse auf neue UV-Map (2048x2048)
        |
        v
[Export: File > Export > glTF 2.0]
  - Format: GLB (Binary)
  - Include: Mesh, Materials, Textures
  - Compression: Draco aktivieren
  - Draco-Level: 6 (Balance aus Qualität/Größe)
        |
        v
room.glb (~5-15 MB, Draco-komprimiert)
```

**Konkrete Blender-Export-Einstellungen:**
```
Format:              Binary (.glb)
Include:
  - Selected Objects: ja (nur den Raum-Mesh)
  - Custom Properties: nein
  - Cameras: nein
  - Punctual Lights: optional
Transform:
  - Y Forward, Z Up (Three.js-kompatibel)
Geometry:
  - Apply Modifiers: ja
  - UVs: ja
  - Normals: ja
  - Vertex Colors: nein (spart Speicher)
  - Materials: ja
  - Images: Automatic (integriert in GLB)
  - Compression: Draco
    - Mesh Compression Level: 6
    - Position Quantization: 14
    - Normal Quantization: 10
    - Texture Coordinate Quantization: 12
```

### Schritt 3: Post-Export-Optimierung mit gltf-transform

`gltf-transform` ist ein CLI-Tool für weitere Optimierungen nach Blender:

```bash
npm install -g @gltf-transform/cli

# Vollständige Optimierungspipeline
gltf-transform optimize room_blender.glb room.glb \
  --texture-compress webp \
  --texture-size 2048

# Alternativ Schritt für Schritt:
gltf-transform dedup room.glb room_dedup.glb
gltf-transform prune room_dedup.glb room_pruned.glb
gltf-transform resize room_pruned.glb room_resized.glb --width 2048 --height 2048
gltf-transform webp room_resized.glb room.glb
```

**Ziel-Metriken nach Optimierung:**
- Dateigröße: unter 15 MB
- Draw Calls: unter 50
- Textur-Gesamtgröße: unter 50 MB GPU-RAM
- Faces: unter 200.000

---

## 4. Projektstruktur (Next.js / Three.js)

```
project-root/
├── public/
│   ├── models/
│   │   └── room.glb                 # Optimiertes 3D-Modell
│   ├── archive/
│   │   └── images/                  # Archivbilder (original)
│   │       ├── work-001/
│   │       │   ├── main.jpg
│   │       │   └── detail-01.jpg
│   │       └── work-002/
│   └── draco/                       # Draco-Decoder (lokal hosten!)
│       ├── draco_decoder.js
│       ├── draco_decoder.wasm
│       └── draco_wasm_wrapper.js
│
├── content/
│   └── archive/                     # Archiv-Daten als JSON
│       ├── index.json               # Alle Einträge als Array
│       ├── work-001.json
│       └── work-002.json
│
├── src/
│   ├── app/                         # Next.js App Router
│   │   ├── layout.tsx
│   │   ├── page.tsx                 # Landingpage = 3D-Viewer
│   │   ├── archive/
│   │   │   ├── page.tsx             # Archiv-Übersicht
│   │   │   └── [slug]/
│   │   │       └── page.tsx         # Einzelner Archiveintrag
│   │   └── api/
│   │       └── archive/
│   │           └── route.ts         # Optional: API Route
│   │
│   ├── components/
│   │   ├── ThreeScene/
│   │   │   ├── ThreeScene.tsx       # Haupt-Canvas-Komponente
│   │   │   ├── ThreeScene.module.css
│   │   │   ├── useThreeSetup.ts     # Scene/Camera/Renderer Hook
│   │   │   ├── useModelLoader.ts    # GLTFLoader Hook
│   │   │   ├── useHotspots.ts       # Raycaster + Hotspot-Logik
│   │   │   └── useOrbitControls.ts  # Controls-Konfiguration
│   │   ├── Hotspot/
│   │   │   ├── Hotspot.tsx          # HTML-Overlay für Hotspots
│   │   │   └── Hotspot.module.css
│   │   ├── LoadingScreen/
│   │   │   ├── LoadingScreen.tsx
│   │   │   └── LoadingScreen.module.css
│   │   ├── ArchiveGrid/
│   │   │   └── ArchiveGrid.tsx      # Archiv-Übersicht
│   │   └── ArchiveEntry/
│   │       └── ArchiveEntry.tsx     # Einzel-Ansicht
│   │
│   ├── lib/
│   │   ├── three/
│   │   │   ├── sceneManager.ts      # Three.js Scene-Verwaltung
│   │   │   ├── hotspotManager.ts    # Hotspot-Koordinaten
│   │   │   └── animationLoop.ts     # RAF-Verwaltung
│   │   └── archive/
│   │       ├── getEntries.ts        # Archiv-Daten laden
│   │       └── types.ts             # TypeScript-Interfaces
│   │
│   └── styles/
│       ├── globals.css
│       └── variables.css
│
├── hotspots.json                    # Hotspot-Definitionen (Koordinaten)
├── next.config.js
├── tsconfig.json
└── package.json
```

### Kritische Dateien im Detail

**`hotspots.json` - Struktur:**
```json
[
  {
    "id": "hotspot-001",
    "label": "Werk 1889",
    "position": { "x": 1.2, "y": 1.5, "z": -2.3 },
    "archiveSlug": "work-001",
    "normal": { "x": 0, "y": 0, "z": 1 }
  }
]
```

**`content/archive/work-001.json`:**
```json
{
  "slug": "work-001",
  "title": "Titel des Werks",
  "year": 2024,
  "medium": "Öl auf Leinwand",
  "dimensions": "80 × 60 cm",
  "description": "Beschreibungstext...",
  "images": [
    { "src": "/archive/images/work-001/main.jpg", "alt": "Hauptansicht" },
    { "src": "/archive/images/work-001/detail-01.jpg", "alt": "Detail" }
  ],
  "hotspotId": "hotspot-001"
}
```

---

## 5. Entwicklungsschritte chronologisch

### Phase 0: Asset-Produktion (Woche 1)

**Tag 1-2: Scan**
- Polycam-App auf iPhone installieren/aktualisieren
- Raum vorbereiten: Beleuchtung prüfen, reflektierende Flächen ggf. abdecken
- Mehrere Scan-Durchläufe, besten auswählen
- Export als GLB aus Polycam (Pro-Account erforderlich)

**Tag 3-4: Blender-Nachbearbeitung**
- Import in Blender 4.x
- Mesh-Bereinigung und Decimation
- Textur-Check: alle Materialien korrekt?
- GLB-Export mit Draco-Kompression
- `gltf-transform` Optimierungspipeline durchlaufen

**Tag 5: Asset-Validierung**
- Dateigrößen-Check (Ziel: unter 15 MB)
- Browser-Test mit lokalem Three.js-Snippet
- Hotspot-Koordinaten im 3D-Raum identifizieren und notieren
- `hotspots.json` initial befüllen

---

### Phase 1: Setup und Infrastruktur (Woche 2)

**Next.js-Projekt initialisieren:**
```bash
npx create-next-app@latest archive-3d \
  --typescript \
  --tailwind=false \
  --app \
  --src-dir \
  --import-alias "@/*"

cd archive-3d

# Three.js und Loader
npm install three @types/three

# gltf-transform (Optimierung, lokal)
npm install -D @gltf-transform/cli

# Linting
npm install -D eslint-config-next
```

**Draco-Decoder setup:**

Der Draco-Decoder muss lokal verfügbar sein (nicht von CDN laden, Latenz-Problem):
```bash
# Draco-Dateien kopieren
cp node_modules/three/examples/jsm/libs/draco/ public/draco/ -r
```

**`next.config.js` konfigurieren:**
```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  // GLB-Dateien als statische Assets behandeln
  webpack: (config) => {
    config.module.rules.push({
      test: /\.(glb|gltf)$/,
      type: 'asset/resource',
    });
    return config;
  },
  // Große Bilder aus /public erlauben
  images: {
    domains: [],
    deviceSizes: [640, 1080, 1920],
  },
};

module.exports = nextConfig;
```

**Vercel CLI einrichten:**
```bash
npm install -g vercel
vercel login
vercel link
```

---

### Phase 2: 3D-Viewer-Core (Woche 3-4)

Dies ist die technisch kritischste Phase. Der bestehende Panorama-Code wird zu einem vollständigen GLB-Viewer erweitert.

**`src/components/ThreeScene/useThreeSetup.ts`:**
```typescript
import { useEffect, useRef } from 'react';
import * as THREE from 'three';

export function useThreeSetup(containerRef: React.RefObject<HTMLDivElement>) {
  const sceneRef = useRef<THREE.Scene | null>(null);
  const cameraRef = useRef<THREE.PerspectiveCamera | null>(null);
  const rendererRef = useRef<THREE.WebGLRenderer | null>(null);

  useEffect(() => {
    if (!containerRef.current) return;

    // Scene
    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0x111111);
    sceneRef.current = scene;

    // Camera - Innenraum-Perspektive
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.01,
      100
    );
    camera.position.set(0, 1.6, 0); // Augenhöhe ~1.6m
    cameraRef.current = camera;

    // Renderer
    const renderer = new THREE.WebGLRenderer({
      antialias: true,
      alpha: false,
    });
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.toneMapping = THREE.ACESFilmicToneMapping;
    renderer.toneMappingExposure = 1.0;
    renderer.outputColorSpace = THREE.SRGBColorSpace;
    rendererRef.current = renderer;

    containerRef.current.appendChild(renderer.domElement);

    // Basis-Beleuchtung
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(ambientLight);

    const dirLight = new THREE.DirectionalLight(0xffffff, 1.0);
    dirLight.position.set(5, 10, 5);
    scene.add(dirLight);

    const handleResize = () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    };
    window.addEventListener('resize', handleResize);

    return () => {
      window.removeEventListener('resize', handleResize);
      renderer.dispose();
      if (containerRef.current?.contains(renderer.domElement)) {
        containerRef.current.removeChild(renderer.domElement);
      }
    };
  }, []);

  return { sceneRef, cameraRef, rendererRef };
}
```

**`src/components/ThreeScene/useModelLoader.ts`:**
```typescript
import { useEffect, useState } from 'react';
import * as THREE from 'three';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
import { DRACOLoader } from 'three/addons/loaders/DRACOLoader.js';

export function useModelLoader(
  scene: THREE.Scene | null,
  modelPath: string
) {
  const [loadProgress, setLoadProgress] = useState(0);
  const [isLoaded, setIsLoaded] = useState(false);
  const [modelRef, setModelRef] = useState<THREE.Group | null>(null);

  useEffect(() => {
    if (!scene) return;

    const dracoLoader = new DRACOLoader();
    dracoLoader.setDecoderPath('/draco/');

    const loader = new GLTFLoader();
    loader.setDRACOLoader(dracoLoader);

    loader.load(
      modelPath,
      (gltf) => {
        const model = gltf.scene;

        // Kamera-Startposition: Mittelpunkt des Bounding Box
        const box = new THREE.Box3().setFromObject(model);
        const center = box.getCenter(new THREE.Vector3());

        model.position.sub(center);
        scene.add(model);
        setModelRef(model);
        setIsLoaded(true);
      },
      (progress) => {
        const percent = (progress.loaded / progress.total) * 100;
        setLoadProgress(Math.round(percent));
      },
      (error) => {
        console.error('Fehler beim Laden des 3D-Modells:', error);
      }
    );

    return () => {
      dracoLoader.dispose();
    };
  }, [scene, modelPath]);

  return { loadProgress, isLoaded, modelRef };
}
```

**`src/components/ThreeScene/useOrbitControls.ts`:**

Der entscheidende Unterschied zum Panorama-Code: OrbitControls werden für Innenraum-Navigation konfiguriert.
```typescript
import { useEffect } from 'react';
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

export function useOrbitControls(
  camera: THREE.PerspectiveCamera | null,
  renderer: THREE.WebGLRenderer | null,
  autoRotate: boolean = true
) {
  useEffect(() => {
    if (!camera || !renderer) return;

    const controls = new OrbitControls(camera, renderer.domElement);

    // Innenraum-Konfiguration (wie im Panorama-Beispiel, aber für GLB)
    controls.enableZoom = true;
    controls.enablePan = false;
    controls.enableDamping = true;
    controls.dampingFactor = 0.05;
    controls.rotateSpeed = -0.3; // Negativ = intuitiv für Innenraum
    controls.zoomSpeed = 0.5;

    // Vertikale Rotation begrenzen (Boden/Decke nicht überschlagen)
    controls.minPolarAngle = Math.PI * 0.1;
    controls.maxPolarAngle = Math.PI * 0.85;

    // Zoom begrenzen (nicht durch Wände)
    controls.minDistance = 0.1;
    controls.maxDistance = 5;

    // Auto-Rotation für Landingpage
    controls.autoRotate = autoRotate;
    controls.autoRotateSpeed = 0.3;

    // Animation Loop in Controls
    const animate = () => {
      controls.update();
    };

    return () => {
      controls.dispose();
    };
  }, [camera, renderer, autoRotate]);
}
```

**`src/app/page.tsx` - Die Landingpage:**
```typescript
'use client';

import dynamic from 'next/dynamic';

// ThreeScene darf nicht SSR sein (kein DOM im Node-Kontext)
const ThreeScene = dynamic(
  () => import('@/components/ThreeScene/ThreeScene'),
  {
    ssr: false,
    loading: () => <div className="loading-fallback">Lade Raum...</div>
  }
);

export default function HomePage() {
  return (
    <main style={{ width: '100vw', height: '100vh', overflow: 'hidden' }}>
      <ThreeScene modelPath="/models/room.glb" />
    </main>
  );
}
```

---

### Phase 3: Archiv-System (Woche 4-5)

**Archiv-Daten-Loader (`src/lib/archive/getEntries.ts`):**
```typescript
import { ArchiveEntry } from './types';
import entries from '../../../content/archive/index.json';

export async function getAllEntries(): Promise<ArchiveEntry[]> {
  return entries as ArchiveEntry[];
}

export async function getEntryBySlug(slug: string): Promise<ArchiveEntry | null> {
  const all = await getAllEntries();
  return all.find(e => e.slug === slug) ?? null;
}
```

**Archiv-Übersicht (`src/app/archive/page.tsx`):**
```typescript
import { getAllEntries } from '@/lib/archive/getEntries';
import ArchiveGrid from '@/components/ArchiveGrid/ArchiveGrid';

export default async function ArchivePage() {
  const entries = await getAllEntries();
  return <ArchiveGrid entries={entries} />;
}

// Statisch generieren
export const revalidate = false;
```

**Einzel-Eintrag (`src/app/archive/[slug]/page.tsx`):**
```typescript
import { getAllEntries, getEntryBySlug } from '@/lib/archive/getEntries';
import { notFound } from 'next/navigation';
import Image from 'next/image';

export async function generateStaticParams() {
  const entries = await getAllEntries();
  return entries.map(e => ({ slug: e.slug }));
}

export default async function EntryPage({
  params
}: {
  params: { slug: string }
}) {
  const entry = await getEntryBySlug(params.slug);
  if (!entry) notFound();

  return (
    <article>
      <h1>{entry.title}</h1>
      <p>{entry.year} · {entry.medium}</p>
      {entry.images.map((img, i) => (
        <Image
          key={i}
          src={img.src}
          alt={img.alt}
          width={1200}
          height={800}
          quality={85}
        />
      ))}
      <p>{entry.description}</p>
    </article>
  );
}
```

---

### Phase 4: Hotspot-System und Verknüpfung (Woche 5-6)

Das Hotspot-System ist die technisch anspruchsvollste Komponente. Es kombiniert 3D-Raycasting mit HTML-Overlays.

**`src/components/ThreeScene/useHotspots.ts`:**
```typescript
import { useEffect, useState, useCallback } from 'react';
import * as THREE from 'three';
import hotspotData from '../../../hotspots.json';

interface HotspotScreenPosition {
  id: string;
  x: number;
  y: number;
  visible: boolean;
  label: string;
  archiveSlug: string;
}

export function useHotspots(
  scene: THREE.Scene | null,
  camera: THREE.PerspectiveCamera | null,
  renderer: THREE.WebGLRenderer | null,
  isModelLoaded: boolean
) {
  const [hotspotPositions, setHotspotPositions] = useState<HotspotScreenPosition[]>([]);
  const hotspotMeshes = useRef<THREE.Mesh[]>([]);

  // Hotspot-Marker erstellen (unsichtbare Kugeln für Raycasting)
  useEffect(() => {
    if (!scene || !isModelLoaded) return;

    hotspotData.forEach(hotspot => {
      const geometry = new THREE.SphereGeometry(0.05, 8, 8);
      const material = new THREE.MeshBasicMaterial({
        transparent: true,
        opacity: 0,
      });
      const mesh = new THREE.Mesh(geometry, material);
      mesh.position.set(
        hotspot.position.x,
        hotspot.position.y,
        hotspot.position.z
      );
      mesh.userData = { hotspotId: hotspot.id, archiveSlug: hotspot.archiveSlug };
      scene.add(mesh);
      hotspotMeshes.current.push(mesh);
    });

    return () => {
      hotspotMeshes.current.forEach(m => scene.remove(m));
      hotspotMeshes.current = [];
    };
  }, [scene, isModelLoaded]);

  // Screen-Positionen jedes Frame aktualisieren
  const updateScreenPositions = useCallback(() => {
    if (!camera || !renderer) return;

    const positions: HotspotScreenPosition[] = hotspotData.map(hotspot => {
      const pos3D = new THREE.Vector3(
        hotspot.position.x,
        hotspot.position.y,
        hotspot.position.z
      );

      // Prüfen ob Hotspot vor der Kamera liegt
      const dir = pos3D.clone().sub(camera.position);
      const dot = dir.dot(camera.getWorldDirection(new THREE.Vector3()));
      const isBehind = dot < 0;

      pos3D.project(camera);

      const x = (pos3D.x * 0.5 + 0.5) * renderer.domElement.clientWidth;
      const y = (-pos3D.y * 0.5 + 0.5) * renderer.domElement.clientHeight;

      return {
        id: hotspot.id,
        x,
        y,
        visible: !isBehind,
        label: hotspot.label,
        archiveSlug: hotspot.archiveSlug,
      };
    });

    setHotspotPositions(positions);
  }, [camera, renderer]);

  // Klick-Raycasting
  const handleClick = useCallback((event: MouseEvent) => {
    if (!camera || !renderer || !scene) return;

    const raycaster = new THREE.Raycaster();
    const mouse = new THREE.Vector2(
      (event.clientX / window.innerWidth) * 2 - 1,
      -(event.clientY / window.innerHeight) * 2 + 1
    );

    raycaster.setFromCamera(mouse, camera);
    const intersects = raycaster.intersectObjects(hotspotMeshes.current);

    if (intersects.length > 0) {
      const slug = intersects[0].object.userData.archiveSlug;
      window.location.href = `/archive/${slug}`;
    }
  }, [camera, scene]);

  return { hotspotPositions, updateScreenPositions, handleClick };
}
```

**HTML-Hotspot-Overlay (`src/components/Hotspot/Hotspot.tsx`):**
```typescript
import Link from 'next/link';
import styles from './Hotspot.module.css';

interface HotspotProps {
  x: number;
  y: number;
  visible: boolean;
  label: string;
  archiveSlug: string;
}

export default function Hotspot({ x, y, visible, label, archiveSlug }: HotspotProps) {
  if (!visible) return null;

  return (
    <Link
      href={`/archive/${archiveSlug}`}
      className={styles.hotspot}
      style={{ left: x, top: y }}
    >
      <span className={styles.dot} />
      <span className={styles.label}>{label}</span>
    </Link>
  );
}
```

```css
/* Hotspot.module.css */
.hotspot {
  position: absolute;
  transform: translate(-50%, -50%);
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  text-decoration: none;
  pointer-events: auto;
  z-index: 10;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: white;
  border: 2px solid rgba(255,255,255,0.5);
  box-shadow: 0 0 0 0 rgba(255,255,255,0.5);
  animation: pulse 2s infinite;
}

.label {
  background: rgba(0,0,0,0.7);
  color: white;
  font-size: 11px;
  padding: 3px 8px;
  border-radius: 4px;
  white-space: nowrap;
  opacity: 0;
  transition: opacity 0.2s;
}

.hotspot:hover .label {
  opacity: 1;
}

@keyframes pulse {
  0% { box-shadow: 0 0 0 0 rgba(255,255,255,0.4); }
  70% { box-shadow: 0 0 0 8px rgba(255,255,255,0); }
  100% { box-shadow: 0 0 0 0 rgba(255,255,255,0); }
}
```

---

### Phase 5: Finalisierung und Deployment (Woche 6-7)

- Loading-Screen mit Fortschrittsanzeige implementieren
- Navigation zwischen 3D-View und Archiv
- Back-Button aus Archivseiten zurück zum 3D-Raum
- Keyboard-Shortcuts (ESC zum Schließen)
- Finale Performance-Tests
- Vercel Deployment (siehe Abschnitt 11)

---

## 6. Performance-Optimierung

### 3D-Modell-Optimierung

**Zielmetriken:**
| Metrik | Warnschwelle | Optimum |
|---|---|---|
| GLB-Dateigröße | >20 MB | <10 MB |
| Faces | >500.000 | <200.000 |
| Draw Calls | >100 | <30 |
| Texturen (GPU) | >200 MB | <80 MB |
| First Paint (3G) | >8s | <4s |

**Rendering-Optimierungen im Code:**

```typescript
// Pixel Ratio cappen (wichtigste Single-Optimierung)
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

// Frustum Culling ist per default an (nicht deaktivieren)
// mesh.frustumCulled = true; // Standard

// Texture Caching
const textureLoader = new THREE.TextureLoader();
const cachedTextures = new Map();
function loadCachedTexture(url: string) {
  if (!cachedTextures.has(url)) {
    cachedTextures.set(url, textureLoader.load(url));
  }
  return cachedTextures.get(url);
}

// Dispose bei Unmount (Memory Leaks verhindern)
function disposeScene(scene: THREE.Scene) {
  scene.traverse((obj) => {
    if (obj instanceof THREE.Mesh) {
      obj.geometry.dispose();
      if (Array.isArray(obj.material)) {
        obj.material.forEach(m => m.dispose());
      } else {
        obj.material.dispose();
      }
    }
  });
}
```

**Progressive Loading Strategie:**

```
1. Loading-Screen anzeigen (sofort)
2. Niedrig-aufgelöste Umgebungsfarbe/Fog anzeigen (0ms)
3. GLB laden (0ms - Start des Downloads)
4. Sobald GLB geladen: Szene einblenden
5. Texturen werden parallel vom Browser geladen
```

### Bild-Optimierung für das Archiv

Next.js Image-Komponente übernimmt automatisch:
- WebP-Konvertierung
- Lazy Loading
- `srcset` für verschiedene Viewport-Größen
- Blur Placeholder

```typescript
// Immer width/height angeben für CLS-Score
<Image
  src={entry.images[0].src}
  alt={entry.images[0].alt}
  width={1200}
  height={800}
  quality={80}
  placeholder="blur"
  blurDataURL="data:image/jpeg;base64,..."
/>
```

**Bilder vorab optimieren (einmalig, offline):**
```bash
# Sharp für Batch-Konvertierung
npm install -g sharp-cli
sharp --input public/archive/images/**/*.jpg \
      --output public/archive/images/optimized/ \
      --format webp \
      --quality 80
```

---

## 7. Archivstruktur, Content-Management und Skalierbarkeit

### Phase 1: Flat-File JSON (Empfehlung für Start)

**Skalierbarkeit:** bis ~500 Einträge problemlos, Next.js generiert alle Seiten statisch.

```
content/archive/
├── index.json          # Array mit allen Einträgen (für Listenansicht)
└── [slug].json         # Ein File pro Eintrag (für Detailseite)
```

**`index.json` enthält nur Listenfelder** (kein Body, keine großen Texte), um den Initial-Download klein zu halten:
```json
[
  {
    "slug": "work-001",
    "title": "Werktitel",
    "year": 2023,
    "medium": "Öl auf Leinwand",
    "thumbnail": "/archive/images/work-001/thumb.jpg",
    "hotspotId": "hotspot-001"
  }
]
```

### Phase 2: Notion als CMS (ab ~50 Einträgen empfehlenswert)

Notion bietet eine komfortable Editor-Oberfläche für Nicht-Entwickler und eine REST-API.

```typescript
// src/lib/archive/notionClient.ts
import { Client } from '@notionhq/client';

const notion = new Client({ auth: process.env.NOTION_SECRET });

export async function getArchiveFromNotion() {
  const response = await notion.databases.query({
    database_id: process.env.NOTION_DB_ID!,
    filter: { property: 'Status', status: { equals: 'Published' } },
    sorts: [{ property: 'Year', direction: 'descending' }],
  });

  return response.results.map(transformNotionPage);
}
```

**Notion-zu-Website-Sync via Vercel Webhook:**
Wenn eine Seite in Notion aktualisiert wird, triggert Vercel ein Rebuild via Deploy Hook.

### Skalierbarkeits-Roadmap

| Einträge | CMS | Rebuild-Zeit |
|---|---|---|
| 1-50 | JSON-Dateien | ~5s |
| 50-200 | Notion API | ~30s |
| 200+ | Sanity / Contentful | On-demand ISR |

---

## 8. Verbindung 3D-Raum und Archivinhalte

### Zwei Verbindungstypen

**Typ A: Klickbare Raum-Bereiche (Flächen-Raycasting)**

Bestimmte Flächen des Polycam-Meshes werden als Hotspot-Objekte markiert. Der Nutzer klickt auf eine Wand / ein Objekt im Raum und landet im zugehörigen Archiveintrag.

Umsetzung:
1. In Blender: Objekte/Flächen als separate Meshes exportieren (z.B. Rahmen an der Wand)
2. Im Three.js-Code: `raycaster.intersectObjects([wandrahmen, regal, ...])`
3. Klick navigiert zu `/archive/[slug]`

**Typ B: Schwebende Hotspot-Marker (HTML-Overlay)**

Unsichtbare Referenzpunkte im 3D-Raum, deren Bildschirmposition jedem Frame berechnet wird. Darüber werden HTML-Elemente positioniert (pulsierender Kreis + Label).

Dieser Typ ist in Phase 4 vollständig implementiert (siehe `useHotspots.ts`).

**Empfehlung: Typ B** für die erste Version. Typ A kann additiv ergänzt werden, erfordert aber sorgfältiger strukturierte Blender-Meshes.

### Hotspot-Koordinaten ermitteln

**Methode 1: Blender (präzise)**
1. In Blender eine leere Sphere an der gewünschten Position platzieren
2. Object Properties > Location ablesen: `X: 1.2, Y: -0.5, Z: 1.8`
3. In `hotspots.json` eintragen

**Methode 2: Browser-Konsole (iterativ)**
```javascript
// Im Browser-Dev-Modus: Klick-Position im 3D-Raum ausgeben
renderer.domElement.addEventListener('click', (e) => {
  const raycaster = new THREE.Raycaster();
  const mouse = new THREE.Vector2(
    (e.clientX / window.innerWidth) * 2 - 1,
    -(e.clientY / window.innerHeight) * 2 + 1
  );
  raycaster.setFromCamera(mouse, camera);
  const hits = raycaster.intersectObject(roomModel, true);
  if (hits.length > 0) {
    console.log('Position:', hits[0].point);
  }
});
```

### Navigation zurück zum 3D-Raum

Aus dem Archiv zurück zum 3D-Raum mit optionalem Auto-Fokus auf den Hotspot:

```typescript
// Archivseite Link:
<Link href="/?hotspot=hotspot-001">Im Raum anzeigen</Link>

// In ThreeScene: URL-Parameter auslesen und Kamera animieren
const searchParams = useSearchParams();
const targetHotspot = searchParams.get('hotspot');

useEffect(() => {
  if (targetHotspot && isModelLoaded) {
    const hotspot = hotspotData.find(h => h.id === targetHotspot);
    if (hotspot) animateCameraTo(hotspot.position);
  }
}, [targetHotspot, isModelLoaded]);
```

---

## 9. Aufwandsschätzung

Alle Angaben in Stunden für eine einzelne Person mit Next.js- und Three.js-Grundkenntnissen.

| Phase | Aufgabe | Min | Max |
|---|---|---|---|
| 0 | Polycam-Scan (inkl. Wiederholungen) | 2h | 5h |
| 0 | Blender-Nachbearbeitung + Export | 4h | 10h |
| 0 | Asset-Validierung + Koordinaten | 1h | 3h |
| 1 | Next.js-Setup + Konfiguration | 2h | 4h |
| 1 | Vercel-Setup + CI/CD | 1h | 2h |
| 2 | Three.js Viewer-Core | 8h | 16h |
| 2 | OrbitControls + Innenraum-Konfiguration | 2h | 4h |
| 2 | Loading-Screen | 2h | 4h |
| 3 | Archiv-Datenstruktur + JSON | 2h | 4h |
| 3 | Archiv-Übersichtsseite | 3h | 6h |
| 3 | Archiv-Detailseite | 3h | 6h |
| 4 | Hotspot-System (Raycasting + HTML-Overlay) | 8h | 16h |
| 4 | Kamera-Animation zu Hotspot | 2h | 5h |
| 5 | Performance-Tuning | 3h | 8h |
| 5 | Finales Styling + Navigation | 3h | 6h |
| 5 | Testing + Deployment | 2h | 4h |
| **Gesamt** | | **48h** | **103h** |

**Realistischer Zeitrahmen:** 6-8 Wochen Teilzeit (~10h/Woche) oder 2-3 Wochen Vollzeit.

**Größte Zeitfresser:**
1. Blender-Nachbearbeitung (stark abhängig von Scan-Qualität)
2. Hotspot-System (viele Edge Cases: Hotspot hinter Wand, außerhalb Canvas etc.)
3. Performance-Optimierung (iterativ, schwer vorherzusagen)

---

## 10. Risiken, technische Herausforderungen und Lösungen

### Risiko 1: Schlechte Scan-Qualität

**Symptom:** Mesh hat Löcher, Texturen fehlen, Geometrie stimmt nicht
**Wahrscheinlichkeit:** Mittel (besonders ohne LiDAR-iPhone)
**Lösung:**
- Mehrere Scans machen, besten auswählen
- Polycam Premium zahlt sich aus: mehr Verarbeitungsqualität
- Lichtverhältnisse: kein direktes Sonnenlicht, gleichmäßige Beleuchtung
- Fallback: Equirectangular 360-Foto aus Polycam exportieren und Panorama-Approach wie im Originalcode verwenden

### Risiko 2: GLB-Datei zu groß

**Symptom:** Ladezeit >10 Sekunden auf normaler Leitung
**Wahrscheinlichkeit:** Mittel-hoch (unkomprimierte Polycam-Exporte sind oft >50 MB)
**Lösung:**
- Draco-Kompression konsequent einsetzen
- Texturen auf 2048x2048 begrenzen
- Mesh-Decimation aggressiver (Ratio 0.2 statt 0.5)
- Progressive Loading: Zuerst niedrig-aufgelöste Version laden, dann hochauflösende

### Risiko 3: Draco-Decoder-Fehler

**Symptom:** `DRACOLoader: Could not load decoder` im Browser
**Ursache:** Decoder-Dateien nicht gefunden oder falsche Version
**Lösung:**
```bash
# Exakte Version aus three.js-Paket kopieren
cp -r node_modules/three/examples/jsm/libs/draco public/draco
```
Nie von CDN laden auf Produktionsseite (Version mismatch möglich).

### Risiko 4: SSR-Konflikte mit Three.js

**Symptom:** `window is not defined` beim Bauen
**Ursache:** Three.js greift auf DOM/Window zu, das bei SSR nicht existiert
**Lösung:** Konsequent `dynamic import` mit `ssr: false` für alle Three.js-Komponenten:
```typescript
const ThreeScene = dynamic(() => import('@/components/ThreeScene/ThreeScene'), {
  ssr: false
});
```

### Risiko 5: Hotspot-Positions-Drift

**Symptom:** Hotspot-Labels erscheinen an falscher Stelle im Raum
**Ursache:** Koordinatensystem-Unterschied Blender vs. Three.js, oder Modell-Zentrierung verändert Positionen
**Lösung:**
- Koordinaten erst nach finalem Modell-Import in Three.js ermitteln (Browser-Methode in Abschnitt 8)
- Modell-Position im Code dokumentieren und nicht mehr ändern

### Risiko 6: Performance auf älteren Grafikkarten

**Symptom:** Frame-Rate unter 30 FPS, Browser warnt oder abstürzt
**Lösung:**
```typescript
// Performance-Modus Detection
const maxPixelRatio = window.navigator.hardwareConcurrency <= 4 ? 1 : 2;
renderer.setPixelRatio(Math.min(window.devicePixelRatio, maxPixelRatio));

// Stats.js für Development-Monitoring
import Stats from 'three/addons/libs/stats.module.js';
const stats = new Stats();
document.body.appendChild(stats.dom);
// Im Animate-Loop: stats.update();
```

### Risiko 7: Koordinatensystem Polycam vs. Three.js

**Symptom:** Modell liegt auf der Seite oder ist gespiegelt
**Ursache:** Polycam und Blender verwenden Y-up, Three.js auch Y-up, aber Export-Einstellungen können abweichen
**Lösung in Blender-Export:**
```
Transform: Y Forward, Z Up
```
Falls im Browser trotzdem falsch:
```typescript
model.rotation.x = -Math.PI / 2; // Nur falls nötig
```

---

## 11. Deployment-Strategie Vercel

### Initialer Setup

```bash
# Vercel CLI global installieren
npm install -g vercel

# Im Projektordner
vercel login
vercel link

# Erste Deployment
vercel deploy --prod
```

### `vercel.json` Konfiguration

```json
{
  "buildCommand": "next build",
  "outputDirectory": ".next",
  "framework": "nextjs",
  "headers": [
    {
      "source": "/models/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    },
    {
      "source": "/archive/images/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    }
  ]
}
```

**Erklärung:** Die `immutable`-Header sorgen dafür, dass GLB und Bilder ein Jahr lang gecacht werden. Wenn du Dateien aktualisierst, musst du den Dateinamen ändern (z.B. `room-v2.glb`) um den Cache zu invalidieren.

### Environment Variables

```bash
# In Vercel Dashboard oder via CLI setzen:
vercel env add NOTION_SECRET production
vercel env add NOTION_DB_ID production
```

### CI/CD Workflow

```
Git Push → Vercel Build → Deploy
    |
    v
Preview-URL für jeden Branch automatisch:
https://projektname-[branch-hash].vercel.app

Production-URL bei Push auf main:
https://projektname.vercel.app
```

### Vercel Blob für große Assets (optional)

Falls `room.glb` >20 MB ist, sollte Vercel Blob statt `/public` verwendet werden:

```bash
npm install @vercel/blob
```

```typescript
// Upload via Vercel Blob (einmalig, via Script)
import { put } from '@vercel/blob';
import { readFile } from 'fs/promises';

const file = await readFile('room.glb');
const blob = await put('models/room.glb', file, { access: 'public' });
console.log(blob.url); // URL in ThreeScene verwenden
```

Vercel Blob liefert Assets über Vercel Edge Network, was Latenzen gegenüber `/public` weiter reduziert.

### Deployment-Checkliste (Pre-Production)

```
[ ] GLB-Datei unter 20 MB
[ ] Draco-Decoder in /public/draco/ vorhanden
[ ] Alle JSON-Einträge syntaktisch valide (JSON.parse test)
[ ] next build lokal erfolgreich
[ ] Keine console.error in Browser-Konsole
[ ] Ladezeit <5s auf gedrosselter 3G-Verbindung getestet (Chrome DevTools)
[ ] Alle Archiv-Slugs haben zugehörige Detailseiten
[ ] Hotspot-Positionen im Browser validiert
[ ] Cache-Control-Header konfiguriert
[ ] Custom Domain in Vercel gesetzt (falls vorhanden)
[ ] HTTPS aktiv (automatisch via Vercel)
```

---

## Anhang: Entscheidungsbaum Panorama vs. GLB

```
Polycam-Export-Strategie:

Hast du ein iPhone mit LiDAR (Pro/Pro Max)?
├── JA: LiDAR-Room-Scan → GLB-Export → Blender → Web
│         Vorteil: echte 3D-Geometrie, walk-around möglich
│         Nachteil: komplexer, größere Dateien
│
└── NEIN: Photo-Scan oder 360-Export wählen
          ├── Scan-Qualität ausreichend? → GLB (reduzierte Qualität)
          └── Scan unbrauchbar → 360-Foto aus Polycam exportieren
                                  → Equirectangular → Skybox (wie Original-Code)
                                  → Vorteil: immer funktioniert
                                  → Nachteil: kein echtes 3D, nur Panorama
```

**Empfehlung:** Immer zuerst den GLB-Approach versuchen. Falls die Qualität unzureichend ist, ist der Fallback auf den Panorama-Approach (der dem bestehenden Code entspricht) jederzeit möglich und erfordert nur das Austauschen des Loaders.
