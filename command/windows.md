## Create junction link

### cmd

```bash
mklink /J junction-link-name target-directory
```

### PowerShell

```powershell
New-Item -ItemType Junction -Path "C:\Path\To\Your\New\Link" -Target "C:\Path\To\The\Original\Target"
```