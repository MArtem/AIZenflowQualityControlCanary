# AIZenflow Quality Control Canary

Public synthetic consumer for manually validating
[`MArtem/AIZenflowQualityControl`](https://github.com/MArtem/AIZenflowQualityControl) against a
small buildable Xcode project.

## Current Scope

- one deterministic macOS command-line Xcode target;
- manual `static` and authenticated `build-evidence` workflow modes;
- one Git-tracked schema-version-2 profile whose repository-relative sandbox resolves unchanged on
  local Macs and GitHub runners;
- exact pinned quality-engine and GitHub Action revisions;
- standard public GitHub-hosted runner with expected additional monetary cost `$0`;
- no tests, signing, private source, secrets, paid APIs, branch protection, or automatic execution.

The workflow is advisory and starts only through `workflow_dispatch`. `static` runs the policy-driven
quality scan. `build` runs the same static gate and then calls the pinned engine's public
`build-evidence` boundary. That boundary authenticates the clean source and engine revisions,
tracked profile bytes, engine CodeDirectory hash, selected toolchain, declared Xcode matrix entry,
compiled-source membership, and stable structured result before it may emit `PASS / READY`.
Absence of a workflow run is `NOT_RUN_BY_USER_DECISION`, never PASS or FAIL.

## Local Static Validation

```bash
plutil -lint QualityControlCanary.xcodeproj/project.pbxproj
xmllint --noout QualityControlCanary.xcodeproj/xcshareddata/xcschemes/QualityControlCanary.xcscheme
python3 -m json.tool .quality-control/profile.json >/dev/null
```

Runtime build and GitHub workflow execution remain user-controlled.
