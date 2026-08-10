# AIZenflow Quality Control Canary

Public synthetic consumer for manually validating
[`MArtem/AIZenflowQualityControl`](https://github.com/MArtem/AIZenflowQualityControl) against a
small buildable Xcode project.

## Current Scope

- one deterministic macOS command-line Xcode target;
- manual `static` and `build` workflow modes;
- exact pinned quality-engine and GitHub Action revisions;
- standard public GitHub-hosted runner with expected additional monetary cost `$0`;
- no tests, signing, private source, secrets, paid APIs, branch protection, or automatic execution.

The workflow is advisory and starts only through `workflow_dispatch`. `static` builds the pinned
engine once, reads the resulting macOS `CDHash`, and runs its `static-evidence` boundary against the
exact checked-out Git trees. `build` runs the same static-evidence gate and then builds the synthetic
target with signing disabled. Absence of a workflow run is `NOT_RUN_BY_USER_DECISION`, never PASS or FAIL.

## Local Static Validation

```bash
plutil -lint QualityControlCanary.xcodeproj/project.pbxproj
xmllint --noout QualityControlCanary.xcodeproj/xcshareddata/xcschemes/QualityControlCanary.xcscheme
```

Runtime build and GitHub workflow execution remain user-controlled.
