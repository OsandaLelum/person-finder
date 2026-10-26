# Person Finder

Find every photo of one person across your drives (Windows: C:, D:, G:, H:, USB, network; macOS: home, Pictures, external drives).
Free, 100% offline. JPG, PNG, HEIC (+ WEBP, BMP, TIFF).

## Publish free Windows + Mac downloads (recommended: GitHub Releases)

You need a Mac to build a Mac app. GitHub provides free Windows and Mac build machines, so you don't need one.

1. Create a free account at github.com and a **public** repository, e.g. `person-finder`.
2. Upload this whole folder, **including `models/` and `.github/`** (`.gitignore` keeps build output and your face cache out) (GitHub Desktop or `git push`).
3. In the repo: **Releases → Draft a new release → tag `v1.0.0` → Publish**
   (or push a tag: `git tag v1.0.0 && git push --tags`).
4. Open the **Actions** tab. "Build installers" runs for about 10–15 minutes and attaches the files to the release:
   - `PersonFinder-Setup-1.0.0-Windows.exe` – Windows installer
   - `PersonFinder-1.0.0-Windows-portable.zip`
   - `PersonFinder-1.0.0-macOS-arm64.dmg` – Apple Silicon Macs
   - `PersonFinder-1.0.0-macOS-x86_64.dmg` – Intel Macs
5. Website: copy `website/person-finder.html` and `website/person-finder-icon.png` to your site.
   Edit `GITHUB_REPO` near the bottom of the HTML. The buttons then always point to the newest release.
6. New version: change `APP_VERSION` in `face_engine.py`, then publish a new tag (`v1.0.1`).

### Warnings users will see (unsigned apps)
- **Windows SmartScreen**: "Windows protected your PC" → *More info → Run anyway*.
  You can remove it with a code-signing certificate (or Microsoft Store / Azure Trusted Signing).
- **macOS Gatekeeper**: "cannot be opened" → *System Settings → Privacy & Security → Open Anyway*.
  You can remove it with an Apple Developer account (USD 99/year). Add these repo secrets and the workflow signs and notarizes automatically:
  `APPLE_CERT_P12` (base64 Developer ID Application .p12), `APPLE_CERT_PASSWORD`, `APPLE_SIGN_IDENTITY`
  (e.g. `Developer ID Application: Osanda Lelum (TEAMID)`), `APPLE_ID`, `APPLE_TEAM_ID`, `APPLE_APP_PASSWORD`.

## Build locally
- Windows: `build_exe.bat` (installer too if Inno Setup 6 is installed) → `release\`
- Mac: `bash build_mac.sh` → `release/*.dmg`
- Run from source: `run.bat` (Windows) or `pip install -r requirements.txt && python3 person_finder.py`
- Health check: `PersonFinder --selftest result.txt` (exit code 0 = OK)

## Project layout
```
person_finder.py        GUI (Tkinter)
face_engine.py          face detection/recognition, scanner, cache   (APP_VERSION lives here)
models/                 YuNet + SFace ONNX models (bundled into the apps)
assets/                 app icons (png / ico / icns)
PersonFinder.spec       PyInstaller build (Windows .exe + macOS .app)
installer/              Inno Setup script (Windows) and DMG script (macOS)
.github/workflows/      automatic Windows + Mac builds
website/                download page for your site
```

## Notes
- User data (face cache, downloaded models) goes to `%LOCALAPPDATA%\PersonFinder` (Windows) or
  `~/Library/Application Support/PersonFinder` (Mac). The About dialog includes a "Clear face cache" button.
- HEIC is decoded with pi-heif (decode-only, so no GPL encoder is bundled). See `THIRD_PARTY_LICENSES.txt`.
- The app is MIT-licensed (`LICENSE.txt`). If you want a different license, change it before publishing.
"# person-finder" 
