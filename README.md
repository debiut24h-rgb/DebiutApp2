name: Build Android APK (Offline PWA)

on:
  workflow_dispatch:
  push:
    branches: [ main, master ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Java 17
        uses: actions/setup-java@v4
        with:
          distribution: 'temurin'
          java-version: '17'

      - name: Setup Android SDK
        uses: android-actions/setup-android@v3
        with:
          packages: |
            platform-tools
            platforms;android-34
            build-tools;34.0.0

      - name: Make gradlew executable
        run: chmod +x gradlew

      - name: Build debug APK
        run: ./gradlew assembleDebug --stacktrace

      - name: Upload APK artifact
        uses: actions/upload-artifact@v4
        with:
          name: DebiutOfflineApp-debug-apk
          path: app/build/outputs/apk/debug/app-debug.apk
