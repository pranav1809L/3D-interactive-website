# iPhone — Every Layer

React 19 + TypeScript + Vite. A 2.5D product gallery using supplied photography and deterministic scroll-scrubbed image sequences. No invented phone geometry or internal hardware.

## Run

```sh
npm ci
npm run dev
npm run build
npm test
```

## Edit

- `src/data.ts`: one phone object per generation, verified specification fields, source links, colors, hero framing, and feature chapters. Add a definition to add a generation; navigation and comparison derive from this collection.
- `src/types.ts`: shared phone and animation contracts.
- `src/Exploded.tsx`: sequence renderer, video seeking renderer, and registration hook for a future GLTF renderer.
- `public/phones/iphone15` through `iphone18`: byte-identical uploaded hero JPEGs, MP4 originals, and extracted frame sequences. The additional iPhone 15 PNG is preserved as `front.png`.
- `src/style.css`: theme interpolation, product perspective, studio presentation, mobile/landscape layouts, and reduced-motion support.

The hero images include light backgrounds, and some have low native resolutions (225px for the 15). CSS places them in a soft studio stage; it does not reconstruct or change hardware. Animation crops, lighting and camera movement already baked into the supplied files are preserved. The supplied visuals are an independent visual study and are not represented as verified internal engineering diagrams.

## Sequence timing

Frame numbers below are one-based and inclusive. Full original sequences remain in the project.

| Model | Assembled start | Exploded end | Original frames |
| --- | ---: | ---: | ---: |
| 15 | 129 | 189 | 189 |
| 16 | 84 | 134 | 134 |
| 17 | 79 | 163 | 240 |
| 18 concept | 85 | 174 | 192 |

The iPhone 17 source reassembles at the end; its scroll range deliberately ends before reassembly. These are the original split boundaries. The updated main timeline starts from original frame 1: 0–18% presentation, 18–42% source rotation to the model’s rearFrame, 42–55% assembled rear progression to the split start, 55–90% separation, 90–100% final hold. Reverse scrolling reverses the same mapping. The accessible range input overrides scroll progress until Resume scroll control is clicked.

## Replace assets

Keep filenames to swap visuals without code changes, or change the central data fields. Specify `kind: 'sequence'` with the inclusive start/end frame numbers and poster; `kind: 'video'` with src/startTime/endTime/poster to use MP4/WebM seeking. The original video is retained in each sequence object's `videoFallback` field for future editorial switching, but frame errors currently offer retry rather than automatically switching rendering modes. Video seeks wait for the current seek to finish and always converge on the latest scroll position.

For a genuine GLB/GLTF, register a model renderer with `registerModelAdapter(factory)`, returning `setProgress` and `dispose`. The renderer is intentionally not bundled without a model. Until an adapter is registered (or on rendering failure), the supplied poster stays visible. This avoids inaccurate substitute geometry and unnecessary WebGL dependencies.

## Data and factual content

15/16/17 display sizes, chip names and playback estimates link to Apple Support in the model data and comparison. iPhone 18 is explicitly a concept, as requested, with null technical fields. All labels baked into supplied visuals remain untouched and may differ from verified product specifications. Sources are documented per model rather than inferred from render labels.

## Performance and accessibility

All four lightweight hero images are shown in the lineup; SVG presentation masks isolate their silhouettes without changing the source files. Frame loading starts just before the opening crossfade, uses four parallel requests and a bounded decoded cache (12 frames on mobile, 22 on desktop), and prioritizes the target frame after fast scrolling. Images render at their native resolution. No continuous canvas render loop runs when idle. The UI uses browser scrolling and CSS sticky, avoiding a smooth-scroll wrapper competing with touch gestures. Reduced-motion disables ambient transforms and transitions while retaining direct scroll control. Controls support keyboard, touch, accessible names, a skip link, visible focus, a native comparison dialog, and a manual separation slider.

## Verification

TypeScript and production Vite build pass. Automated tests cover reversible frame mapping, overscroll bounds, sticky-section progress, gallery/chapter limits, source asset existence, and binary preservation. The supplied frames were visually inspected for timing. The supervised preview was started. An attempted browser visit was rejected with net::ERR_BLOCKED_BY_CLIENT, so browser-based desktop/mobile QA could not be completed; responsive appearance and native pointer/scroll behavior still require browser review. No claim of measured 60 FPS is made.

## Continuous gallery update

The existing App, Story, Exploded and model architecture are retained. SELECT and the separate selection gate are removed. The same pinned opening section now combines horizontal browsing and vertical flip/split control. Existing feature chapters and the summary follow it. Comparison and per-model URL hashes are retained.

- Fractional scrubber values drive live phone positions and RGB theme interpolation. Releasing snaps to the nearest generation.
- Horizontal pointer gestures lock to X after an 8px threshold. Vertical and ambiguous diagonal gestures stay native. Horizontal wheel input is used only when it dominates Y (or Shift-wheel is used). Vertical wheel events are never cancelled or used to choose models.
- Navigation to another model returns the timeline to its start. Scrolling backward reverses the same deterministic frame map.
- Desktop keeps all four renders in the DOM with depth, scale and lighting hierarchy. Mobile exposes the neighboring silhouettes at the viewport edges.
- Hero SVG masks reference the unmodified original images. All original raster files and sequences remain byte-identical; no generated hardware is substituted. The animation backgrounds and any baked zoom remain as supplied.
- Theme lighting continues behind the source footage and feature chapters. The original sequence footage is presented with a soft edge mask; its baked-in studio background is retained rather than modifying internal component pixels.
- Eight automated checks now include directional gesture classification, continuous color interpolation, full flip/rear/split phase boundaries, monotonicity and reverse mapping.
