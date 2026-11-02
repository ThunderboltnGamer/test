# Fortnite 4.5 build archive

The complete `D:\Og Fortnite\Fortnite+Release-4.5` folder is stored as a split
release archive in this private repository.

## Why it is split

The zip is **24,827,143,557 bytes** (~23.12 GiB). GitHub limits a single release
asset to 2 GiB and a normal repository file to 100 MB, so it is split into
13 assets.

Whole zip SHA-256:

```text
1574279d742d97244dfb58fc2096f8b1a8ee074d2c7f541b7679c530153f38fc
```

Release:

https://github.com/ThunderboltnGamer/test/releases/tag/fortnite-45-build

## Download

```powershell
gh release download fortnite-45-build `
  -R ThunderboltnGamer/test `
  --dir D:\Fortnite45.parts
```

The parts are:

```text
Fortnite+Release-4.5.zip.part01 ... part12  (2,000,000,000 bytes each)
Fortnite+Release-4.5.zip.part13               (827,143,557 bytes)
MANIFEST-45.sha256
```

## Combine

PowerShell:

```powershell
$dir = "D:\Fortnite45.parts"
$out = Join-Path $dir "Fortnite+Release-4.5.zip"
$fs = [IO.File]::Create($out)

Get-ChildItem $dir -Filter "Fortnite+Release-4.5.zip.part*" |
  Sort-Object Name |
  ForEach-Object {
    $in = [IO.File]::OpenRead($_.FullName)
    $in.CopyTo($fs)
    $in.Dispose()
  }

$fs.Dispose()
Get-FileHash $out -Algorithm SHA256
```

Expected:

```text
1574279d742d97244dfb58fc2096f8b1a8ee074d2c7f541b7679c530153f38fc
```

Linux:

```bash
gh release download fortnite-45-build -R ThunderboltnGamer/test --dir /tmp/f45
cat /tmp/f45/Fortnite+Release-4.5.zip.part* > /tmp/Fortnite+Release-4.5.zip
sha256sum /tmp/Fortnite+Release-4.5.zip
```

Then extract the zip so the resulting folder is:

```text
D:\Og Fortnite\Fortnite+Release-4.5
```

Point `%LOCALAPPDATA%\ProjectNostalgia\config\builds.json` `LastBuild` at that
folder.
