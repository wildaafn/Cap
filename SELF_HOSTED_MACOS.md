# Cap Wilda for macOS

This fork includes a dedicated macOS build profile for a self-hosted Cap deployment.

## What this profile changes

- The installed app is named `Cap Wilda`.
- The macOS bundle identifier is `com.wildaafn.cap`, so it can coexist with the official Cap app.
- The official Cap updater is disabled. Updates come from this fork.
- The desktop app uses `VITE_SERVER_URL` from the local `.env` file.

Cap's backend treats deployments without `NEXT_PUBLIC_IS_CAP=true` as self-hosted. Self-hosted users receive the application entitlements that do not depend on Cap Cloud billing. Features that call external services still require the corresponding provider configuration, such as an S3-compatible store and optional transcription or AI API keys.

## Local setup

```bash
bun install
bun run env-setup
bun run cap-setup
```

Choose both Desktop and Web during environment setup, then choose Docker for local MySQL and S3.

Do not add `NEXT_PUBLIC_IS_CAP=true` to a self-hosted environment. That value enables Cap Cloud billing behavior.

## Build the macOS app

```bash
bun run tauri:build:wilda
```

The macOS application bundle is generated under `target/release/bundle/macos/Cap Wilda.app` for the native architecture.

The build is intended to connect to the URL configured as `VITE_SERVER_URL`. For a local deployment, the default setup uses `http://localhost:3000`.

Building locally requires the full Xcode application. Command Line Tools alone are not sufficient. When Xcode is unavailable, run the `Build Cap Wilda for macOS` workflow from the GitHub Actions page. It produces a `cap-wilda-macos-arm64` artifact containing the application ZIP.

## Service-dependent features

The recorder and local editor run on the Mac. Sharing, collaboration, accounts, organizations, and browser playback require the self-hosted web stack. Storage requires S3 or an S3-compatible service such as MinIO. Transcription and AI generation require one of the providers supported by `packages/env/server.ts`.

Review the repository license before distributing the app or using it commercially.
