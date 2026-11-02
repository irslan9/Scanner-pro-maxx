# Scanner Pro max

Native Kotlin, Material 3, CameraX, Room. Fully offline. No ML Kit, no OpenCV, no cloud, no ads, no accounts.

## Build
1. Open this folder in Android Studio (Koala or newer, JDK 17). Let Gradle sync (needs internet once to download libraries).
2. Debug APK:   Build > Build APK(s)            -> app/build/outputs/apk/debug/
3. Release APK: Build > Generate Signed App Bundle / APK, or `./gradlew assembleRelease`
   (release is signed with the debug key so it installs directly; use your own keystore to publish).
4. Min Android 8.0 (API 26).

## OCR (offline, Tesseract)
Download eng.traineddata and urd.traineddata from https://github.com/tesseract-ocr/tessdata_fast
and either put them in app/src/main/assets/tessdata/ before building, or import them in the app:
Settings > Import OCR language file.

## How it works
- Edge detection: Otsu threshold + largest-region corner search (ImageProc.kt). Adjust corners manually in Crop.
- Perspective correction: android.graphics.Matrix.setPolyToPoly.
- Filters: colour matrices, shadow removal (paper-colour division), adaptive threshold, sharpen kernel.
- Originals are always kept; filter/crop/rotation are stored per page and re-rendered on change.
- PDF: own JPEG-based writer (PdfWriter.kt); quality sets resolution + JPEG level.
- Email: Android share sheet (Gmail or any mail app). Only the recipient address is stored; no passwords.
- Backup: manual .zip file (restore adds documents). No automatic cloud backup.
