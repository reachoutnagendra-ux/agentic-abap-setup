# SAP Dev Setup — Copilot Agent Workflows

Automated SAP developer environment setup using GitHub Copilot Agent Mode in VS Code.

---

## Step 0 — Set Up GitHub Copilot (skip if you already have it)

GitHub Copilot is built into VS Code — no separate extension install needed.
You just need a GitHub account and to activate it.

### 0.1 Create a GitHub account
Go to https://github.com/signup — free, no credit card needed.

### 0.2 Enable Copilot in VS Code
1. Open VS Code
2. Look for the **Copilot icon** in the bottom status bar (looks like two angle brackets `>_`)
3. Hover over it and click **Use AI Features**
4. Choose a sign-in method and follow the prompts in your browser
5. Sign in with your GitHub account

> VS Code will automatically sign you up for the **Copilot Free plan** if you don't
> have an existing subscription. No credit card needed.
> You get **2,000 code completions** and **50 chat/agent requests** per month.

### 0.3 Verify Copilot is active
- The Copilot icon in the status bar should now show as active (not greyed out)
- Press `Ctrl+Alt+I` (or `Cmd+Alt+I` on Mac) — the Copilot Chat panel should open on the right

> **For ongoing ABAP development** beyond the initial setup, 50 requests/month
> will run out quickly. Consider upgrading to **Copilot Pro at $10/month**:
> https://github.com/features/copilot/plans

---

## Step 1 — Get This Repo

Clone it or download it as a ZIP from GitHub:

```bash
git clone https://github.com/YOUR_USERNAME/sap-dev-setup.git
cd sap-dev-setup
code .
```

---

## Step 2 — Run a Setup Flow

1. **Open Copilot Chat**
   Press `Ctrl+Alt+I` (or `Cmd+Alt+I` on Mac)

2. **Switch to Agent mode**
   Click the mode dropdown in the chat panel and select **Agent**

3. **Type the command for what you need:**

   | Command | What it does |
   |---|---|
   | `/setup-adt` | Install ADT extension + connect to BTP ABAP Environment via service key |
   | More coming soon | Fiori Tools, abapGit, Cloud Connector |

4. **Follow the agent's prompts.**
   It runs commands automatically and pauses only when it needs something from you
   — like your BTP service key or system URL.

---

## What the Agent Handles Automatically
- Checking prerequisites (Java 21, VS Code version)
- Installing the SAP ADT VS Code extension
- Parsing your BTP service key and extracting connection details
- Verifying each step before moving to the next
- Troubleshooting and retrying on errors
- Activating the ADT MCP server for Copilot ↔ ABAP integration

## What You Always Provide
- Your BTP service key JSON (never saved to disk)
- Confirmation of your system URL
- Your SAP username/password when prompted

---

## Security Notes
- Service keys and credentials are **never written to disk** by the agent
- All sensitive values are held in the chat session only
- The agent will remind you of this at each step where credentials are needed

---

## Folder Structure
```
.github/
  copilot-instructions.md   ← Global agent rules (auto-loaded by Copilot)
  prompts/
    setup-adt.prompt.md     ← /setup-adt slash command
README.md                   ← This file
```

## Adding More Setup Flows
Copy `.github/prompts/setup-adt.prompt.md` as a template.
Name new files `<command>.prompt.md` — the filename becomes the slash command.
Follow the same Phase structure: Check → Install → Collect Input → Configure → Verify.
