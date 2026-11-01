name: build
on: [push, workflow_dispatch]
jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 8
      - name: Preparar projeto
        run: |
          unzip -q telamod-1.8.9-fonte.zip
          curl -L -o mdk.zip https://maven.minecraftforge.net/net/minecraftforge/forge/1.8.9-11.15.1.2318-1.8.9/forge-1.8.9-11.15.1.2318-1.8.9-mdk.zip
          unzip -q mdk.zip -d mdk
          rm -rf mdk/src/main/java/com/example
          cp -r telamod/src mdk/
          cp telamod/build.gradle mdk/build.gradle
      - name: Compilar
        run: cd mdk && chmod +x gradlew && ./gradlew build --no-daemon
      - uses: actions/upload-artifact@v4
        with:
          name: telamod
          path: mdk/build/libs/*.jar
