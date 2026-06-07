---
mode: agent
description: Install SAP ADT for VS Code and connect to a BTP ABAP Environment via instance URL
---

# SAP ADT Setup — BTP ABAP Environment

You are setting up the official SAP ABAP Development Tools (ADT) extension for VS Code
and connecting it to an SAP BTP ABAP Environment instance.

No Java or Eclipse required. Everything runs inside VS Code.

Work through the phases below in order. Do not skip phases.
Complete every verification check before moving to the next phase.

---

## PHASE 1 — Install the ADT Extension

### 1.1 Check if already installed

```bash
code --list-extensions | grep -i "SAPSE.adt-vscode"
```

- If found: ✅ "ADT extension already installed — skipping install." Move to Phase 2.
- If not found: proceed to 1.2.

### 1.2 Install the extension

```bash
code --install-extension SAPSE.adt-vscode
```

Wait for the command to complete. Then verify:

```bash
code --list-extensions | grep -i "SAPSE.adt-vscode"
```

- If found: ✅ "ADT extension installed successfully."
- If not found: tell the developer:
  > 1. Press **Ctrl+Shift+X** to open Extensions
  > 2. Search **ABAP Development Tools**
  > 3. Install the one published by **SAP SE**

---

## PHASE 2 — Collect the ABAP Service Instance URL

❓ **Ask the developer:**
> "Please paste your ABAP service instance URL.
>
> **Where to find it:**
> SAP BTP Cockpit → your subaccount → Services → Instances and Subscriptions
> → find your ABAP instance → copy the URL from the URL column
>
> **What it looks like:**
> ```
> https://my-system.abap.eu10.hana.ondemand.com
> ```
> Remove anything after `.hana.ondemand.com` if present."

Wait for the developer to provide the URL. Store it as [SYSTEM_URL].

### 2.1 Validate the URL

Check it:
- Starts with `https://`
- Contains `.abap.` in the middle
- Ends with `.hana.ondemand.com`

If they pasted a full path, strip it and confirm:
> "I'll use: **[cleaned URL]** — does that look right?"

---

## PHASE 3 — Create the ADT System Connection

⚠️ `ABAP: New System` opens a GUI dialog — the agent cannot pass the URL
automatically. Display it clearly so the developer can copy-paste it.

Tell the developer:

> **Your system URL is:**
> ```
> [SYSTEM_URL]
> ```
> **Copy it now, then follow these steps:**
>
> 1. Press **Ctrl+Shift+P** (Cmd+Shift+P on Mac)
> 2. Type **ABAP: New System** and press Enter
> 3. Paste your URL when the dialog asks for it
> 4. Give it a name e.g. **BTP-DEV**
> 5. A browser window will open — log in with your **BTP email and password**
> 6. After login close the browser and return to VS Code
>
> Tell me when done.

Wait for confirmation before continuing.

### 3.1 Handle login issues

- **"User not found"** → use BTP email, not SAP S-User
- **"No authorisation"** → BTP user needs `Developer` role:
  BTP Cockpit → your ABAP instance → Users → confirm Developer role is assigned
- **"System not reachable"** → remove any trailing path from the URL

---

## PHASE 4 — Log On to the System

⚠️ Logon requires a right-click in the VS Code Explorer — cannot be automated.

Tell the developer:

> **Now log on to your ABAP system:**
>
> 1. Look at the **Explorer panel** on the left side of VS Code
> 2. You should see **BTP-DEV** listed as a folder
> 3. **Right-click on BTP-DEV**
> 4. Select **Log On** from the menu
> 5. A browser window opens — log in with your BTP credentials
> 6. Close the browser and return to VS Code
> 7. BTP-DEV should now expand and show your ABAP packages
>
> Do you see your ABAP packages in the Explorer?

- If yes: ✅ proceed to Phase 5.
- If no:
  - **"Unauthorised"** → Developer role missing → BTP Cockpit → Security → Role Collections
  - **Empty folder** → right-click BTP-DEV → **Add to Favorite Packages**
  - **Log On option missing** → Ctrl+Shift+P → **Reload Window**

---

## PHASE 5 — Activate the ADT MCP Server (Automated)

The ADT MCP server lets Copilot Agent Mode read and write ABAP objects directly.
It is built into the extension but off by default.

