# Build

## Prerequisites

- `msbuild` — via Visual Studio 2015+ on Windows, or [Mono](https://www.mono-project.com/download/stable/) on macOS/Linux
- Python 3.7+ for the release helper scripts in the `Build` folder

> Projects target .NET Framework 3.5. The `dotnet` CLI cannot build them on macOS/Linux — use `msbuild` from Mono instead.

## Build Zenject-usage.dll

`Zenject-usage.dll` contains the Zenject interfaces (`ITickable`, `IInitializable`, `IDisposable`, etc.) for use in external DLLs that reference Zenject types without depending on the full Unity project.

From `AssemblyBuild/Zenject-usage/`, run:

```bash
msbuild Zenject-usage.sln                           # Debug → bin/Debug/
msbuild Zenject-usage.sln /p:Configuration=Release  # Release → Source/Usage/ (auto-copied)
```

## Using Zenject Outside Unity Or For DLLs

For external DLLs included in Unity, constructor injection works out of the box. For member/method injection, reference `Zenject-Usage.dll`.

For a standalone non-Unity project, build `NonUnityBuild/Zenject.sln`, which includes Zenject core, Signals, Zenject-usage, and the ReflectionBakingCommandLine tool. Output appears in the `Bin` folder.

> The non-Unity build automatically defines `NOT_UNITY3D` and `ZEN_MULTITHREADING`, excluding all Unity-specific code. For unit tests outside Unity, also define `ZEN_TESTS_OUTSIDE_UNITY`.

### Baking External DLLs

Build `Zenject-ReflectionBakingCommandLine` from `NonUnityBuild/Zenject.sln` and add it as a post-build step. See [Reflection Baking](../README.md#reflection-baking).
