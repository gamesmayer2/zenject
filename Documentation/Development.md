# Development

This page is for contributors who want to extend or collaborate on Zenject.

## Build

### Prerequisites

- Unity Editor 6 or newer (6000.x).
- .NET SDK / MSBuild for C# solution builds.
- Python 3.7+ for the release helper scripts in the `Build` folder.

### Build Zenject-usage.dll

The `Zenject-usage.dll` assembly is built from the solution in `AssemblyBuild/Zenject-usage`.

On macOS/Linux:

```bash
cd AssemblyBuild/Zenject-usage
dotnet build Zenject-usage.sln
```

On Windows (Developer Command Prompt):

```bat
cd AssemblyBuild\Zenject-usage
msbuild Zenject-usage.sln
```

If needed for local Unity workflows, place the produced DLL in:

`UnityProject/Assets/Plugins/Zenject/Source/Usage`

### Release helper scripts

The repository includes packaging/build helper scripts in `Build/`:

- `CreateRelease` / `CreateRelease.bat`
- `CreateSampleBuilds` / `CreateSampleBuilds.bat`

These scripts call Python modules under `Build/python/mtm/zen`.

Example (macOS/Linux):

```bash
cd Build
python3 -m mtm.zen.CreateRelease
python3 -m mtm.zen.CreateSampleBuilds
```

If dependencies are missing, install `pyyaml` (also listed in `Build/Pipfile`).

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
