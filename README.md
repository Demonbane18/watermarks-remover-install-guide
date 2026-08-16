# Easy install guide: watermarks-remover (Windows and Mac)

This repo is **only a how-to**. It is not the watermark tool.

The real tool is here:

**[github.com/guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover)**  
by [Guillaume Meyer](https://github.com/guillaumemeyer) · [v0.5.0](https://github.com/guillaumemeyer/watermarks-remover/releases/tag/v0.5.0)

This guide tells you how to put that tool on:

- **Grok Build** (`grok`)
- **Claude Code**
- **Gemini / Antigravity** (`agy`)
- **OpenAI Codex**

It does **not** copy their code. Install the tool from their repo.

**It works on Windows and Mac.** Linux can follow the Mac steps.

Pick **one** track and stay on it. Do not mix Windows and Mac commands.

---

## Read this first (plain-language law note)

**This is not legal advice.** Laws change. Your country may differ. If you need a real answer for your case, ask a lawyer.

### What people often get wrong

The EU **does** ask AI companies to mark AI output.  
That duty is **not** “if you clean your own file, you go to jail.”

### What the EU AI Act actually says

| Who | Duty | Source |
| --- | --- | --- |
| **Providers** (the AI companies) | Mark synthetic text / image / audio / video so a machine can tell it is AI-made | [Article 50(2)](https://artificialintelligenceact.eu/article/50/) of [Regulation (EU) 2024/1689](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) |
| **Deployers** (who *use* the AI in public) | Say so if they publish a deepfake, or AI text meant to inform the public | [Article 50(4)](https://artificialintelligenceact.eu/article/50/) |
| **A person cleaning their own file** | Article 50 does **not** name you as the one who must keep the mark | same article |

The AI Act’s own punishments are **company fines**, not prison for a consumer:

- [Article 99](https://artificialintelligenceact.eu/article/99/) — administrative fines on providers, deployers, and other operators
- Official text: [EUR-Lex, Regulation (EU) 2024/1689](https://eur-lex.europa.eu/eli/reg/2024/1689/oj)

So: **yes, those marking rules are aimed at companies (and some professional users), not at jailing an end user who removes marks from content they own, or who rephrases their own text.**

That is the fair reading of Article 50 + Article 99. It is **not** a promise that “nothing bad can ever happen.”

### What can still get you in trouble

The AI Act is not the only law.

Do **not** use this to:

- Pretend AI work is 100% human when a school, job, court, or publisher requires disclosure
- Strip marks from **someone else’s** file you do not own (copyright / rights-management rules can apply, including [Directive 2001/29/EC Article 7](https://eur-lex.europa.eu/eli/dir/2001/29/oj) on rights-management information)
- Hide a deepfake or public-interest AI text that **you** publish (that can be a **deployer** duty under Article 50(4))
- Break a website or app’s own rules (a company may ban stripping metadata in its terms)

The official tool says the same thing in its ethics note: use it on **content you own**, and do not claim “this proves a human wrote it.”

See: [ethics.md in watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover/blob/main/skills/remove-ai-marks/references/ethics.md)

### Short version

- Cleaning **your own** files for privacy or hygiene is the intended use of the official tool.
- The EU AI Act does **not** create a jail sentence for that.
- Lying, fraud, or cleaning files you do not own is a different story.

---

## The one rule (both computers)

Use the **coding app**, not the **website chat**.

| App you might open | Use it? |
| --- | --- |
| grok.com, Grok on your phone | No |
| **Grok Build** (type `grok`) | **Yes** |
| claude.ai website | No |
| Claude Cowork | No |
| **Claude Code** | **Yes** |
| gemini.google.com | No |
| **Antigravity** or type `agy` | **Yes** |
| chatgpt.com | No |
| **Codex** (app or type `codex`) | **Yes** |

Website chat can rewrite words if you paste them. It **cannot** clean files.

---

## Pick your computer

| I have… | Open this | Then follow |
| --- | --- | --- |
| **Windows** | Start → type **PowerShell** → Enter | [Windows](#windows) |
| **Mac** | Command + Space → type **Terminal** → Enter | [Mac](#mac) |

---

# Windows

## W1. What you need

Install these if they are missing (one-time):

1. [Git for Windows](https://git-scm.com/download/win)
2. [Python 3](https://www.python.org/downloads/) — tick **Add python.exe to PATH**
3. [Node.js](https://nodejs.org/) — this gives you `npx`

Then in PowerShell:

```powershell
git --version
py --version
npx --version
```

All three should print a version number.

## W2. Get the official tool

```powershell
cd $HOME
git clone --depth 1 --branch v0.5.0 https://github.com/guillaumemeyer/watermarks-remover.git
cd watermarks-remover
```

Already downloaded?

```powershell
cd $HOME\watermarks-remover
```

Source: [guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover)

## W3. Teach the coding AIs

```powershell
npx skills add . --skill remove-ai-marks -g -y -a grok -a claude-code -a gemini-cli -a antigravity -a antigravity-cli -a codex
```

Wait until it says **Done**.

## W4. Extra links (so every app finds the skill)

```powershell
$src = Join-Path $HOME ".agents\skills\remove-ai-marks"
$dirs = @(
  "$HOME\.grok\skills",
  "$HOME\.claude\skills",
  "$HOME\.codex\skills",
  "$HOME\.gemini\skills",
  "$HOME\.gemini\antigravity\skills",
  "$HOME\.gemini\antigravity-cli\skills"
)
foreach ($d in $dirs) {
  New-Item -ItemType Directory -Force -Path $d | Out-Null
  $dest = Join-Path $d "remove-ai-marks"
  if (Test-Path $dest) { Remove-Item $dest -Force -Recurse }
  New-Item -ItemType Junction -Path $dest -Target $src | Out-Null
}
```

If a line fails, Step W3 is still enough for most apps.

## W5. Turn the washer on

Every time you want to clean a file:

```powershell
cd $HOME\watermarks-remover
py service\scripts\server.py --host 127.0.0.1 --port 8765
```

Leave that window open.

Open a **second** PowerShell window and check:

```powershell
curl.exe -s http://127.0.0.1:8765/health
```

You should see `{"ok": true, ...}`.

When you are done, click the first window and press **Ctrl + C**.

## W6. Clean a file

1. Keep the washer running.
2. Open **Claude Code**, **Codex**, **Grok Build**, or **Antigravity**.
3. Start a **new** session.
4. Type `/remove-ai-marks`.
5. Paste your file path: **Shift + right-click** the file → **Copy as path**.

```text
/remove-ai-marks C:\Users\YourName\Documents\draft.md
```

It writes `draft.cleaned.md`. The original stays safe.

### Clean without an AI (Windows)

```powershell
cd $HOME\watermarks-remover
py service\scripts\inspect_file.py C:\path\to\your-file.md
py service\scripts\clean_file.py C:\path\to\your-file.md -o C:\path\to\your-file.cleaned.md
```

---

# Mac

## M1. What you need

You already have Terminal. Also need:

1. `git` (if asked: `xcode-select --install`)
2. Python 3
3. Node.js (for `npx`)

Then in Terminal:

```bash
git --version
python3 --version
npx --version
```

All three should print a version number.

## M2. Get the official tool

```bash
cd ~
git clone --depth 1 --branch v0.5.0 https://github.com/guillaumemeyer/watermarks-remover.git
cd watermarks-remover
```

Already downloaded?

```bash
cd ~/watermarks-remover
```

Source: [guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover)

## M3. Teach the coding AIs

```bash
npx skills add . --skill remove-ai-marks -g -y -a grok -a claude-code -a gemini-cli -a antigravity -a antigravity-cli -a codex
```

Wait until it says **Done**.

## M4. Extra links (so every app finds the skill)

```bash
mkdir -p ~/.grok/skills ~/.claude/skills ~/.codex/skills \
  ~/.gemini/skills ~/.gemini/antigravity/skills ~/.gemini/antigravity-cli/skills

ln -sfn ~/.agents/skills/remove-ai-marks ~/.grok/skills/remove-ai-marks
ln -sfn ~/.agents/skills/remove-ai-marks ~/.claude/skills/remove-ai-marks
ln -sfn ~/.agents/skills/remove-ai-marks ~/.codex/skills/remove-ai-marks
ln -sfn ~/.agents/skills/remove-ai-marks ~/.gemini/skills/remove-ai-marks
ln -sfn ~/.agents/skills/remove-ai-marks ~/.gemini/antigravity/skills/remove-ai-marks
ln -sfn ~/.agents/skills/remove-ai-marks ~/.gemini/antigravity-cli/skills/remove-ai-marks
```

`~` means **your** home folder on this Mac.

## M5. Turn the washer on

Every time you want to clean a file:

```bash
cd ~/watermarks-remover
python3 service/scripts/server.py --host 127.0.0.1 --port 8765
```

Or, if you have `make`:

```bash
cd ~/watermarks-remover
make serve
```

Leave that window open.

Open a **second** Terminal window and check:

```bash
curl -s http://127.0.0.1:8765/health
```

You should see `{"ok": true, ...}`.

When you are done, click the first window and press **Ctrl + C**.

## M6. Clean a file

1. Keep the washer running.
2. Open **Claude Code**, **Codex**, **Grok Build**, or **Antigravity**.
3. Start a **new** session.
4. Type `/remove-ai-marks`.
5. Paste your file path: right-click the file, hold **Option**, click **Copy as Pathname**.

```text
/remove-ai-marks /Users/yourname/Documents/draft.md
```

It writes `draft.cleaned.md`. The original stays safe.

### Clean without an AI (Mac)

```bash
cd ~/watermarks-remover
python3 service/scripts/inspect_file.py /path/to/your-file.md
python3 service/scripts/clean_file.py /path/to/your-file.md -o /path/to/your-file.cleaned.md
```

---

# Gemini extra (both computers)

Need the `agy` command?

- **Windows:** install [Antigravity CLI](https://antigravity.google/product/antigravity-cli)
- **Mac:** same link, or `brew install --cask antigravity-cli`

---

# If something goes wrong

| Problem | Fix |
| --- | --- |
| `py` / `python3` not found | Install Python. On Windows tick **Add to PATH**, then open a **new** PowerShell. |
| `npx` not found | Install Node.js, then open a **new** terminal. |
| `git` not found | Install Git, then open a **new** terminal. |
| Skill not found | New session. Use Claude Code / Codex / Grok / Antigravity, not the website. |
| Service is down | Do W5 or M5 again. |
| Check washer | Windows: `curl.exe -s http://127.0.0.1:8765/health` · Mac: `curl -s http://127.0.0.1:8765/health` |

---

# What it can and cannot do

**It can**

- Remove hidden invisible characters in text
- Remove AI tags / C2PA / extra info from pictures and docs
- Offer a rewrite of the words (this changes how it sounds)

**It cannot**

- Prove a human wrote something
- Clean video or audio watermarks
- Work from a normal chat website

Only use it on files you own.

Full details: [official README](https://github.com/guillaumemeyer/watermarks-remover#readme)

---

# Tiny cheat sheet

**Windows**

```text
1. PowerShell → cd $HOME\watermarks-remover
2. py service\scripts\server.py --host 127.0.0.1 --port 8765
3. Other window: curl.exe -s http://127.0.0.1:8765/health
4. Open Claude Code / Codex / grok / agy
5. /remove-ai-marks + your file
6. Ctrl + C when done
```

**Mac**

```text
1. Terminal → cd ~/watermarks-remover
2. python3 service/scripts/server.py --host 127.0.0.1 --port 8765
3. Other window: curl -s http://127.0.0.1:8765/health
4. Open Claude Code / Codex / grok / agy
5. /remove-ai-marks + your file
6. Ctrl + C when done
```

---

## Credits

- Tool: [guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover)
- Skill name in that repo: [`remove-ai-marks`](https://github.com/guillaumemeyer/watermarks-remover/tree/main/skills/remove-ai-marks)
- This repo: install steps only, with a full Windows track and a full Mac track
