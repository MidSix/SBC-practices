--> https://www.overleaf.com/project/6abb811d6e477f9c071619b8

### Working with LATEX in vscode
This will let us have the same functionalities that
Overleaf offer but without paying anything to allow
three collaborators, because rn Alvaro can't do a thing xd

### What it has?
- Everything that Overleaf offer EXCEPT for parallellism collaboration
but we aren't going to use it. Basically we are working sequentially
"I'll do this, then you do that, ..." no need to pay for a function
that we aren't using xd. Also we can use our friend hehe.

### How to set up? (Windows, step by step)

Do it once per person. Total: ~15 min, most of it is downloading.

**Step 1 - Extension**
In vscode go to Extensions, search "LaTeX Workshop" (James Yu), install.

**Step 2 - Install MiKTeX and Perl**
MiKTeX is the Latex distribution (the thing that actually builds the PDF),
just like installing python, a programming language, while the extension is
the piece of software that improves the experience of using it in vscode.
Perl is needed because latexmk (the build tool) is a Perl script.
Open PowerShell and run:

    winget install MiKTeX.MiKTeX
    winget install StrawberryPerl.StrawberryPerl

(If MiKTeX asks "Install missing packages on-the-fly" choose "Yes".
Otherwise a dialog pops up every time a package is missing.)

**Step 3 - Add MiKTeX to the PATH**
Without this the extension says "spawn latexmk ENOENT". In PowerShell:

    $bin = @("$env:LOCALAPPDATA\Programs\MiKTeX\miktex\bin\x64",
             "$env:ProgramFiles\MiKTeX\miktex\bin\x64") |
           Where-Object { Test-Path $_ } | Select-Object -First 1
    $userPath = [Environment]::GetEnvironmentVariable('Path','User')
    if ($userPath -notlike "*$bin*") {
        [Environment]::SetEnvironmentVariable('Path', "$userPath;$bin", 'User')
    }

**Step 4 - Restart vscode COMPLETELY**
Close ALL vscode windows and open it again ("Reload Window" is not enough,
a running program never sees a changed PATH).

**Step 5 - Check it**
In a NEW PowerShell:

    latexmk -v
    perl -v

Both must print a version. If one says "not recognized": redo step 3
(or 2), close the terminal / reboot, try again.

### How to set up? (macOS, step by step)

Replaces Windows steps 2-5. Steps 1, 6 and 7 are the same (on Mac
Ctrl -> Cmd for the vscode shortcuts: Cmd+Shift+P, Cmd+Alt+B, Cmd+Alt+V).
Step 6b is NOT needed, step 5b below already configures the build.
Perl already comes with macOS, nothing to install.

Why not MiKTeX? On macOS it's abandoned (last version 22.1 from 2022,
Intel only). We use TinyTeX (the smallest TeX Live, ~250 MB, no password
needed) + our script latexmk-auto (in this folder) that installs the
missing packages on the fly, like MiKTeX does on Windows.

**Step 2 - Install TinyTeX + latexmk**
In Terminal:

    curl -sL "https://yihui.org/tinytex/install-bin-unix.sh" | sh -s -- --no-path
    ~/Library/TinyTeX/bin/universal-darwin/tlmgr install latexmk

(ignore a "mv: rename ... --no-path" error at the end, it's a bug of
their script, TinyTeX is installed anyway)

**Step 3 - Add TinyTeX to the PATH + install latexmk-auto**
Without this the extension says "spawn latexmk ENOENT". In Terminal, from
this folder (modelo_contextual_help):

    echo 'export PATH="$HOME/Library/TinyTeX/bin/universal-darwin:$HOME/.local/bin:$PATH"' >> ~/.zprofile
    mkdir -p ~/.local/bin
    cp latexmk-auto ~/.local/bin/ && chmod +x ~/.local/bin/latexmk-auto

**Step 4 - Restart vscode COMPLETELY**
Cmd+Q (closing the windows is not enough on Mac, the app keeps running),
then open it again.

