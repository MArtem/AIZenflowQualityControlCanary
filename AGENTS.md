# Repository Guidelines

## Project Structure & Module Organization

This repository is a public synthetic consumer of `MArtem/AIZenflowQualityControl`. The
`QualityControlCanary.xcodeproj` target compiles the intentionally minimal source under `Sources/`.
The manual workflow under `.github/workflows/` checks this repository with an exact pinned engine
revision before optionally building the Xcode target. Keep product code, private fixtures,
credentials, signing assets, and app-specific policy out of this repository.

## Build, Test, and Development Commands

- `plutil -lint QualityControlCanary.xcodeproj/project.pbxproj` validates the project document.
- `xmllint --noout QualityControlCanary.xcodeproj/xcshareddata/xcschemes/QualityControlCanary.xcscheme`
  validates the shared scheme XML.
- `xcodebuild -project QualityControlCanary.xcodeproj -scheme QualityControlCanary -configuration Debug -derivedDataPath <repo-contained-path> CODE_SIGNING_ALLOWED=NO build`
  builds the synthetic macOS command-line target without signing.

The repository has no test target. Do not add, modify, or run tests unless the user explicitly
opens that phase. Keep local build products and caches inside `/Users/Artem/.zenflow`.

## Coding Style & Naming Conventions

Use Swift 6 and warnings-as-errors for the canary source. Keep the source deterministic and free of
network, filesystem, environment, user-data, and credential dependencies. Do not add third-party
packages or floating workflow dependencies.

## Commit & Pull Request Guidelines

After the initial empty-repository bootstrap, make changes through bounded branches and pull
requests. Pin GitHub Actions by full commit SHA and the quality engine by exact commit SHA. Manual
GitHub checks remain advisory and user-triggered; branch protection, paid runners, paid APIs,
automatic workflows, signing, Simulator/device work, and application integration are out of scope.
