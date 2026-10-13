<p align="center">
  <img src="assets/pumpkin.svg" alt="Hacktoberfest pumpkin" width="120">
</p>

# <img src="assets/pumpkin.svg" alt="" height="32"> Hacktoberfest Skill

**Get any GitHub repository Hacktoberfest-ready in minutes, with an AI agent doing the prep and you making every call.**

Works with IBM Bob ([free trial](https://bob.ibm.com/trial)), Claude Code, or any AI coding agent that can run shell commands.

**This skill is for everyone.** Any maintainer, of any GitHub repository, anywhere in the world, can use it to get ready for the worldwide [Hacktoberfest](https://hacktoberfest.com). You don't need to work at IBM or have any IBM affiliation.

The IBM InnerSource team built it and uses it on our own open source projects. We also run an internal copy to support IBM Hacktoberfest, where IBMers contribute to IBM's InnerSource and open source projects every October. It worked so well that we wanted every maintainer to have it.

The skill walks your AI coding agent through a guided, step-by-step workflow that:

1. ✅ Checks that the `gh` CLI is installed and authenticated
2. 🏷️ Verifies your repo has the `hacktoberfest` topic, and adds it (without wiping your existing topics) if you approve
3. 🔎 Pulls your open issues from a lookback window you choose (6 months, 12 months, or all time)
4. 🧠 Triages each issue into **Recommended**, **Possible**, or **Not recommended** based on labels, scope, clarity, and assignment status
5. 🙋 Shows you the list and **waits for your approval** before touching anything
6. 🟠 Creates the `hacktoberfest` label if needed and applies it only to the issues you choose
7. 📋 Ends with a summary and next steps

Nothing is changed on your repository without your explicit "yes."

---

## Why this matters in 2026

Hacktoberfest 2026 is focused on learning and building with open source AI and open-weight models. Pull requests **no longer count toward Hacktoberfest rewards**, a change made in response to maintainer feedback about low-effort spam PRs.

That makes a well-prepared repo more valuable, not less. Contributors who show up this year are coming to learn and build, and they need clear, scoped, beginner-friendly issues to start with. This skill helps you surface exactly those issues and keep the vague, blocked, or internal-only work out of the spotlight.

---

## What's in this repo

```
hacktoberfest-skill/
├── README.md
├── HACKTOBERFEST_SETUP-PROMPT.md   # The skill prompt
└── assets/
    └── pumpkin.svg                 # Our Hacktoberfest pumpkin
```

---

## Prerequisites

- An AI coding agent that can run shell commands in your terminal. Use whichever one you like: we built and tested this with **IBM Bob** ([start a free trial](https://bob.ibm.com/trial)), and because the prompt is plain Markdown, it also works with Claude Code and other agents.
- [GitHub CLI (`gh`)](https://cli.github.com), authenticated to `github.com`
- [`jq`](https://jqlang.org) and `curl`
- Maintainer or admin access to the repository you want to prepare
- A GitHub personal access token with `repo` scope (the skill walks you through getting one)

---

## Quick start

### 1. Get the skill

```bash
git clone https://github.com/InnerSource-IBM/hacktoberfest-skill.git
```

### 2. Add it to your agent

Pick the agent you already use. Don't have one? [Try IBM Bob free](https://bob.ibm.com/trial).

**IBM Bob:** create a skill folder in your workspace and copy the prompt in.

```bash
mkdir -p .bob/skills/hacktoberfest
cp hacktoberfest-skill/HACKTOBERFEST_SETUP-PROMPT.md .bob/skills/hacktoberfest/
```

**Claude Code:** create a skill folder and save the fenced prompt block from `HACKTOBERFEST_SETUP-PROMPT.md` as `SKILL.md`, with a short frontmatter header so Claude knows when to use it:

```bash
mkdir -p .claude/skills/hacktoberfest
```

```markdown
---
name: hacktoberfest
description: Prepare a GitHub repository for Hacktoberfest by checking the hacktoberfest topic and triaging and labeling open issues.
---

<paste the prompt here>
```

**Any other agent:** paste the fenced prompt block into your agent, or save it wherever your agent loads reusable instructions.

> **Using a folder other than `.bob/`?** The prompt stores your token at `.bob/skills/hacktoberfest/.ghtoken`. Find-and-replace that path in the prompt to match your agent's skills folder (for example, `.claude/skills/hacktoberfest/.ghtoken`).

### 3. Protect your token 🔐

The skill stores your GitHub token in a local file with `chmod 600` permissions. **Make sure it never gets committed:**

```bash
echo "**/skills/hacktoberfest/.ghtoken" >> .gitignore
```

### 4. Run it

Ask your agent to run the Hacktoberfest skill. It will ask you for the `owner/repo` you want to prepare (it never guesses from your current directory), then guide you through each step.

---

## What a run looks like

After triage, you'll see something like this before any labels are applied:

```
🎃 Hacktoberfest Issue Candidates: github.com/your-org/your-repo
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ Recommended for hacktoberfest label:
  #42   Fix typo in installation docs        [documentation, good first issue]
  #57   Add example for config file usage    [help wanted]

⚠️ Possible candidates (review before labelling):
  #63   Add unit tests for date parser       []

❌ Not recommended:
  #12   Redesign plugin architecture         [epic, blocked]

Summary: 2 issues recommended, 1 possible, 1 skipped.
```

Then you reply with `all recommended`, specific numbers like `#42 and #63`, or `none`.

---

## How issues are triaged

| | Signals |
|---|---|
| ✅ **Recommended** | Labeled `good first issue`, `help wanted`, or `documentation`; words like *beginner*, *easy*, *docs*, *typo*; small, self-contained scope with clear expected outcomes; unassigned |
| ⚠️ **Possible** | Isolated feature requests or unit tests that may need a bit of onboarding context |
| ❌ **Not recommended** | Vague scope; part of a large epic; blocked by dependencies; needs internal credentials or restricted access |

Want different rules for your project? Edit Step 6 of the prompt.

---

## After you run it

The skill gets your repo discoverable. These steps make it welcoming:

- **Review the "Possible" list** and tighten those issues so they're ready to label.
- **Write clear acceptance criteria** on every labeled issue so contributors know what "done" looks like.
- **Pin your best starter issues** to the top of the repo.
- **Check your `CONTRIBUTING.md`** and make sure setup instructions actually work for a newcomer.
- **Announce it** in your community channels, at a local [Hacktoberfest Fest](https://hacktoberfest.com/fests/), or on DEV.

---

## Safety by design

- **Approval gates.** The skill stops and asks before adding a topic and before applying any label.
- **Non-destructive topic updates.** GitHub's Topics API replaces the full list, so the skill always reads your existing topics, validates them, and appends `hacktoberfest` rather than overwriting.
- **No auto-detection.** You always name the target repo explicitly.
- **Local token handling.** Your token stays in a permission-restricted local file. Keep it out of version control (see step 3 above), and revoke it when Hacktoberfest is over.

---

## Contributing

Ideas, fixes, and adaptations for other AI agents are welcome. Open an issue or a pull request. Yes, we'd love it if you labeled your first one `hacktoberfest`. <img src="assets/pumpkin.svg" alt="" height="18">

---

## Meet the team

We're demoing this skill live at **MLH x DigitalOcean @ AI Engineer NYC, hosted by IBM** on Wednesday, October 14, 2026, 6:00 to 9:00 PM ET, at IBM One Madison Avenue in New York. [Register here](https://ibm.biz/hacktoberfest2026-mlh).

---

## Resources

- [Hacktoberfest 2026](https://hacktoberfest.com)
- [Hacktoberfest FAQs](https://hacktoberfest.com/questions/)
- [GitHub CLI docs](https://cli.github.com/manual/)
- [GitHub REST API: repository topics](https://docs.github.com/en/rest/repos/repos#replace-all-repository-topics)
- [InnerSource Commons](https://innersourcecommons.org)

---

<p align="center">
  <a href="https://github.com/InnerSource-IBM">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/InnerSource-IBM/.github/main/profile/assets/innersource-logo-dark.png">
      <img src="https://raw.githubusercontent.com/InnerSource-IBM/.github/main/profile/assets/innersource-logo.png" alt="IBM InnerSource logo" width="48">
    </picture>
  </a>
  <br>
  <sub>Built with 🧡 by the IBM InnerSource Program Office. Part of <a href="https://github.com/InnerSource-IBM">IBM InnerSource</a>.</sub>
</p>
