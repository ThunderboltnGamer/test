# Project Nostalgia — `New.zip` archive

This private repository holds the split archive of the full Project Nostalgia
project for transfer to the VM.

## Why it is split

The original `New.zip` is **5.95 GB** (6,242,299,890 bytes). GitHub does not allow
a single repository file or release asset of that size, so the archive is split
into four release assets:

| Asset | Bytes |
|---|---:|
| `New.zip.part01` | 2,000,000,000 |
| `New.zip.part02` | 2,000,000,000 |
| `New.zip.part03` | 2,000,000,000 |
| `New.zip.part04` | 242,299,890 |

**Whole-file SHA-256:**

```text
c0ec6c3e43240d22e28a09b8ae59118b7c16ccca593faa60a813e57f4eaf005a
```

## Download

Release:

https://github.com/ThunderboltnGamer/test/releases/tag/project-archive

The repository is private. Use an authenticated GitHub session or GitHub CLI:

```powershell
gh release download project-archive `
  -R ThunderboltnGamer/test `
  --dir C:\Users\thund\Downloads\New.zip.parts
```

## Combine

```powershell
$dir = "C:\Users\thund\Downloads\New.zip.parts"
$out = Join-Path $dir "New.zip"
$fs = [IO.File]::Create($out)
Get-ChildItem $dir -Filter "New.zip.part*" |
  Sort-Object Name |
  ForEach-Object {
    $in = [IO.File]::OpenRead($_.FullName)
    $in.CopyTo($fs)
    $in.Dispose()
  }
$fs.Dispose()

Get-FileHash $out -Algorithm SHA256
```

Expected SHA-256:

```text
c0ec6c3e43240d22e28a09b8ae59118b7c16ccca593faa60a813e57f4eaf005a
```

Then:

```powershell
Expand-Archive $out -DestinationPath "C:\Users\thund\Downloads\NEW"
```

## Notes

- This repository is private. Do not make it public; the archive contains project
  configuration and private credentials.
- The release assets are split only because of GitHub's size limits.
- Individual part hashes are in `MANIFEST.sha256` in the release assets.

## Fortnite 4.5 build

The complete 4.5 build is split under the ortnite-45-build release. See [FORTNITE_45_BUILD.md](FORTNITE_45_BUILD.md).

