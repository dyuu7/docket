# docket

[![CI](https://github.com/dyuu7/docket/actions/workflows/ci.yml/badge.svg)](https://github.com/dyuu7/docket/actions/workflows/ci.yml)
[![Excavator](https://github.com/dyuu7/docket/actions/workflows/excavator.yml/badge.svg)](https://github.com/dyuu7/docket/actions/workflows/excavator.yml)
[![Scoop bucket](https://img.shields.io/badge/Scoop-bucket-5a4?logo=windows-terminal&logoColor=white)](https://scoop.sh/)

A personal [Scoop](https://scoop.sh) bucket for Windows software I use, maintain, or want to make easy to install. It contains manifests for game launchers, media and subtitle tools, automation utilities, networking tools, and Chinese-language desktop applications.

This is an independent bucket and is not affiliated with ScoopInstaller.

The repository is intentionally small and focused: manifests point to upstream releases, pin SHA-256 hashes, preserve user data where needed, and use Scoop's update automation when upstream versioning is reliable.

> 中文简介：这是一个个人 Scoop bucket，收录我实际使用或维护的 Windows 软件。每个 manifest 尽量使用上游发布地址、固定 SHA-256 校验值，并在需要时保留用户数据。

## Install

Install [Scoop](https://scoop.sh/) first, then add this bucket:

```powershell
scoop bucket add docket https://github.com/dyuu7/docket
scoop install docket/Starward docket/ddns-go
```

The bucket name is `docket`; manifest names match the JSON files in [`bucket/`](bucket/), without the `.json` suffix. For example:

```powershell
scoop search Fanhuaji
scoop install docket/Fanhuaji
scoop update Fanhuaji
```

## Available manifests

The manifest files are the source of truth for versions, download URLs, hashes, and installation behavior. This table describes the current scope without duplicating version numbers that are updated over time.

| Manifest | Purpose | Platform coverage | Data and behavior |
| --- | --- | --- | --- |
| [`EnableLoopbackUtility`](bucket/EnableLoopbackUtility.json) | Creates loopback exemptions for Windows apps | x64 | Adds a Start Menu shortcut |
| [`Fanhuaji`](bucket/Fanhuaji.json) | Converts between Traditional and Simplified Chinese | x86, x64 | Preserves the user profile |
| [`FontLoaderSub`](bucket/FontLoaderSub.json) | Loads OpenType fonts for ASS/SSA subtitles | Windows | Persists fonts and the font database |
| [`KeymouseGo`](bucket/KeymouseGo.json) | Records and automates mouse and keyboard input | x64 | Persists automation scripts |
| [`SimpleTex`](bucket/SimpleTex.json) | Recognizes mathematical formulas from images | Windows | Persists application data |
| [`Starward`](bucket/Starward.json) | Game launcher for miHoYo/HoYoverse games | x64, ARM64 | Persists data and configuration |
| [`SubRenamer`](bucket/SubRenamer.json) | Batch-renames subtitle files to match videos | Windows | Portable installation |
| [`UU`](bucket/UU.json) | NetEase online game accelerator | Windows | Adds a Start Menu shortcut |
| [`ddns-go`](bucket/ddns-go.json) | Updates DNS records to the current public IP | x86, x64, ARM64 | Exposes `ddns-go` on PATH |
| [`foobox-cn`](bucket/foobox-cn.json) | Chinese foobar2000 DUI skin and configuration | Windows | Persists the foobar2000 profile |

> Maintenance note: `foobox-cn` is a legacy entry. Newer [upstream releases](https://github.com/dream7180/foobox-cn/releases) are add-on payloads for an existing foobar2000 installation, so this manifest needs a separate dependency/integration design before it can be updated safely.

Some manifests are intentionally maintained manually because the upstream project does not provide a stable version endpoint or release layout for safe automatic updates.

## What this project demonstrates

This bucket is also a small, practical packaging project. Its manifests cover:

- upstream release tracking and reproducible SHA-256 verification;
- architecture-specific downloads for x86, x64, and ARM64;
- portable installation, shortcuts, executable shims, and persistent user data;
- PowerShell install hooks for applications with non-standard archive layouts;
- automated validation and update workflows on Windows.

## Maintenance

- [`ci.yml`](.github/workflows/ci.yml) runs the Scoop bucket test suite under both Windows PowerShell and PowerShell 7.
- [`excavator.yml`](.github/workflows/excavator.yml) checks for upstream releases on a schedule and prepares manifest updates where `checkver` and `autoupdate` support it.
- The scripts in [`bin/`](bin/) wrap Scoop's checks for versions, URLs, hashes, JSON formatting, and bucket tests.
- [Dependabot](.github/dependabot.yml) keeps GitHub Actions versions on a regular update path.
- Low-privilege CI follows release tags; workflows that can write to the repository pin action commits.
- Issue forms cover package requests, manifest bugs, and hash errors.

## Local checks

Run these commands from the repository root on Windows with Scoop installed. The test wrapper requires `BuildHelpers` 2.0.1+ and Pester 5.2.0+:

```powershell
Install-Module BuildHelpers -MinimumVersion 2.0.1 -Scope CurrentUser
Install-Module Pester -MinimumVersion 5.2.0 -Scope CurrentUser

.\bin\test.ps1
.\bin\checkurls.ps1
.\bin\checkhashes.ps1
.\bin\checkver.ps1 Starward
```

To apply an automatic update for a manifest:

```powershell
.\bin\checkver.ps1 Starward -Update
```

## Contributing

Bug reports, package requests, and focused pull requests are welcome. Please read the repository's [contributing guide](CONTRIBUTING.md), use the relevant [issue form](.github/ISSUE_TEMPLATE/) before proposing a new manifest, and consult Scoop's [contributing guide](https://github.com/ScoopInstaller/.github/blob/main/.github/CONTRIBUTING.md) and [app manifest documentation](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests).

New or updated manifests should use official upstream download sources, include a verified SHA-256 hash, describe persistence and install hooks clearly, and include `checkver`/`autoupdate` when the upstream release format is dependable.

## Repository layout

```text
bucket/                 Scoop manifests
bin/                    Local wrappers around Scoop maintenance scripts
.github/workflows/      CI, Excavator, and issue/PR automation
Scoop-Bucket.Tests.ps1  Pester entry point for bucket validation
```

## License

This repository is released under the [Unlicense](LICENSE). The software packaged by each manifest remains subject to its own upstream license; see the individual manifest for details.
