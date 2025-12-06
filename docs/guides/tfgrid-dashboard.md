# TFGrid Compose Dashboard

Status: **Production Ready (local dashboard)**

This guide explains how to use the `tfgrid-compose` local dashboard to manage apps and deployments visually.

The dashboard is a thin web UI on top of the existing CLI, config, and registry files. You can:

- Use the **command-line interface (CLI)** directly, _or_
- Use the **TFGrid Studio Dashboard** as a user-friendly browser UI that calls the same commands under the hood.

---

## 1. What the dashboard does

- **Visual app registry**
  - Browse registered apps (official + community) from your local registry.
  - Deploy apps (e.g. `tfgrid-ai-stack`) with one click.

- **CLI commands workspace**
  - Run almost any `tfgrid-compose` command from a **CLI Commands** panel.
  - Fill in arguments and flags using forms instead of typing full commands.
  - Commands are grouped as:
    - **Global CLI** – commands that run without a deployment context.
    - **For deployment** – commands that target a specific deployment.
  - A **Context** bar shows whether you are running globally or against a selected deployment.

- **Deployment overview**
  - List all deployments from `~/.config/tfgrid-compose/deployments.yaml`.
  - Show deployment ID, app name, IPs (VM/Mycelium), contract ID, status, created time.
  - For each deployment you can:
    - View addresses (wraps `tfgrid-compose address <deployment-id>`).
    - Set it as the active **deployment context** for CLI Commands.
    - Open an interactive shell (wraps `tfgrid-compose ssh <deployment-id>`).

- **Project actions for AI stack**
  - For `tfgrid-ai-stack` deployments, you can:
    - Create projects.
    - Run projects.
    - Publish projects.
  - The dashboard uses `tfgrid-compose create`, `run`, and `publish` under the hood.

- **Preferences panel**
  - Shows your whitelist/blacklist and thresholds from `preferences.yaml`.
  - Provides shortcuts that open the right CLI Commands forms for editing preferences.

- **Live logs & interactive shell**
  - All operations (deploy, create, run, publish, CLI Commands) stream their logs into the **Output** column.
  - You can open an interactive shell per deployment; the dashboard spawns `tfgrid-compose ssh <deployment-id>` and streams output into a browser terminal.

The dashboard does **not** replace the CLI. It wraps it in a convenient local web UI that stays in sync with your existing CLI config, registry and login.

---

## 1.1 Quick start

If you just want the shortest path to the dashboard:

```bash
# 1) Install / update tfgrid-compose
tfgrid-compose update           # or: t update

# 2) Login once with the CLI (required before deploying)
tfgrid-compose login

# 3) Start the local dashboard (foreground or background)
tfgrid-compose dashboard        # foreground
# or:
tfgrid-compose dashboard start  # background service

# 4) (Optional) Install a desktop launcher on Linux
tfgrid-compose dashboard desktop
```

Then either open the printed `http://localhost:PORT` URL in your browser,
or use the installed **TFGrid Studio Dashboard** launcher from your desktop/menu.

---

## 2. Installation & prerequisites

The dashboard is part of the normal `tfgrid-compose` install and update flow.

- **Install or update**

  ```bash
  tfgrid-compose update       # or: t update
  ```

  This installs the CLI into:

  - Binary launcher: `~/.local/bin/tfgrid-compose`
  - CLI & assets: `~/.local/share/tfgrid-compose/`

  The dashboard template lives under:

  ```text
  ~/.local/share/tfgrid-compose/dashboard/
  ```

- **Runtime dependencies**

  - Node.js **v18+**
  - npm

  If Node.js or npm are missing, `tfgrid-compose dashboard` will print a clear error with install hints.

---

## 3. Where the dashboard lives

The dashboard uses two locations on your machine:

- **Install tree (read-only template)**

  ```text
  ~/.local/share/tfgrid-compose/dashboard/
  ```

  This directory is installed by `make install` and by `tfgrid-compose update`.

- **User config tree (runtime copy)**

  ```text
  ~/.config/tfgrid-compose/dashboard/
  ```

  On first run, `tfgrid-compose dashboard` copies the template from the install tree into the config tree and runs from there. This keeps user changes isolated under `~/.config`.

You do **not** need a git checkout to run the dashboard.

---

## 4. Login & credentials

The dashboard does **not** introduce a separate browser login. It reuses the same identity and configuration that you already use with the CLI.

- Before deploying or running anything from the dashboard, run once in a terminal:

  ```bash
  tfgrid-compose login
  ```

- The dashboard then:
  - Uses the same key material and config files as `tfgrid-compose`.
  - Respects your network preferences, whitelists/blacklists, and patterns.

If you can successfully deploy from the CLI, the dashboard will be able to deploy with the same credentials.

---

## 5. Basic usage

### 4.1 Foreground (one-off sessions)

```bash
# Foreground dashboard, logs in the terminal
tfgrid-compose dashboard
# or, with shortcut
t dashboard
```

- Bootstraps `~/.config/tfgrid-compose/dashboard` on first run.
- Runs the Node server in the **foreground**.
- Default base URL:

  ```text
  http://localhost:3000
  ```

- If port **3000** is already in use, the server automatically tries `3001`, `3002`, ... up to a small limit.
- The actual bound port is logged to the terminal and to the dashboard log file.

Use this mode when you are actively working and happy to keep a terminal tab open.

### 4.2 Background service (start/stop/status)

The dashboard can also run as a small local service.

- **Start in background**

  ```bash
  tfgrid-compose dashboard start
  # or: t dashboard start
  ```

  Behavior:

  - Bootstraps and installs dependencies if needed.
  - Starts the Node server in the background.
  - Writes a PID file:

    ```text
    ~/.config/tfgrid-compose/dashboard/dashboard.pid
    ```

  - Writes the chosen port to:

    ```text
    ~/.config/tfgrid-compose/dashboard/dashboard-port
    ```

  - Logs a message with the final URL, for example:

    ```text
    Dashboard started at http://localhost:3001 (pid 12345)
    ```

