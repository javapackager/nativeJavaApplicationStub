# nativeJavaApplicationStub

Native macOS launcher for Java apps, used by [JavaPackager](https://github.com/javapackager/JavaPackager) as the executable inside the `.app` bundles it generates (`macStartup` = `UNIVERSAL`, `ARM64` or `X86_64`; `UNIVERSAL` is the default since JavaPackager 2.0.0).

It's an Objective-C program (`src/nativeStub.m`) that reads the Java settings from the app's `Info.plist` (Apple style `Java`/`JavaX` dictionary or Oracle style keys), looks for a suitable JVM (the bundled one in `Contents/PlugIns`, then the installed ones), runs the optional bootstrap script and launches the main class. Running as a native binary, macOS sees the app itself rather than a shell, so it runs natively on Apple Silicon and Intel without Rosetta.

## Build

On macOS, with the Xcode command line tools:

```bash
make universal
```

This builds `build/arm64/`, `build/x86_64/` and `build/universal/nativeJavaApplicationStub`. Every push and pull request is built by the `Build` workflow.

## Release

Run the **Compile and release** workflow (Actions tab). It creates a release named after the date (e.g. `20251202.010109`) with three assets: `nativeJavaApplicationStub` (universal), `nativeJavaApplicationStub.arm64` and `nativeJavaApplicationStub.x86_64`.

To use a release in JavaPackager, set its version in the `updateNativeJavaApplicationStub` task of JavaPackager's `build.gradle` and run `./gradlew updateNativeJavaApplicationStub`, which downloads the three binaries into `src/main/resources/mac`.

## History

This repository started as a fork of [tofi86/universalJavaApplicationStub](https://github.com/tofi86/universalJavaApplicationStub), a Bash launcher that is now deprecated. The native launcher was written here as its replacement and follows the same `Info.plist` conventions. The Bash script used with `macStartup=SCRIPT` is now maintained in JavaPackager (`src/main/resources/mac/universalJavaApplicationStub.sh`).

## License

[MIT](LICENSE)
