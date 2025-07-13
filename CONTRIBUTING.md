# Contributing

Thanks for helping improve `docket`. This is a personal [Scoop](https://scoop.sh) bucket for Windows software that is useful to me and may be useful to others.

## Scope

New packages should fit the bucket's focus and be installable from an official upstream Windows release. A request should include:

- the upstream project and homepage;
- a direct download URL for the Windows release;
- a stable version and a verifiable SHA-256 hash;
- enough information to explain why the package belongs in this bucket.

Open a [package request](.github/ISSUE_TEMPLATE/package-request.yml) before proposing a new package. Routine version updates and fixes for existing manifests can go directly through a pull request. For broken downloads or hashes, use the relevant issue form.

## Manifest checklist

Before opening a pull request:

1. Name the file after the Scoop install name, for example `bucket/example.json`.
2. Prefer official upstream release URLs. Do not re-host or repackage binaries.
3. Verify every committed hash against the downloaded file.
4. Add architecture-specific URLs only when the upstream artifacts are genuinely different.
5. Add `checkver` and `autoupdate` when the upstream release format is stable enough to update safely.
6. Describe shortcuts, shims, install hooks, and persisted data accurately.
7. Check the upstream license and preserve any required attribution or license URL.

## Validation

Run the following from the repository root on Windows with Scoop installed:

```powershell
.\bin\test.ps1
.\bin\checkurls.ps1
.\bin\checkhashes.ps1
.\bin\checkver.ps1 Starward
```

The test wrapper requires `BuildHelpers` 2.0.1+ and Pester 5.2.0+. Network checks can fail transiently; include the command and result in the pull request when a check cannot be completed.

For more detail about manifest fields and contribution policy, read Scoop's [app manifest documentation](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests) and [contributing guide](https://github.com/ScoopInstaller/.github/blob/main/.github/CONTRIBUTING.md).
