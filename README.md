# Zehnama releases

Signed Windows installers for **Zehnama**, an electronic patient record for
private psychiatric practices. This repository holds releases only — there
is no source code here.

## Channels

| Channel | What it is | How it appears here |
|---|---|---|
| **Stable** | Releases approved for clinical use | A full release, e.g. `v1.0.0` |
| **Beta** | Pre-release builds for testers | A GitHub *pre-release*, e.g. `v0.9.0-beta.3` |

Versions follow [SemVer 2.0.0](https://semver.org/). A tag is never reused or
moved; a withdrawn release is superseded by the next number.

## Verifying a download

Every release carries the installer (`zehnama_setup_<version>.exe`) and
`SHA256SUMS.txt`. Before running an installer, in PowerShell:

```powershell
# 1. The file is the one that was published
(Get-FileHash .\zehnama_setup_<version>.exe -Algorithm SHA256).Hash
#    must equal the line for that file in SHA256SUMS.txt (case-insensitive)

# 2. It was signed by the publisher, and the signature is intact
Get-AuthenticodeSignature .\zehnama_setup_<version>.exe | Format-List Status, SignerCertificate
#    Status must be "Valid"; the signer is the publisher named in the release notes

# 3. It is the version it claims to be
(Get-Item .\zehnama_setup_<version>.exe).VersionInfo.ProductVersion
```

Do not run an installer that fails any of these checks.

## Going back to an earlier version

A release that changes the database format says so in its notes. Such a
change is one-way: an earlier version will refuse to open the newer data.
The way back is the earlier installer **plus a backup taken before the
upgrade**.

## Licences

See [NOTICE](NOTICE) and [THIRD_PARTY_LICENSES.txt](THIRD_PARTY_LICENSES.txt).
