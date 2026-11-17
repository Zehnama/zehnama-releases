# Zehnama releases

Signed Windows installers for **Zehnama**, an electronic patient record for
private psychiatric practices, from Zehnama Ltd ([zehnama.com](https://zehnama.com)).
This repository holds releases only: the installers, their checksums and
release notes. There is no source code here.

Installed copies of Zehnama check this repository for updates. They read it
anonymously and verify every download themselves before installing it (see
below), so nothing here needs to be trusted blindly.

## Channels

| Channel | What it is | On GitHub |
|---|---|---|
| **Stable** | Releases approved for clinical use | Full releases, `vX.Y.Z` |
| **Beta** | Pre-releases for testing ahead of the next stable release | Pre-releases, `vX.Y.Z-beta.N` |

A copy on the stable channel is offered only stable releases. A copy on the
beta channel is offered the highest version of either kind. Nobody is ever
offered a version lower than the one they are running.

### Feature and critical releases

Each release is a **feature** release or a **critical** one: a critical
release fixes a patient-safety risk, a security vulnerability, a risk to
clinical data, a problem with lawful use after a change in the law, or
updates the medication dictionary (the Zehnama licence agreement, section 1,
"Critical Update"). Under a Lifetime Subscription, installed copies are
offered feature releases published during their three years of feature
updates, and every critical release for the supported life of the product.
Past the three years, a newer feature release is shown as available with a
renewal; nothing stops working.

## Verifying a download

Each release carries exactly three files: `zehnama_setup_<version>.exe`,
`SHA256SUMS.txt` and `RELEASE-CLASS.txt` (one line, `class: feature` or
`class: critical`). Before you install, in PowerShell, in the folder you
downloaded to:

```powershell
# 1. The checksum: must equal the line in SHA256SUMS.txt
Get-FileHash .\zehnama_setup_<version>.exe -Algorithm SHA256
Get-Content .\SHA256SUMS.txt

# 2. The signature: Status must be Valid, the signer's subject must be
#    Zehnama's publisher (below), and it must carry a timestamp
Get-AuthenticodeSignature .\zehnama_setup_<version>.exe |
  Format-List Status, SignerCertificate, TimeStamperCertificate

# 3. The version inside the installer: must equal the release's version
(Get-Item .\zehnama_setup_<version>.exe).VersionInfo.ProductVersion

# 4. The release class inside the installer: must match RELEASE-CLASS.txt
(Get-Item .\zehnama_setup_<version>.exe).VersionInfo.FileDescription
```

**Expected publisher:** `CN=Fareedoon Ahmed, O=Fareedoon Ahmed, L=Ilford, C=GB`,
issued by `CN=Certum Code Signing 2021 CA, O=Asseco Data Systems S.A., C=PL`.
Windows names the verified publisher as **Fareedoon Ahmed**.

When you run the installer, Windows asks for permission and names the
verified publisher. If it names nobody, or anyone else, do not install it.

The checksum proves the file is the one published here. The signature proves
it was built and signed by Zehnama's publisher: anyone able to change a file
here could change the checksum too, but not the signature. The version
inside the installer is part of the signed file, so an old installer cannot
be passed off as a new release. So is the release class: `RELEASE-CLASS.txt`
is not signed, so an installed copy that takes a release only because it is
marked critical checks the class inside the signed installer too, and
refuses a mismatch.

## Each release is checked twice

The release script downloads every release back from GitHub while it is
still a draft and verifies it before publishing it. Then the
[validate-release](.github/workflows/validate-release.yml) workflow checks the
published release again on a clean Windows machine: the assets, the checksum,
the signature and its publisher, the installer's version, the release class
and the pre-release flag. A release that fails is turned back into a draft at once, which hides
it from every installed copy.

## Rules for this repository

- **Never delete or move a tag.** A tag names one installer forever. A bad
  release is withdrawn - `gh release edit vX --draft=true`, which hides it
  from every installed copy at once - and superseded by the next version
  number. A version number is never reused.
- Releases are published only by the maintainer, through the release script.
- Never rename this repository or its organisation: installed copies look
  for it by name.
- If a release changed the practice's data (its notes have a **Database**
  section), going back means installing the older installer and restoring the
  backup taken **before** the update. Never install an older version over data
  a newer one has opened.

## Licences

Zehnama is licensed under the Zehnama End User Licence Agreement
([zehnama.com/legal](https://zehnama.com/legal)). Third-party components
bundled in the installer are listed in [NOTICE](NOTICE) and
[THIRD_PARTY_LICENSES.txt](THIRD_PARTY_LICENSES.txt).

## Contact

- Help with installing or using Zehnama: support@zehnama.com
- A security vulnerability: security@zehnama.com — please do not open a
  public issue
