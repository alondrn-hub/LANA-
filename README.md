# LANA DEV Windows releases

This repository hosts the reviewed source snapshot and GitHub Actions workflow used to publish LANA DEV's signed Windows installer.

## One-time GitHub setup

Move `windows-release.yml` to `.github/workflows/windows-release.yml` in this repository. GitHub requires the account editing a workflow file to have its separate `workflow` permission.

Then configure these repository Actions secrets (their values are never committed):

- `LANA_SIGNING_CERTIFICATE_PFX_BASE64` — base64 encoding of the owner-controlled PFX.
- `LANA_SIGNING_CERTIFICATE_PASSWORD` — password for that PFX.
- `LANA_SIGNING_CERTIFICATE_THUMBPRINT` — the imported certificate's SHA-1 thumbprint.

After setup, open **Actions → Publish signed Windows installer → Run workflow**, enter the release tag, and wait for the installed-app smoke test. On success, the workflow publishes `LANA-DEV-Setup.exe` and `LANA-DEV-Setup.exe.sha256` together in the GitHub Release.

The workflow verifies the reviewed source snapshot SHA-256 before extracting it, provisions the pinned local runtime and model, validates upstream hashes, signs and validates all runtime executables and DLLs, verifies the installed local model starts, and only then creates the release.
