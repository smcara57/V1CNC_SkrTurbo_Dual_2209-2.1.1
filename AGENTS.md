# Repository Guidance

## Project Shape

- This is a Marlin 2.1.1 firmware fork for a V1CNC machine using an SKR Turbo v1.4, TMC2209 drivers, dual XY steppers, CNC settings, a dummy extruder, and a RepRap Discount Full Graphic Smart Controller. Treat the existing machine configuration as intentional.
- `Marlin/` is the PlatformIO source root. Firmware implementation is mainly under `Marlin/src/`; board and MCU definitions are under `Marlin/src/pins/` and `Marlin/src/HAL/`.
- `buildroot/` contains Marlin build, formatting, and test tooling. `ini/` contains PlatformIO environment fragments. `config/` contains configuration documentation and examples.

## Configuration Rules

- Put machine and feature changes in [Marlin/Configuration.h](Marlin/Configuration.h) or [Marlin/Configuration_adv.h](Marlin/Configuration_adv.h). Check both files before changing defaults or feature gates.
- Preserve the fork's board, driver, CNC, dual-axis, LCD, and dummy-extruder settings unless the task explicitly changes the hardware target.
- Do not confuse [Marlin/config.ini](Marlin/config.ini) with the active machine configuration. It defines optional PlatformIO configuration presets and build-time configuration behavior.
- When a configuration change causes a compile error, prefer fixing the configuration or the owning sanity check over weakening `Marlin/src/inc/SanityCheck.h`.

## Build and Test

Run commands from the repository root:

```text
pio run
pio run -e LPC1769
pio run --target clean -e LPC1769
```

- `pio run` uses the default `LPC1769` environment from [platformio.ini](platformio.ini). Use `pio run -e <environment>` when validating another board.
- Run a focused firmware test with `make tests-single-local TEST_TARGET=<target>`. Add `ONLY_TEST=<name-or-1-based-index>` to narrow it further.
- Use `make tests-single-local-docker TEST_TARGET=<target>` when local PlatformIO dependencies are unavailable.
- `make tests-all-local` depends on `get_test_targets.py` and `.github/workflows/test-builds.yml`; this checkout may not include that workflow, so prefer focused tests and report that limitation.
- Avoid `GIT_RESET_HARD=true`; test tooling can reset all local changes.
- Run `buildroot/bin/format_code <file-or-directory>` for C/C++ formatting when a change requires it. Do not format unrelated files.

## Windows Environment

- `make`, `mftest`, `run_tests`, and `format_code` are Bash-oriented tools. Use Git Bash, WSL, or the repository's Docker workflow rather than assuming PowerShell compatibility.
- PlatformIO and the PlatformIO VS Code extension are prerequisites for firmware builds. Docker uses the legacy `docker-compose` command in the existing Makefile.

## Change and Validation Practices

- Keep firmware edits small and local to the owning module. Follow nearby Marlin naming, macro, include, and two-space C/C++ indentation patterns.
- For hardware-facing changes, inspect the relevant board pin map and compile the affected `LPC1769` environment before broad tests.
- Validate configuration changes with a PlatformIO build. Validate behavior changes with the narrowest relevant test target; do not claim hardware behavior from compilation alone.
- Check `git diff` and `git status` before finishing. Do not revert unrelated user changes, including the existing `.vscode/extensions.json`, `Marlin/Configuration.h`, or workspace-file changes.

## Useful References

- [README.md](README.md) for this fork's intended hardware and feature set.
- [Makefile](Makefile) for local and Docker test targets.
- [platformio.ini](platformio.ini) for environments, source filters, and build scripts.
- [buildroot/bin/mftest](buildroot/bin/mftest), [buildroot/bin/run_tests](buildroot/bin/run_tests), and [buildroot/bin/format_code](buildroot/bin/format_code) for repository tooling.
- [config/README.md](config/README.md) and [test/README](test/README) for configuration and test context.
- [.editorconfig](.editorconfig) for whitespace and line-ending conventions.
