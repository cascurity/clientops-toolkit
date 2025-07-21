# Winget Upgrade All

This project uses [PSAppDeployToolkit (PSADT)](https://psappdeploytoolkit.com) to automate the upgrade of all installed software on Windows using [winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/).

## Features

- Runs `winget upgrade --all` with all necessary agreements accepted.
- User-friendly progress and completion dialogs via PSADT.
- Error handling and logging.
- Optional Discord webhook notification on completion or error.
- Supports interactive, silent, and non-interactive deployment modes.
- Designed for use with software deployment tools (e.g., SCCM, Intune).

## Usage

### Prerequisites

- Windows 10/11 with [winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/) installed.
- PowerShell 5.1 or later.
- Administrator rights (recommended for full functionality).

### Running the Script

You can run the script directly with PowerShell:

```powershell
powershell.exe -File "Invoke-AppDeployToolkit.ps1"
```

Or with additional parameters:

```powershell
powershell.exe -File "Invoke-AppDeployToolkit.ps1" -DeploymentType Install -WebhookId "<webhook-id>" -WebhookToken "<webhook-token>"
```

#### Parameters

- `-WebhookId` / `-WebhookToken` – (Optional) Discord webhook credentials for notifications.

### What It Does

1. Shows a welcome and progress dialog (unless in silent mode).
2. Runs:
   ```
   winget upgrade --all --nowarn --accept-source-agreements --accept-package-agreements
   ```
3. Notifies the user of success or failure.
4. Optionally sends a notification to a Discord webhook.

## Customization

- Edit `Invoke-AppDeployToolkit.ps1` to change dialogs, messages, or add pre/post-install actions.
- Configure additional PSADT options in the `Config/` and `Strings/` folders.

## Troubleshooting

- Check the PSADT log files for detailed error information.
- Ensure `winget` is installed and available in the system PATH.
- Run PowerShell as Administrator for best results.

## Scheduling Automatic Upgrades with ServiceUI.exe

To automatically and regularly upgrade all software for logged-in users, you can create a Windows Scheduled Task that runs your PSADT script with `ServiceUI.exe`. This allows interactive dialogs to appear on the user's desktop even when the task runs as SYSTEM.

## License

This project is licensed under the GNU LGPLv3. See [COPYING.Lesser](COPYING.Lesser) for details.

---

**Credits:**  
Based on [PSAppDeployToolkit](https://psappdeploytoolkit.com) and