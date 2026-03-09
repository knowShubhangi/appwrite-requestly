# API Testing with Requestly

> **New contributor?** This guide gets you from zero to your first successful Appwrite API call in under 60 seconds — no manual setup, no copy-pasting URLs, no config headaches.

This repo ships with a ready-to-use [Requestly](https://requestly.com) workspace inside the `appwrite-requestly/` folder. The collection is version-controlled alongside the code, so it's always up to date.

---

## Prerequisites

Before you start, make sure you have:

- A free [Appwrite Cloud account](https://cloud.appwrite.io) — takes 2 minutes to sign up
- [Requestly Desktop](https://requestly.com/desktop) installed on your machine
- This repo cloned locally:
  ```bash
  git clone https://github.com/knowShubhangi/appwrite-requestly.git
  cd appwrite-requestly
  ```

---

## Step 1 — Open the Workspace in Requestly

1. Open the **Requestly** desktop app
2. On the home screen, click **"Open Local Workspace"**
3. A file picker will open — navigate to your cloned `appwrite-requestly/` folder and select it
4. Click **"Use existing workspace"** if prompted

> ✅ Requestly will automatically detect the `appwrite-requestly/` folder and load the collection and environment. You don't need to import anything manually.

---

## Step 2 — Find Your Appwrite Project ID

Every API request needs to know which Appwrite project it's talking to. Here's how to find yours:

1. Go to [cloud.appwrite.io](https://cloud.appwrite.io) and log in
2. Click on your project from the dashboard (or create a new one if you don't have one yet)
3. In the left sidebar, scroll to the bottom and click **"Settings"**
4. You'll see **"Project ID"** near the top — it looks something like `69a7f4d40032d4e4643d`
5. Copy it — you'll need it in the next step

---

## Step 3 — Create a Test User in Appwrite

Before you can test any authenticated API, you need a user account in your Appwrite project. Here's how to create one:

1. Go to [cloud.appwrite.io](https://cloud.appwrite.io) and open your project
2. In the left sidebar, click **"Auth"**
3. Click the **"Users"** tab at the top
4. Click **"Create User"** in the top right
5. Fill in the form:
   - **User ID**: leave it as `unique()` or type any ID you like
   - **Email**: use the same email you'll put in `user_email` (e.g. `test@email.com`)
   - **Password**: use something strong — at least 8 characters, not a common password (e.g. `Appwrite@2024!`)
   - **Name**: anything you like (e.g. `Test User`)
6. Click **"Create"**

> ⚠️ Make sure the email and password you use here **exactly match** what you set in the `user_email` and `user_password` environment variables in the next step. Otherwise the Login request will return a `401`.

---

## Step 4 — Set Your Environment Variables



This is where you tell Requestly your credentials. Think of it like a `.env` file, but with a UI.

1. In Requestly, look at the top bar — you'll see a dropdown that says **"Appwrite Cloud"**. That's your active environment.
2. Click the dropdown → **"Edit"**, or open it from the left sidebar under **Environments → Appwrite Cloud**
3. You'll see a table with two columns: **Initial Value (SYNCED)** and **Current Value (LOCAL)**

Fill in the table like this:

| Key | Initial Value (SYNCED) | Current Value (LOCAL) |
|---|---|---|
| `base_url` | `https://cloud.appwrite.io/v1` | `https://cloud.appwrite.io/v1` |
| `project_id` | your Project ID from Step 2 | your Project ID from Step 2 |
| `user_email` | your test account email | your test account email |
| `user_password` | *(leave blank)* | your test account password |

> ⚠️ **Why two columns?**
> - **SYNCED (Initial Value)** = gets saved to the file and committed to GitHub. Safe for non-sensitive values like URLs and project IDs.
> - **LOCAL (Current Value)** = only lives on your machine, never committed to Git. Always use this for passwords and secrets.
>
> Rule of thumb: if you wouldn't post it publicly, it goes in LOCAL only.

4. Click **Save** (`Ctrl+S`) when done

---

## Step 5 — Make Your First API Call

1. In the left sidebar, expand **"Appwrite API"**
2. Click on **"Create Email Session (Login)"**
3. Make sure the environment dropdown at the top shows **"Appwrite Cloud"**
4. Click the blue **"Send"** button

You should see a `201` response on the right with a JSON body containing a session ID, your user ID, country, and more. That's a live Appwrite session — you're in! 🎉

---

## Included API Requests

This collection includes 10 requests covering the full account lifecycle — from registration to logout. **Run them in the order listed below** for the best experience.

> 🔑 **Important:** Most requests require an active session. Always run **"Create Email Session (Login)"** first before trying requests like Get Account, Update Name, or Get Logs. Requestly automatically carries the session cookie across requests — no manual token copying needed.

| # | Request | Method | Endpoint | What it does |
|---|---|---|---|---|
| 1 | **Register** | `POST` | `/account` | Creates a new user account |
| 2 | **Create Email Session (Login)** | `POST` | `/account/sessions/email` | Logs in and creates a session — **run this before steps 3–9** |
| 3 | **Get Account** | `GET` | `/account` | Returns the current logged-in user's profile |
| 4 | **Update Account Name** | `PATCH` | `/account/name` | Updates the display name of the current user |
| 5 | **Get Account Preferences** | `GET` | `/account/prefs` | Fetches saved preferences for the current user |
| 6 | **Update Account Preferences** | `PATCH` | `/account/prefs` | Saves custom preferences (e.g. theme, notifications) |
| 7 | **Get Account Sessions** | `GET` | `/account/sessions` | Lists all active sessions for the current user |
| 8 | **Get Account Logs** | `GET` | `/account/logs` | Shows recent activity and login history |
| 9 | **Create Anonymous Session** | `POST` | `/account/sessions/anonymous` | Creates a guest session without credentials |
| 10 | **Delete All Sessions (Logout)** | `DELETE` | `/account/sessions` | Logs out and ends all active sessions |

### The flow this tells

**Register → Login → View profile → Customise → Inspect sessions → Logout**

This covers everything a new contributor needs to understand how Appwrite handles user authentication end to end.

---

## Want All 80+ Appwrite APIs?

The 10 requests above cover the full account lifecycle. But if you want every single Appwrite endpoint — Databases, Storage, Functions, Messaging, and more — you can import the full collection in one click.

### Where to find it

Appwrite publishes their complete OpenAPI 3.0 specification in their official [`appwrite/specs`](https://github.com/appwrite/specs) repository on GitHub.

### How to import it in Requestly

1. Head to the [`appwrite/specs` repo](https://github.com/appwrite/specs) and download the latest `open-api3-latest-client.json` file
2. In Requestly, click **"Import"** in the top bar
3. Select **"OpenAPI Specifications"**
4. Choose the downloaded file
5. Requestly imports all 80+ endpoints into a new collection instantly — no manual work

> 💡 This is the same spec Appwrite uses to generate their official SDKs, so it's always accurate and in sync with the latest API version.

---

## Why Requestly over Postman or cURL?

### vs Postman

| | Postman | Requestly |
|---|---|---|
| Requires an account to use | ✅ Yes — mandatory since 2023 | ❌ No account needed |
| Collection lives inside your repo | ❌ Lives in Postman's cloud workspace | ✅ Plain JSON files, directly in the repo |
| New contributor setup | Clone repo → open Postman → manually import collection | Clone repo → open workspace. Done. |
| Git collaboration | Requires Postman's own Git integration or manual export/import | Native — just commit and PR like any code file |
| Works fully offline | ❌ Requires cloud sign-in | ✅ Fully local |

### vs cURL

- **Visual interface** — see request and response side by side, no parsing raw terminal output
- **One-click resend** — tweak a parameter and retry without rewriting the command
- **Environment variables** — switch between local, staging, and cloud with one dropdown, no find-and-replace across a list of commands
- **Session handling** — Requestly carries session cookies automatically across requests, so you don't have to manually copy tokens between calls
- **Zero setup for contributors** — clone the repo, open the workspace, and you have the exact same environment as everyone else

---

## Updating the Collection

Found a missing endpoint? Want to improve an existing request?

1. Make your changes directly in Requestly
2. The collection files in `appwrite-requestly/` will auto-save to disk
3. Commit the changes and open a PR — just like updating any other file in the codebase

This is the core idea behind git-based API collaboration. 🚀