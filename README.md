name: Build Android APK

on:
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Unzip Android project
        run: |
          unzip -o AlkafelLiveTV_Project.zip -d project
          ls -la project
          ls -la project/AbbassiaLiveTV

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Setup Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Build APK
        working-directory: project/AbbassiaLiveTV
        run: gradle assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: Alkafel-Live-TV-APK
          path: project/AbbassiaLiveTV/app/build/outputs/apk/debug/app-debug.apk
