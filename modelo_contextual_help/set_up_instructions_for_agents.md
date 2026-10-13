# LaTeX setup for contributors - instructions for AI agents

Goal: make ONE contributor's Windows machine able to build the .tex files of
this repo from vscode (LaTeX Workshop + MiKTeX), with temp files in `build/`.

## Rules (read first)
- Everything here is PER-USER machine configuration. Do NOT commit anything
  and do NOT modify any file inside the repo. The repo already contains what
  it needs (`% !TeX program = latexmk` first line of each .tex,
  `SBC_practica/.latexmkrc`, `.gitignore`).
- Only touch: installed programs, the user's PATH, the user's vscode
  User settings (`%APPDATA%\Code\User\settings.json`).
- Be idempotent: check before every step, skip what is already done.
- Never overwrite the user's settings.json. Back it up, then only ADD missing
  keys. Never remove or change existing keys.
- Ask the user before installing software or editing their settings.
- Shell: Windows PowerShell 5.1 (no `&&`, no `??`). Do not use
  `ConvertFrom-Json` on settings.json: it is JSONC (comments, trailing
  commas) and PS 5.1 chokes on it.
- Tell the user at the end that vscode must be fully closed and reopened.

## Step 1 - Check what exists
```powershell
Get-Command latexmk, perl -ErrorAction SilentlyContinue | Select-Object Name, Source
Test-Path "$env:LOCALAPPDATA\Programs\MiKTeX\miktex\bin\x64\latexmk.exe"
Test-Path "$env:ProgramFiles\MiKTeX\miktex\bin\x64\latexmk.exe"
code --list-extensions | Select-String latex-workshop
```
Decide which of steps 2-5 are needed from the results.

## Step 2 - Install what is missing
```powershell
winget install MiKTeX.MiKTeX                     # if no MiKTeX folder found
winget install StrawberryPerl.StrawberryPerl     # if `perl` not found
code --install-extension James-Yu.latex-workshop # if extension missing
```
Tell the user: in MiKTeX setup choose "Install missing packages on-the-fly:
Yes".

## Step 3 - Add MiKTeX to the user PATH (only if `latexmk` is not found)
Works for per-user and all-users installs, does nothing if already there.
```powershell
$bin = @("$env:LOCALAPPDATA\Programs\MiKTeX\miktex\bin\x64",
         "$env:ProgramFiles\MiKTeX\miktex\bin\x64") |
       Where-Object { Test-Path $_ } | Select-Object -First 1
if (-not $bin) { throw "MiKTeX not installed, redo step 2" }
$userPath = [Environment]::GetEnvironmentVariable('Path','User')
if ($userPath -notlike "*$bin*") {
    [Environment]::SetEnvironmentVariable('Path', "$userPath;$bin", 'User')
}
```
The current shell does not see the change. To verify in the same shell:
`$env:Path += ";$bin"; latexmk -v; perl -v` (both must print a version).
The permanent PATH only reaches vscode after a full restart (step 6).

## Step 4 - Per-user vscode settings (merge, never overwrite)
Target: `$env:APPDATA\Code\User\settings.json`. (If the user uses Insiders or
Cursor etc., the folder is `Code - Insiders` / `Cursor`; ask.)

Two blocks are added, each only if its marker key is missing:
- hide `.synctex.gz` (marker `files.exclude`'s entry `**/*.synctex.gz`).
  If the user already has a `files.exclude` key, do NOT add a second one
  (duplicate keys: the last wins and the old entries are lost). Instead
  insert `"**/*.synctex.gz": true` inside the existing object by hand.
- temp files in `<tex folder>/build` for any project (marker
  `latex-workshop.latex.tools`). If the user already defines their own
  `latex-workshop.latex.tools` or `recipes`, STOP and ask the user; do not
  merge automatically.

Inserting right after the first `{` with trailing commas is always valid
JSONC, so no comma bookkeeping is needed:
```powershell
$p = "$env:APPDATA\Code\User\settings.json"
if (-not (Test-Path $p)) { Set-Content $p "{`n}" -Encoding utf8 }
Copy-Item $p "$p.bak-latex" -Force
$s = Get-Content $p -Raw

$add = ""
if ($s -notmatch '\*\*/\*\.synctex\.gz' -and $s -notmatch '"files\.exclude"') {
$add += @'

    "files.exclude": { "**/*.synctex.gz": true },
'@
}
if ($s -notmatch 'latex-workshop\.latex\.(tools|recipes|outDir)') {
$add += @'

    "latex-workshop.latex.outDir": "%DIR%",
    "latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%DIR%",
                "-auxdir=%DIR%/build",
                "%DOC%"
            ]
        }
    ],
    "latex-workshop.latex.recipes": [
        { "name": "latexmk", "tools": ["latexmk"] }
    ],
'@
}
if ($add) {
    $i = $s.IndexOf('{')
    $s = $s.Substring(0, $i + 1) + $add + $s.Substring($i + 1)
    Set-Content $p $s -Encoding utf8 -NoNewline
}
```
If any `files.exclude` / `latex-workshop.latex.*` key already exists the
script skips it, then tell the user what was skipped and why.

After editing, verify the file still parses as JSONC. With Node:
```powershell
node -e "const t=require('fs').readFileSync(process.argv[1],'utf8');new Function('return ('+t+'\n)')()" $p
```
(JS object literal rules accept comments and trailing commas.)
Caveat: the insert goes after the FIRST { of the file; if a comment before the real opening brace contains a {, insert by hand. If it throws,
restore the backup: `Copy-Item "$p.bak-latex" $p -Force`, and report.

## Step 5 - Check the repo side exists (read only, do not edit)
In the repo: first line of each `*.tex` in `SBC_practica/` is
`% !TeX program = latexmk`, and `SBC_practica/.latexmkrc` contains
`$pdf_mode = 1;`, `$out_dir = ".";`, `$aux_dir = "build";`.
If something is missing, do NOT fix it silently: report to the user, the
repo files are shared and changes go through a normal commit.

## Step 6 - Tell the user to restart vscode
Close ALL vscode windows and reopen. "Reload Window" does not refresh PATH.

## Step 7 - Verify with a real build
```powershell
cd <repo>\SBC_practica
latexmk main.tex
Get-ChildItem            # main.pdf next to main.tex, a build\ folder
Get-ChildItem build      # main.aux, main.log, main.fls, ...
```
Expected: PDF beside the .tex, temp files only in `build/`
(`main.synctex.gz` beside the PDF is normal; it is hidden in the explorer by
step 4 and ignored by .gitignore).
Clean afterwards if desired: `latexmk -c` (keeps the PDF).
Then ask the user to press Ctrl+Alt+B on a .tex in vscode and confirm the
same result.

## Troubleshooting
| Symptom | Cause / fix |
|---|---|
| `spawn latexmk ENOENT` in LaTeX Workshop log | PATH step 3 missing, or vscode not fully restarted |
| latexmk exits at once, mentions perl | Perl not installed (step 2) |
| Package dialog on every build | MiKTeX on-the-fly install not set to Yes (MiKTeX Console > Settings) |
| Temp files next to the .tex again | that .tex lacks `% !TeX program = latexmk` AND user settings step 4 not applied |
| settings.json shows red errors after edit | restore `settings.json.bak-latex` and retry manually |
