# Install in Codex

## Quick install

```text
$skill-installer install https://github.com/Qrzzzz/slide-script-tex-generator
```

## GitHub tree URL fallback

If your Codex installation expects a tree URL:

```text
$skill-installer install https://github.com/Qrzzzz/slide-script-tex-generator/tree/main
```

## Cursor natural-language installation

You can paste this into Cursor:

```text
Please install this GitHub repository as a Codex Skill:
https://github.com/Qrzzzz/slide-script-tex-generator

Requirements:
1. Clone the full repository into my Codex skills directory.
2. If CODEX_HOME exists, install to $CODEX_HOME/skills/slide-script-tex-generator.
3. Otherwise install to ~/.codex/skills/slide-script-tex-generator.
4. Verify SKILL.md exists in the target folder.
5. Keep SKILL.md, assets, references, examples, and the full folder structure.
6. Do not copy README only.
7. Remind me to restart Codex or open a new Codex session.
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

## Verify installation

Check this file exists:

```text
~/.codex/skills/slide-script-tex-generator/SKILL.md
```

You can also confirm the following directories exist under the skill folder:
- `assets/`
- `references/`
- `examples/`

## Final step

Restart Codex or open a new Codex session so the skill can be detected.
