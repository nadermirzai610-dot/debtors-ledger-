name: Android APK
on:
  push:
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - name: Install dependencies
        run: npm install
      - name: Prepare web folder and font
        run: |
          mkdir -p www/fonts
          cp index.html www/index.html
          F=$(find node_modules/vazirmatn -iname 'Vazirmatn*wght*.woff2' | head -1)
          if [ -n "$F" ]; then cp "$F" www/fonts/Vazirmatn.woff2; else echo "font not found"; fi
      - name: Add and sync Android
        run: |
          npx cap add android
          npx cap sync android
      - name: Build debug APK
        run: cd android && chmod +x gradlew && ./gradlew assembleDebug
      - uses: actions/upload-artifact@v4
        with:
          name: debtors-ledger-apk
          path: android/app/build/outputs/apk/debug/app-debug.apk
