# Useful One-liners

### System processes

```csharp
#Running processes from a system
Get-WmiObject win32_process | select processname, ProcessId, CommandLine

#Timeline of running processes
Get-WmiObject win32_process | select processname,@{NAME='CreationDate';EXPRESSION={$_.ConvertToDateTime($_.CreationDate)}},ProcessId,CommandLine |sort CreationDate -desc | format-table -auto -wrap

#Event logs
$UserDirectory = (gi env:\userprofile).value; Get-WinEvent -FilterHashtable @{Logname='security';ID=4624} | select TimeCreated,ID,Message | ft –auto -wrap | out-file $UserDirectory\desktop\Event-4624.txt
```

### USB devices

```csharp
#Collect a list of USB devices connected to the machine
Get-ItemProperty -ea 0 hklm:\system\currentcontrolset\enum\usbstor\*\* | select FriendlyName, PSChildName	

#Identify the first connection date of these devices
Get-ItemProperty -ea 0 hklm:\SYSTEM\CurrentControlSet\Enum\USBSTOR\*\* | select PSChildName | foreach-object {$P = $_.PSChildName ; Get-Content C:\Windows\inf\setupapi.dev.log | select-string $P -SimpleMatch -context 1 } 

#Identify the last connection date for these devices
Get-ItemProperty -ea 0 hklm:\SYSTEM\CurrentControlSet\Enum\USBSTOR\*\* | select PSChildName | foreach-object {$P = $_.PSChildName ;Get-WinEvent -LogName Microsoft-Windows-DriverFrameworks-UserMode/Operational | where {$_.message -match "$P"} | select TimeCreated, message | sort TimeCreated -desc | ft -auto -wrap}

#Identify the drive letters that were assigned to each of the USB devices
Get-ItemProperty -path hklm:\system\currentcontrolset\enum\usbstor\*\* | ForEach-Object {$P= $_.PSChildName; Get-ItemProperty hklm:\SOFTWARE\Microsoft\"Windows Portable Devices"\*\* |where {$_.PSChildName -like "*$P*"} | select PSChildName,FriendlyName } | ft -auto

#Identify if any link file refers to the drive letter that the USB device used
gwmi -ea 0 Win32_ShortcutFile | where {$_.FileName –like “*[string]*”} | select FileName, caption, @{Name='CreationDate'; EXPRESSION={$_.ConvertToDateTime($_.CreationDate)}},@{Name=’LastAccessed’;EXPRESSION={$_.ConvertToDateTime($_.LastAccessed)}},@{Name=’LastModified’;EXPRESSION={$_.ConvertToDateTime($_.LastModified)}},Target | sort LastModified -Descending
```

