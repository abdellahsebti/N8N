# 🎯 n8n Mastery Roadmap — The Milestones Method

> **Methodology:** Break down n8n into progressive milestones. Do not advance to the next milestone until every checkpoint box is ticked.

| # | Milestone | Status |
|---|-----------|--------|
| 0 | Deployment & Infrastructure Setup | ⬜ |
| 1 | The Mental Model (Anatomy of a Workflow) | ⬜ |
| 2 | The Currency (JSON & Items) | ⬜ |
| 3 | The Language (Expressions & Mapping) | ⬜ |
| 4 | The Brain (Logic & Control Flow) | ⬜ |
| 5 | The Keys (Credentials & Security) | ⬜ |
| 6 | The Reality (Testing, Production & Debugging) | ⬜ |
| 7 | The Outside World (Webhooks & Sub-workflows) | ⬜ |
| 8 | Operations & Hardening (Self-Hosted) | ⬜ |

---

## ✅ Milestone 0: Deployment & Infrastructure Setup
**Goal:** A running, persistent, and correctly configured n8n instance.
*Note: If using n8n Cloud, you can skip to Milestone 1.*

**Concepts:** Docker Compose, environment variables, named volumes, persistent storage, encryption keys.

- [ ] Deploy n8n via Docker (or your preferred container orchestration tool).
- [ ] Configure environment variables (`.env`): Timezones, execution data pruning, and webhooks.
- [ ] Set a strong `N8N_ENCRYPTION_KEY` to secure saved credentials.
- [ ] Ensure persistent storage is mapped so data survives container restarts.
- [ ] Log in and create the initial Owner account.

---

## 🏁 Milestone 1: The Mental Model (Anatomy of a Workflow)
**Goal:** Understand the canvas and the three node families.

**Concepts:**
- A **Workflow** is a visual flowchart; data flows strictly **left → right**.
- **Trigger nodes** = the "When" (every workflow starts with exactly one).
- **Action nodes** = the "Do" (send emails, update databases, call APIs).
- **Logic nodes** = the "Decide" (filter, branch, merge).
- **Connections** (lines) = the data path between nodes.

**Hands-on:**
- [ ] Create a new workflow and add a **Schedule Trigger** (set to run every 5 minutes for testing).
- [ ] Connect a **Code node** or **Edit Fields (Set) node** to generate some mock data.
- [ ] Connect an Action node (e.g., Send Email, Telegram, Discord, or a simple logging node).
- [ ] Click **Execute Workflow** manually to test the flow.
- [ ] **Publish / Activate** the workflow and verify it runs automatically on schedule via the **Executions** tab.

**Checkpoint:**
- [ ] I can explain Trigger vs Action vs Logic nodes out loud.
- [ ] I have a scheduled workflow successfully running in the background.

---

## 🏁 Milestone 2: The Currency (JSON & Items)
**Goal:** Understand exactly how data moves through n8n.

**Concepts:**
- Everything in n8n is an **Item** (a single JSON object: `{ "key": "value" }`).
- Nodes output an **array of items**; the blue badge shows the count.
- **The Multiplication Rule:** If N items enter an action node, the node executes N times (once per item).
- Each node only sees data from nodes to its **left**.

**Hands-on:**
- [ ] Create a node that outputs **multiple items** (e.g., fetching a list of 5 users from an API or generating an array in a Code node).
- [ ] Connect an action node (like a logging or notification node) and observe it firing exactly 5 times.
- [ ] Use an aggregation method (like the "Item Lists" node or Code node) to merge those 5 items into **1 single item** before sending it to the action node.

**Checkpoint:**
- [ ] I can explain what an "item" is in n8n.
- [ ] Given N input items, I can accurately predict how many times a downstream node will execute.

---

## 🏁 Milestone 3: The Language (Expressions & Mapping)
**Goal:** Extract and inject specific data between nodes dynamically.

**Concepts:**
- **Expressions:** JavaScript wrapped in curly braces `{{ ... }}`.
- `$json` refers to the current input item (e.g., `{{ $json.email }}`).
- **Cross-node references:** `{{ $node["Node Name"].json.field }}`.
- **Drag & Drop Mapper:** Dragging data from the Input panel directly into a node's parameter field.

**Hands-on:**
- [ ] Hand-write an expression to combine data from two different previous nodes into a single text field.
- [ ] Delete the hand-written expression and recreate the exact same output using only the Drag & Drop UI mapper.
- [ ] Use an expression inside a URL parameter or HTTP Request header.

**Checkpoint:**
- [ ] I can write basic expressions using `$json`.
- [ ] I can reference data from a non-adjacent node by its name.

---

## 🏁 Milestone 4: The Brain (Logic & Control Flow)
**Goal:** Make workflows smart: branch, filter, and loop.

**Concepts:**
- **Filter:** Drops items that don't match a condition.
- **If / Switch:** Splits the flow into distinct branches based on conditions.
- **Merge:** Joins branches back together.
- **Loop Over Items (Split In Batches):** Processes items in chunks to respect API rate limits.
- **Wait:** Pauses execution between batches.

