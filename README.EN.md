# MMD Studio — Test Build v1.0

MMD-focused iOS PMX geometry viewer foundation.

## What v1.0 test build includes
- SwiftUI iOS app shell (iOS 17+)
- PMX 2.0 / 2.1 header parsing
- Vertex and triangle index parsing
- BDEF1, BDEF2, BDEF4, SDEF and QDEF vertex-record skipping
- Texture path and material metadata parsing
- Basic Metal indexed-triangle rendering
- Drag to orbit and pinch / buttons to zoom
- GitHub Actions workflow to build an unsigned IPA artifact

## Not included in this test build
- Texture image loading / PMX material shading
- Bone hierarchy and GPU skinning
- Morph animation
- VMD / VPD import and playback
- IK and MMD physics
- Camera/light editor
- Offline high-quality still rendering and MOV export
- PMX write-back/export

This is a **first geometry-viewer test build**, not yet a feature-complete MMD renderer. The parser currently stops after the material section; it does not yet parse the remaining PMX bone, morph, display-frame, rigid-body, and joint sections.

## Build with GitHub Actions
1. Create a GitHub repository and upload the contents of this folder.
2. Push to `main` or `master`, or run the `iOS Build` workflow manually.
3. Download the `MMDStudio-unsigned-IPA` artifact from the workflow run.

The IPA is unsigned. Installing on a physical iPhone requires Apple code signing and provisioning. The workflow build must complete successfully on GitHub's macOS runner before treating the artifact as verified.

## Local build
Requires macOS, Xcode 16+, and XcodeGen:
```bash
brew install xcodegen
xcodegen generate
xcodebuild -project MMDStudio.xcodeproj \
  -scheme MMDStudio \
  -sdk iphoneos \
  -configuration Release \
  -derivedDataPath build \
  CODE_SIGNING_ALLOWED=NO
```

## First device test
Use a small, known-good PMX 2.0/2.1 model. Keep its texture files alongside the PMX for future texture-support milestones; this v1.0 build currently renders a diagnostic solid material. Test:
- PMX file picker and parsing status
- Model name and geometry counts
- Geometry visible in the viewport
- Drag orbit and zoom controls
- Large model import memory behavior

## Project structure
- `App/`: SwiftUI application and import flow
- `Core/`: PMX data structures and binary parser
- `Renderer/`: Metal preview renderer and shader
- `.github/workflows/`: unsigned IPA build