**Step 5 - Check it**
In a NEW Terminal window:

    latexmk -v
    perl -v
    which latexmk-auto

All three must print something. If "command not found": redo step 3.

**Step 5b - Make vscode build with latexmk-auto**
Cmd+Shift+P > Preferences: Open User Settings (JSON), inside the main { }
(comma after the previous entry) paste this. Replace YOUR_USER with the
output of `whoami` (vscode doesn't understand ~ here):

    "latex-workshop.latex.tools": [
        {
            "name": "latexmk-auto",
            "command": "/Users/YOUR_USER/.local/bin/latexmk-auto",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ]
        }
    ],
    "latex-workshop.latex.recipes": [
        { "name": "latexmk (auto-install packages)", "tools": ["latexmk-auto"] }
    ]

Now every build that hits "File `xxx.sty' not found" installs the package
and builds again by itself (you'll see "latexmk-auto: installing missing
package(s): ..." in the output). The first build of a document is slow
because of that. From a terminal: `latexmk-auto main.tex`.

**Step 6 - Hide the .synctex.gz files (once)**
The temp files already go to build/ (see below) but .synctex.gz must stay
next to the PDF (it's what makes ctrl+click PDF <-> code work). So we just
hide it in the explorer:
 1. In vscode press Ctrl+Shift+P
 2. Type and select: Preferences: Open User Settings (JSON)
 3. Add this line inside the main { } the root basically, (mind the comma on the previous line):

    "files.exclude": { "**/*.synctex.gz": true }

 4. Save. If you already have a "files.exclude", just add the
    "**/*.synctex.gz": true entry inside it.

**Step 6b - (Optional) Temp files in build/ for ANY project**
The repo already does it for these .tex. If you want it for every latex
project on your machine, in the SAME User Settings (JSON) file of step 6,
inside the main { } (comma after the previous entry), paste:

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
    ]

%DIR% = the folder of the .tex you build. PDF stays there, temp files go
to <that folder>/build/ (created automatically).

**Step 7 - Build**
Open the .tex, press Ctrl+Alt+B (or the green play button in the TeX
sidebar). Ctrl+Alt+V shows the PDF. The first build is slow, MiKTeX
downloads the missing packages.

### How the build/ folder works (already in the repo, nothing to do)
The PDF stays next to the .tex, the temp files (.aux, .log, .fls...) go to
SBC_practica/build/ (created automatically). It works no matter which folder
you open in vscode because it lives in the repo files:
- First line of every .tex:   % !TeX program = latexmk
  (LaTeX Workshop reads it and runs plain latexmk, so it doesn't force its
  own output folders). NEW .tex FILES NEED THIS LINE TOO!
- SBC_practica/.latexmkrc (latexmk reads it from the .tex folder):
      $pdf_mode = 1;
      $out_dir = ".";
      $aux_dir = "build";
- .gitignore ignores build/ and the temp extensions.

From a terminal it's the same:

    cd SBC_practica
    latexmk main.tex
    latexmk -c          (deletes temp files, keeps the PDF)

### Troubleshooting
- "spawn latexmk ENOENT": MiKTeX/TinyTeX not installed or not in the PATH
  (steps 2-3) or vscode not fully restarted (step 4, on Mac: Cmd+Q).
- macOS, still ENOENT after Cmd+Q: launch vscode from a Terminal with
  `code .` so it inherits that terminal's PATH.
- macOS, "latexmk-auto: no TeX Live package provides xxx.sty": the package
  name is misspelled in the .tex (or it really doesn't exist in TeX Live).
- macOS, symbols look pixelated when zooming the PDF: `tlmgr install cm-super`
  (vector versions of some fonts, ~60 MB, optional).
- latexmk fails right away / "could not find perl": install Perl (step 2).
- Dialog asking to install a package: say yes (or MiKTeX Console >
  Settings > install missing packages on-the-fly = Yes).
- Temp files appear next to the .tex again: that .tex is missing the
  "% !TeX program = latexmk" first line.