**Hands-on:**
- [ ] Build a workflow that receives multiple items and uses an **If** node to route them down two different paths based on a specific value.
- [ ] Use a **Filter** node to drop invalid data before it reaches an API call.
- [ ] Simulate an API rate limit by processing a large array of items using **Loop Over Items** combined with a **Wait** node (e.g., process 10 items, wait 2 seconds, repeat).

**Checkpoint:**
- [ ] I can filter items by condition.
- [ ] I can branch flow with an If node and handle both the "True" and "False" paths.

---

## 🏁 Milestone 5: The Keys (Credentials & Security)
**Goal:** Manage secrets safely, never hardcoding them in nodes.

**Concepts:**
- **Credentials Vault:** Centralized, encrypted storage for API keys and passwords.
- **Reusability:** One credential can be applied across dozens of workflows.
- **Rotation:** Update a credential once in the vault, and all workflows update instantly.

**Hands-on:**
- [ ] Create a credential for an external service (e.g., GitHub, Slack, or a database).
- [ ] Use that exact same credential in two different, unrelated workflows.
- [ ] (Optional) Revoke/regenerate an API key on the provider's side, update it in the n8n vault, and verify both workflows instantly work again without modifying the nodes.

**Checkpoint:**
- [ ] Zero API keys or passwords are hardcoded into any node's text fields.
- [ ] I know how to update a global credential without editing individual workflows.

---

## 🏁 Milestone 6: The Reality (Testing, Production & Debugging)
**Goal:** Master the editor-vs-production split and debug failures like a pro.

**Concepts:**
- **Editor Mode (Execute Workflow):** Manual, sandboxed runs for building.
- **Production Mode (Active/Published):** Background runs triggered by events/schedules.
- **Executions Tab:** The history log of every production run, allowing per-node inspection.
- **Pruning:** Automatically deleting old execution data to save database space.

**Hands-on:**
- [ ] Intentionally break a workflow (e.g., reference a JSON field that doesn't exist in an HTTP Request).
- [ ] Publish the workflow, let it trigger, and open the **Executions** tab.
- [ ] Locate the failed execution, click on the red node, and read the error message to diagnose the issue.
- [ ] Fix the node, re-execute, and confirm a green success state.

**Checkpoint:**
- [ ] I can explain the difference between Editor execution and Production execution.
- [ ] I can confidently diagnose a failure using the Executions log.

---

## 🏁 Milestone 7: The Outside World (Webhooks, Sub-workflows & Errors)
**Goal:** Let external systems talk to n8n, and structure workflows like modular software.

**Concepts:**
- **Webhook Trigger:** Generates a unique URL; any HTTP request to this URL wakes the workflow.
- **Execute Workflow Node:** Calls a "child" workflow from a "parent" workflow (modularity).
- **Error Trigger:** A special workflow that fires automatically when another workflow crashes.

**Hands-on:**
- [ ] Add a **Webhook Trigger** to a workflow. Copy the Production URL and trigger it using an external tool like `curl`, Postman, or a third-party service.
- [ ] Build a "Child" workflow (e.g., a standardized "Send Alert" flow). Build a "Parent" workflow that calls it using the **Execute Workflow** node.
- [ ] Build an **Error Workflow** using the Error Trigger node. Set your main workflows to route their errors to this Error Workflow so you get notified if they fail.

**Checkpoint:**
- [ ] An external device/service successfully triggered my workflow via a Webhook.
- [ ] I reused logic across multiple workflows using sub-workflows.
- [ ] I have a global error handler in place for failed automations.

---

## 🏁 Milestone 8: Operations & Hardening (Self-Hosted)
**Goal:** Production-grade infrastructure around your n8n instance.

**Tasks:**
- [ ] **HTTPS:** Place n8n behind a reverse proxy (Nginx Proxy Manager, Traefik, Caddy) or a tunnel (Cloudflare). Enable secure cookies (`N8N_SECURE_COOKIE=true`).
- [ ] **Database:** Migrate from the default SQLite database to **PostgreSQL** for better performance and concurrency.
- [ ] **Backups:** Implement a cron job to back up your n8n database and file storage. *Crucial: Test the restore process.*
- [ ] **Updates:** Document a standard operating procedure for updating the n8n Docker image safely (always backup first!).
- [ ] **Monitoring:** Set up an uptime monitor (like Uptime Kuma) to alert you if your n8n instance goes offline.

**Checkpoint:**
- [ ] n8n is accessible via valid HTTPS.
- [ ] Database is migrated to Postgres (or backed up reliably if sticking to SQLite).
- [ ] A disaster recovery restore has been successfully tested.

---

## 🎓 The n8n Mindset — Cheat Sheet
1. A **Trigger** starts the flow.
2. Data moves **left → right** as **JSON items**.
3. **Expressions** (`{{ $json.x }}`) extract the specific data you need.
4. **Logic nodes** filter, branch, merge, and loop that data.
5. **Credentials** authenticate your actions — never hardcode secrets.
6. **Test** in the editor, **Publish** for production.
7. **Debug** via the Executions log, inspecting data node by node.