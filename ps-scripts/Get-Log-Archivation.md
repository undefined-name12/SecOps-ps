# Get-Log-Archivation PowerShell Script

## Overview
The **`Get-Log-Archivation`** function automates the archiving of old log files and maintains folder hygiene by compressing and removing outdated files. It is designed to ensure efficient log file management in IT infrastructure.
Besides material written in the description below, you can also read my article that i published on linked in which I review this problem in slightly different angle [https://www.linkedin.com/pulse/mitigating-security-risks-change-requests-maks-zaikin-lh6xe/?trackingId=JsuYjAPi4B7pWuH6T3oVNw%3D%3D](Mitigating Security Risks in IT Change Requests)

---

## Features
- Archives old log files based on their age.
- Deletes outdated archive files (`*.zip`).
- Maintains a detailed log of performed actions in a specified log file.
- User-configurable parameters for flexibility.

---

## Parameters
| Parameter       | Description                                                                 | Mandatory |
|-----------------|-----------------------------------------------------------------------------|-----------|
| `OldFilesAge`   | Filters log files for archiving based on the specified age (in days).       | Yes       |
| `OldCabsAge`    | Removes archive files (`*.zip`) older than the specified age (in days).     | Yes       |
| `LogFilePath`   | Specifies the path to the log file for saving all actions and statuses.     | Yes       |

---

## Examples

### Example 1: Archive Logs and Remove Old Archives
```powershell
Get-Log-Archivation -OldFilesAge 7 -OldCabsAge 30 -LogFilePath 'C:\Temp\log.log'
```

- Archives log files older than 7 days.
- Deletes archive files (`*.zip`) older than 30 days.
- Logs all actions to the file `C:\Temp\log.log`.

### Example 2: A Immediate Archiving and Deletion
```powershell
Get-Log-Archivation -OldFilesAge 0 -OldCabsAge 0 -LogFilePath 'C:\Temp\log.log'
```

- Archives all log files regardless of age.
- Deletes all archive files (*.zip) regardless of age.
- Logs all actions to C:\Temp\log.log.

### How It Works

1. Initialization:
  - The script initializes with a set of predefined paths for monitoring.
  - It writes a start message to the specified log file.

2. Archiving Process:
  - Identifies log files older than OldFilesAge and compresses them into .zip archives.
  - Removes the original log files after successful archiving.

3. Cleanup:
  - Deletes archive files (*.zip) older than OldCabsAge.

4. Error Handling:
  - Catches and logs any errors that occur during the process.

5. Completion:
  - Writes an end message to the log file upon successful execution.

### ⚠️ IMPORTANT: SECURITY RISK MITIGATION

1. Use a Regular User Account:
   - If you plan to schedule this script using a task scheduler, ensure it runs under a non-privileged, regular user account. Avoid using administrative or highly privileged accounts.

2. Set Proper Permissions:
   - Grant the regular user account read/write access to the directories containing logs and the location where the script will store its action logs.
   - Restrict access to these folders for all other users to minimize exposure.

3. Disable Interactive Logon:
   - For the user account under which this script will run, ensure interactive logon is disabled. This prevents the account from being used to log into systems directly, reducing the risk of unauthorized access.

### Configure audit policy for control access to the script file

1. Enable Object Access Auditing
```powershell
auditpol /set /subcategory:"File System" /success:enable /failure:enable
```

2. Check config
```powershell
auditpol /get /category:"Object Access"
```

3. Enable audit for the script file
```powershell
# Path to the script file
$filePath = "C:\Path\To\Get-Log-Archivation.ps1"

# Set auditing rules for Everyone (Success and Modify Access)
icacls $filePath /setaudit Everyone:(OI)(CI)(M) /t /c
```

4. Check Audit event logs for the messages

- Event ID: 4663 (File Access)

```powershell
# Define variables
$LogName = "Security"
$EventID = 4663
$FilePath = "C:\Path\To\Get-Log-Archivation.ps1"

# Filter and display events
Get-WinEvent -LogName $LogName | Where-Object {
    $_.Id -eq $EventID -and $_.Message -match $FilePath
} | Select-Object TimeCreated, Id, Message

# Export to CSV
Get-WinEvent -LogName $LogName | Where-Object {
    $_.Id -eq $EventID -and $_.Message -match $FilePath
} | Select-Object TimeCreated, Id, Message | Export-Csv -Path "C:\Temp\FilteredEvents.csv" -NoTypeInformation

# Export to TXT
Get-WinEvent -LogName $LogName | Where-Object {
    $_.Id -eq $EventID -and $_.Message -match $FilePath
} | Select-Object TimeCreated, Id, Message | Out-File -FilePath "C:\Temp\FilteredEvents.txt"

```

### ⚠️ Disclaimer

 All scripts are provided as-is with no implicit warranty or support.

- Always test scripts in a DEV/TEST environment before using them in production.
- Use at your own risk!