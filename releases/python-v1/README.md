# Python dependency release (python-v1)

Additive publication: the original `/registry/index.json` remains unchanged.

Index: https://raw.githubusercontent.com/saya-ashen/pi-capability-registry/main/releases/python-v1/index.json

Index SHA256: `d8e0bb178c4cc99bd67ae0b0ec9514a878fbd8d7ebc0a3c8308fef9da84f2025`

| Package | Capabilities | Bundle SHA256 |
|---|---|---|
| python-kit@0.1.0 | python.author, python.execute | a5972a90cfb3762e32b7cb93bda99161a48136544afc42217d6adfb4f6260d2e |
| shell-kit@0.1.0 | shell.execute | 5081ab931b28c02365bde39a2bf34c724de4a013686aa2d3e01048b8a138b887 |
| remote-text@0.1.0 | text.statistics | 372cb2fc800be90a1312c7dceaa92554625716b8bca209c314558c3353dff254 |

Use the client's `--registry-only --registry LOCAL_TRUST_FILE` to disable bundled local capabilities. Otherwise built-in Python/Shell packages have duplicate package versions and are deliberately rejected, not silently replaced.

## Execution and authorization

- Requesting python.execute requires authorization for BOTH python-kit and shell-kit at their exact hashes, before any lifecycle code runs.
- A preparation-only shell dependency does not automatically expose shell_run to the model. Only python_run is exposed when python.execute is requested.
- Package authorization is not dependency-only permission: explicitly requesting shell.execute can expose shell_run. Code tools are NOT sandboxed.
- Python.author provides guidance only and does not need the shell dependency.
- Host Linux, Python 3 and POSIX sh must already exist. Preparation creates a per-task venv with `--without-pip`; no interpreter or third-party dependency download.
- A new task checks/prepares its own environment even if package bundles are cached.
- Do not overwrite this publication path to change its contents; create a new release path and deliberately update local pins.

Trust is local: review the bundle sources before adding precise allow entries. This repository does not contain the user's local trust configuration or credentials.
