# Managed Python runtime release (opt-in)

This additive release does NOT replace the original text or host-Python indexes. Existing authorizations do not opt in to runtime downloads.

Index: https://raw.githubusercontent.com/saya-ashen/pi-capability-registry/main/releases/python-managed-v1/index.json

Index SHA256: `6a8240394260be39ec1497b1f378acc87cbe8cf2d5c7ef6cd589a70ed791b1bd`

| Package | Capability | SHA256 |
|---|---|---|
| general-coding@0.1.0 | python.author | a66ecdc7dac13e0422e1c948d08a44afe808918c73e2c6584145abf928934b6f |
| python-managed@0.1.0 | python.execute | dc5bace72c1e15ed5c02d6148400bac0424114f4d0af7c48cc09092ee2912c5a |
| python-runtime@0.1.0 | python.runtime (resource; no model tools) | 21e12862c208d76c404b232043173c69187873f44e0a3ea4f06ca2d4e5bb62a0 |

## New opt-in permission

The runtime package declares `permissions.network: "host-artifact"` and a fixed `runtime` descriptor. It requires a client supporting that contract; older clients must reject it.

The host may download CPython 3.13.15 from Astral python-build-standalone release 20260929:

- Artifact: `cpython-3.13.15+20260929-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz`
- SHA256: `c552deaf9ed0e678dee4ccc760d6dcd7cab33f1bd43e8037123b4a4db8508e14`
- Actual published compressed size: 35,004,265 bytes. Download limit 64 MiB; decompressed archive limit 256 MiB.
- Download URL is pinned in the manifest. Only a single HTTPS redirect to release-assets.githubusercontent.com is allowed, with no model credentials.
- Licensing and provenance: https://github.com/astral-sh/python-build-standalone ; license files are retained in the installation.

The client checks archive digest, paths, entry types and internal links before extracting into a private staging directory. An executable version/stdlib probe is required before atomic publication; installation locks prevent simultaneous cooperating installers. A complete tree fingerprint detects later drift when the runtime is checked again.

## Lifetime

Matching host Python (exact version and successful probe) may be reused. Otherwise installation is under the client's persistent private package cache, never system directories. Linux x64 and a compatible glibc loader/base libraries are still required.

python.execute creates a task-local venv (`--without-pip`) using the runtime resource; no Shell dependency. The runtime is a lifetime dependency because task venvs reference its standard library. Releasing the task removes the venv but retains the shared runtime cache. No sudo, global PATH update or package manager installation is used.

Initial validation used PATH empty, real pinned download, real venv execution (338350 and 76127), then reuse with network calls disabled. This is not a clean-container or cross-distribution compatibility certification.

Source authorization covers entire trusted packages, not OS sandboxing. Review the runtime descriptor and code before granting exact package hashes. No default trust configuration is published or changed by this release.