- **Check status**

  ```bash
  tfgrid-compose dashboard status
  # or: t dashboard status
  ```

  Shows whether the dashboard is running and which port it is bound to, e.g.:

  ```text
  Dashboard running at http://localhost:3001 (pid 12345)
  ```

- **Stop the dashboard**

  ```bash
  tfgrid-compose dashboard stop
  # or: t dashboard stop
  ```

  - Reads the PID file.
  - Sends a graceful `SIGTERM`.
  - Cleans up the PID file.
  - Prints a confirmation, or a friendly message if it was not running.

- **View dashboard logs**

  ```bash
  tfgrid-compose dashboard logs
  # or: t dashboard logs
  ```

  Tails the last 200 lines (and follows) of:

  ```text
  ~/.config/tfgrid-compose/dashboard/dashboard.log
  ```

  This is particularly useful for debugging Node/port issues or seeing backend API errors.

---

### 4.3 Desktop launcher (Linux)

On Linux desktops, you can install a launcher icon for the dashboard:

```bash
tfgrid-compose dashboard desktop
```

This command is **idempotent** and will:

- Ensure the dashboard template is installed under `~/.config/tfgrid-compose/dashboard/`.
- Create or refresh a launcher script at `~/.local/bin/tfgrid-dashboard-launcher`.
- Create or refresh a desktop entry at `~/.local/share/applications/tfgrid-dashboard.desktop`.
- Optionally place a "TFGrid Studio Dashboard" icon on your `~/Desktop` (if it exists).

When you click the launcher icon:

- It runs `tfgrid-compose dashboard start` to ensure the backend is running.
- Opens your default browser to `http://localhost:<dashboard-port>`.

To stop the dashboard backend, use:

```bash
tfgrid-compose dashboard stop
```

The launcher can be re-installed or refreshed at any time by rerunning
`tfgrid-compose dashboard desktop`.

---

## 6. UI overview

Once the server is running, open the dashboard in your browser:

```text
http://localhost:<port from status>
```

The UI is a split-screen single-page app with two columns:

- **Left: Input (apps, commands, deployments, preferences)**

- **Right: Output (logs and shell)**

### 6.1 Left column – Input

- **Apps from Registry**
  - Lists registry apps from `~/.config/tfgrid-compose/registry/apps.yaml`.
  - One-click **Deploy** buttons for each app.

- **CLI Commands workspace**
  - A primary panel titled **CLI Commands**.
  - Shows a **Context** bar at the top:
    - "Global (tfgrid-compose)" when no deployment is selected.
    - The selected deployment ID (and app name) when a deployment is in context.
  - Commands are listed in two groups:
    - **Global CLI** – runs without deployment context.
    - **For deployment** – uses the selected deployment ID for commands like `status`, `logs`, `ssh`, `address`, `exec`.
  - Selecting a deployment in the **Deployments** table automatically updates the context and pre-fills deployment-scoped commands with the right ID.
  - When you click a command:
    - A detail form opens on the right side of the CLI Commands panel.
    - You can fill in arguments and flags.
    - A preview shows the exact CLI command (e.g. `tfgrid-compose ssh <deployment-id>`).

- **Deployments table**
  - Lists deployments from `~/.config/tfgrid-compose/deployments.yaml`.
  - Shows:
    - Deployment ID
    - App name
    - Status
    - IPs (`vm_ip`, `mycelium_ip`)
    - Contract ID
  - Per-deployment actions include:
    - **Address** → wraps `tfgrid-compose address <deployment-id>` and shows all URLs.
    - **Commands** → sets this deployment as the current **context** for CLI Commands.
    - **Connect** → opens an interactive shell (see below).
  - For `tfgrid-ai-stack` deployments, additional project actions appear (Create/Run/Publish), as described earlier.

- **Preferences panel**
  - Shows:
    - Whitelist nodes/farms
    - Blacklist nodes/farms
    - Thresholds (CPU, disk, uptime)
  - Includes buttons like **Edit in CLI Commands** that:
    - Jump you into the right CLI Commands form for `whitelist`/`blacklist`.
    - After running a preferences command, the panel auto-refreshes to reflect the new state.

### 6.2 Right column – Output

- **Job Output panel**
  - Shows live logs for any background job started from the dashboard:
    - App deploys (up)
    - Project actions (create, run, publish)
    - CLI Commands
  - Logs preserve ANSI colors where possible, similar to the terminal.
  - The subtitle shows job status and, when known, the associated deployment ID.

- **Interactive Shell panel**
  - Opened via **Connect** on a deployment row.
  - Spawns `tfgrid-compose ssh <deployment-id>` on the backend and streams output to the browser using Server-Sent Events.
  - You can type commands into the input box; each line is sent to the remote shell.
  - Closing the shell panel terminates the underlying ssh process.

---

## 7. How it works under the hood

- The dashboard backend is a small Node.js server installed with tfgrid-compose.
- It reads existing CLI state and registry files:
  - `~/.config/tfgrid-compose/registry/apps.yaml`
  - `~/.config/tfgrid-compose/deployments.yaml`
- It spawns `tfgrid-compose` commands using the same binary you use on the CLI:
  - Deploy (`up`)
  - Address lookup (`address`)
  - Project actions (`create`, `run`, `publish`)
- The UI uses simple JSON APIs on `localhost` and does not expose anything publicly.

All sensitive actions (deployment, project operations) still go through the CLI and respect your existing configuration, network preferences, and registry.