The agent can enable it automatically by writing to VS Code's settings.json.

### 5.1 Find the settings.json path

On Mac:
```bash
cat "$HOME/Library/Application Support/Code/User/settings.json"
```

On Windows:
```bash
type "%APPDATA%\Code\User\settings.json"
```

### 5.2 Add the MCP enable setting

Read the current settings.json and add `"abap.ai.enableMcpServer": true` to it.
Be careful not to break existing JSON — merge it properly.

Example result:
```json
{
  "abap.ai.enableMcpServer": true
}
```

If the file does not exist, create it with just that setting.

After writing the file, tell the developer:
> "I've enabled the ADT MCP server in your settings. Now please reload VS Code:"
>
> Press **Ctrl+Shift+P** → type **Reload Window** → press Enter

Wait for the developer to confirm VS Code has reloaded.

### 5.3 Verify the MCP server is running

Tell the developer:
> 1. Open Copilot Chat (**Ctrl+Alt+I**)
> 2. Switch to **Agent** mode
> 3. Click the **Tools icon** (🔧) in the chat input bar
> 4. Look for **sap-abap-adt** in the list — it should be checked
>
> Do you see `sap-abap-adt`?

- If yes: ✅ proceed to Phase 6.
- If no:
  - Not in Agent mode → switch from Ask to Agent
  - Setting didn't save → verify `settings.json` was written correctly
  - Still missing → fully close and reopen VS Code

---

## PHASE 6 — Test the MCP Connection

Run a live test to confirm everything works end to end.

Tell the developer to type this in Copilot Chat (Agent mode):

> "Using the sap-abap-adt tools, list the top-level packages in my ABAP system."

- If packages are listed: ✅ MCP is fully working.
- If an error is returned:
  - Check BTP-DEV is logged on (right-click → Log On)
  - Confirm Developer role is assigned in BTP Cockpit

Once confirmed working, proceed to Phase 7.

---

## PHASE 7 — You Are Ready. Try Your First Agentic ABAP Task

Tell the developer:

> ✅ **Setup complete! Your environment is fully agentic.**
>
> - ADT extension: `SAPSE.adt-vscode` ✅
> - BTP system: `BTP-DEV` connected to [SYSTEM_URL] ✅
> - ADT MCP Server: `sap-abap-adt` active in Copilot Agent ✅
>
> ---
>
> **🚀 Try your first agentic ABAP task right now.**
>
> Copy one of these prompts into Copilot Chat (Agent mode) and watch it work:
>
> **Option A — Create an ABAP Class:**
> ```
> Using sap-abap-adt, create a new ABAP class called ZCL_HELLO_WORLD in package $TMP.
> Add a method called say_hello that returns the string 'Hello from Agentic ABAP!'.
> Activate the class after creating it.
> ```
>
> **Option B — Create a CDS View:**
> ```
> Using sap-abap-adt, create a simple CDS view called ZI_HELLO_WORLD in package $TMP
> that selects the fields carrid, carrname, and currcode from the SCARR table.
> Add a @AbapCatalog.sqlViewName annotation and activate it.
> ```
>
> Pick one and go. Copilot will use the sap-abap-adt tools to create the object
> directly in your ABAP system — no SAP GUI needed.

---

## Troubleshooting

| Error | Cause | Fix |
|---|---|---|
| Extension install fails | Proxy or no internet | Manual install via Ctrl+Shift+X |
| URL not recognised | Wrong format or trailing path | Use only `https://<instance>.abap.<region>.hana.ondemand.com` |
| Browser login fails | Wrong account type | Use BTP email, not SAP S-User |
| User not authorised | Missing Developer role | BTP Cockpit → Security → Role Collections → add Developer |
| Log On missing in right-click | Extension not fully loaded | Ctrl+Shift+P → Reload Window |
| sap-abap-adt not in tools list | Not in Agent mode | Switch Copilot Chat to Agent mode |
| MCP server not starting | settings.json not saved correctly | Check file manually, verify JSON is valid |
| MCP auth error | Session expired | Right-click BTP-DEV → Log On again |
| Class/CDS creation fails | Package $TMP not available | Ask your BTP admin for a development package |
