# Development

This page is for contributors who want to modify Zenject's source code. It covers how to set up the project locally, run tests to validate your changes, and deploy a build.

## Prerequisites

- Unity Editor 6 or newer (6000.x).

## Local Installation

The quickest way to get an importable `.unitypackage` to test in your game:

1. Open `UnityProject` in Unity.
2. In the Project window, right-click `Assets/Plugins/Zenject`.
3. Select **Export Package...**.
4. Uncheck **Include dependencies**.
5. Click **Export...** and choose an output path.

Import the resulting `.unitypackage` into your game project via **Assets → Import Package → Custom Package...**.

Check [Installation section](../README.md#installation) for more information about all installation methods.

## Tests

### Unity tests

Most tests are run from the Unity project in `UnityProject`.

- Open `UnityProject` in Unity.
- Use the Unity Test Runner to run Edit Mode and Play Mode tests.
- Relevant test assemblies include files such as:
  - `UnityProject/Zenject-UnitTests-Editor.csproj`
  - `UnityProject/Zenject-IntegrationTests.csproj`
  - `UnityProject/Zenject-IntegrationTests-Editor.csproj`

### Command-line Unity test run (example)

You can also run tests in batch mode from the command line.

<small>Replace `<UNITY_6_VERSION>` with your exactly installed version.</small>

```bash
/Applications/Unity/Hub/Editor/<UNITY_6_VERSION>/Unity.app/Contents/MacOS/Unity \
  -batchmode \
  -projectPath UnityProject \
  -runTests \
  -testPlatform EditMode \
  -testResults Logs/editmode-results.xml \
  -quit
```

Repeat with `-testPlatform PlayMode` for Play Mode tests.

### CI reference

GitHub workflows run Unity tests and builds using GameCI/webbertakken actions.

See:

- `.github/workflows/ci.yml`
- `.github/workflows/main.yml`

## Deployment

A release goes through two steps: CI validates it, then a git tag makes it available to consumers.

### CI Pipeline

Two GitHub Actions workflows run automatically on every push to `master`:

- **`ci.yml`** — builds `Zenject-usage.dll` (via `dotnet build`), runs EditMode tests, and builds the Unity project for all supported platforms (Android, iOS, Linux, macOS, Windows, WebGL).
- **`main.yml`** — runs on PRs and pushes to `master`, repeating tests and platform builds as a gate.

Both workflows use [GameCI](https://game.ci/) actions. A push should only reach the release step once both workflows are green.

### Releasing a new version

1. Update the `version` field in `UnityProject/Assets/Plugins/Zenject/package.json`.
2. Commit and push to `master`.
3. Once CI passes, tag the commit and push the tag:

```bash
git tag v<version>
git push origin v<version>
```

The tag is what UPM consumers use to pin a specific version. There is no separate build step — the package is served directly from the repository via the git URL.

### UnityPackage release (optional)

To also produce a versioned `.unitypackage` for manual distribution, run the release helper scripts after tagging:

```bash
cd Build
python3 -m mtm.zen.CreateRelease
```

This builds `Zenject-usage.dll`, strips test files, and outputs versioned packages to `Build/Dist/`. Requires Python 3.7+ and `pyyaml` (`pip install pyyaml`).
