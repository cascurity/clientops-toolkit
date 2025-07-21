# PSAppDeployToolkit: Reboot If Needed

This project provides a PowerShell deployment script using [PSAppDeployToolkit (PSADT)](https://psappdeploytoolkit.com) that checks for a pending system reboot and initiates a user-friendly reboot if required. It is designed for use in enterprise environments and can be scheduled as a Windows Scheduled Task, optionally using `serviceui.exe` to display prompts in the user session.

## Features

- **Detects Pending Reboot:** Uses PSADT functions to check if a system reboot is required.
- **User Notification:** Displays a prompt to the user, warning them that a reboot is needed.
- **Graceful Reboot Countdown:** Shows a restart prompt with a countdown (default: 5 minutes).
- **Discord Webhook Integration:** Optionally sends a notification to a Discord channel when a reboot is initiated.
- **Robust Logging:** Leverages PSADT's logging and error handling.
- **Can Be Scheduled:** Designed to be run as a scheduled task, including support for running in the user's session with `serviceui.exe`.

## Prerequisites

- Windows 10/11
- [PowerShell 5.1+](https://docs.microsoft.com/en-us/powershell/)
- [PSAppDeployToolkit](https://psappdeploytoolkit.com) (included in this repo or referenced)
- (Optional) [serviceui.exe](https://docs.microsoft.com/en-us/mem/configmgr/apps/deploy-use/serviceui-exe-technical-reference) for displaying prompts in the user's session when running as SYSTEM
- (Optional) Discord webhook URL for notifications

## Usage

### Manual Execution

Run as administrator:

```powershell
powershell.exe -File "Invoke-AppDeployToolkit.ps1" -DeployMode Silent
```

### Scheduled Task with User Prompts

To ensure prompts appear in the user's session when running as SYSTEM, use `serviceui.exe`:

```cmd
serviceui.exe -process:explorer.exe powershell.exe -ExecutionPolicy Bypass -File "C:\Path\To\Invoke-AppDeployToolkit.ps1" -DeployMode Silent
```

### With Discord Webhook

Add the `-WebhookId` and `-WebhookToken` parameters:

```powershell
powershell.exe -File "Invoke-AppDeployToolkit.ps1" -WebhookId "<your_id>" -WebhookToken "<your_token>"
```

## Parameters

- `-WebhookId` and `-WebhookToken`  
  (Optional) Send a Discord notification when a reboot is triggered.

## How It Works

1. **Checks for Pending Reboot:** Uses PSADT's `Get-ADTPendingReboot` function.
2. **User Prompt:** If a reboot is needed, displays a prompt and a 5-minute countdown before reboot.
3. **Discord Notification:** If webhook parameters are provided, sends a notification to Discord.
4. **Logging:** All actions are logged using PSADT's logging system.

## Customization

- Adjust the countdown timer in the `Show-ADTInstallationRestartPrompt` call inside `Install-ADTDeployment`.
- Modify the Discord message or webhook logic as needed.

## License

This script and PSAppDeployToolkit are licensed under the GNU LGPLv3. See [LICENSE](https://www.gnu.org/licenses/lgpl-3.0.html) for details.

---

**For more information, visit [psappdeploytoolkit.com](https://psappdeploytoolkit.com)