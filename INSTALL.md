# Install in Codex

## Quick install

```text
$skill-installer install https://github.com/Qrzzzz/slide-script-tex-generator
```

## Manual install (macOS / Linux / WSL)

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
rm -rf "${CODEX_HOME:-$HOME/.codex}/skills/slide-script-tex-generator"
git clone https://github.com/Qrzzzz/slide-script-tex-generator.git \
  "${CODEX_HOME:-$HOME/.codex}/skills/slide-script-tex-generator"
```

## Manual install (Windows PowerShell)

```powershell
$CodexHome = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE ".codex" }
$SkillsDir = Join-Path $CodexHome "skills"
$SkillDir = Join-Path $SkillsDir "slide-script-tex-generator"
New-Item -ItemType Directory -Force -Path $SkillsDir | Out-Null
if (Test-Path $SkillDir) { Remove-Item -Recurse -Force $SkillDir }
git clone https://github.com/Qrzzzz/slide-script-tex-generator.git $SkillDir
```

## Verify

Check `SKILL.md` exists in:

```text
~/.codex/skills/slide-script-tex-generator/SKILL.md
```
