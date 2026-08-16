# Easy install guide: watermarks-remover

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

## The one rule

Use the **coding app**, not the **website chat**.

| App you might open | Use it? |
| --- | --- |
| grok.com, Grok on your phone | No |
| **Grok Build** (type `grok` in Terminal) | **Yes** |
| claude.ai website | No |
| Claude Cowork | No |
| **Claude Code** | **Yes** |
| gemini.google.com | No |
| **Antigravity** or type `agy` in Terminal | **Yes** |
| chatgpt.com | No |
| **Codex** (app or type `codex` in Terminal) | **Yes** |

Website chat can rewrite words if you paste them. It **cannot** clean files.

---

## What you need

- A Mac
- Terminal (Spotlight → type **Terminal** → Enter)

---

# Step 1 — Get the official tool

1. Open **Terminal**.
2. Copy this whole block. Paste it. Press Enter.

```bash
mkdir -p ~/Documents/video\ tutorials/watermark-remover
cd ~/Documents/video\ tutorials/watermark-remover
git clone --depth 1 --branch v0.5.0 https://github.com/guillaumemeyer/watermarks-remover.git .
```

If it says the folder is not empty, you already have the tool. That’s fine. Keep going.

Source: [guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover)

---

# Step 2 — Teach every coding AI

Still in Terminal, copy this. Paste. Press Enter.

```bash
cd ~/Documents/video\ tutorials/watermark-remover
npx skills add . --skill remove-ai-marks -g -y \
  -a grok -a claude-code -a gemini-cli -a antigravity -a antigravity-cli -a codex
```

Wait until it says **Done**.

This one command teaches Grok, Claude Code, Gemini, and Codex.

---

# Step 3 — Extra links (so nothing is missed)

Copy this. Paste. Press Enter.

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

---

# Turn the washer on

Do this **every time** you want to clean a file.

1. Open a Terminal window.
2. Copy this. Paste. Press Enter.

```bash
cd ~/Documents/video\ tutorials/watermark-remover
make serve
```

3. Leave that window open.

4. Open a **second** Terminal window and check:

```bash
curl -s http://127.0.0.1:8765/health
```

You should see something like:

```text
{"ok": true, "version": "dev"}
```

If you see that, the washer is on.

When you are finished cleaning, go back to the first Terminal and press **Control + C**. That turns it off. You do not need it running all day.

---

# How to use it

1. Turn the washer on (step above).
2. Open **one** coding app (not a website).
3. Start a **new** chat / session.
4. Type:

```text
/remove-ai-marks
```

5. Then give it a file, like:

```text
/remove-ai-marks /Users/you/Desktop/draft.md
```

or just say:

```text
Please strip AI watermarks from this file: /Users/you/Desktop/photo.png
```

6. It should make a new file named something like `draft.cleaned.md`. Your original stays safe.

---

## Grok

1. Open Terminal.
2. Type `grok` and press Enter.
3. Type `/remove-ai-marks` and your file.

Do **not** use grok.com.

---

## Claude

1. Open **Claude Code** (not claude.ai, not Cowork).
2. Start a new session.
3. Type `/remove-ai-marks` and your file.

---

## Gemini

1. In Terminal, type `agy` and press Enter.
   - First time may ask you to sign in. Do that once.
2. Type `/remove-ai-marks` and your file.

If you use the **Antigravity** app instead, open a new chat there and type the same command.

Do **not** use gemini.google.com.

Need the Gemini programs first?

```bash
brew install --cask antigravity-cli
```

---

## ChatGPT / OpenAI

1. Open **Codex** (the app, or type `codex` in Terminal).
2. Start a new session.
3. Type `/remove-ai-marks` and your file.

Do **not** use chatgpt.com.

---

# If something goes wrong

**The AI says it cannot find the skill**

- Close it and open a **new** session.
- Make sure you used the coding app, not the website.

**The AI says the service is down**

- You forgot to turn the washer on.
- Go back to [Turn the washer on](#turn-the-washer-on).

**You are not sure the washer is on**

```bash
curl -s http://127.0.0.1:8765/health
```

No `ok: true`? The washer is off.

---

# Clean a file yourself (no AI)

Washer must be on.

```bash
cd ~/Documents/video\ tutorials/watermark-remover

python3 service/scripts/inspect_file.py /Users/you/Desktop/draft.md
python3 service/scripts/clean_file.py /Users/you/Desktop/draft.md -o /Users/you/Desktop/draft.cleaned.md
```

Change the paths to your real file.

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

```text
1. Terminal → make serve          (leave it open)
2. Other Terminal → curl health   (look for ok: true)
3. Open grok / Claude Code / agy / Codex
4. Type /remove-ai-marks + your file
5. When done: Control + C in the serve window
```

---

## Credits

- Tool: [guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover)
- Skill name in that repo: [`remove-ai-marks`](https://github.com/guillaumemeyer/watermarks-remover/tree/main/skills/remove-ai-marks)
- This repo: install steps only, written so a beginner can follow them
