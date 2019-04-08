# Striker
Cobalt Strike aggressor scripts. 
```
wmi.cna - Scripts that use WMI to validate credentials and move laterally. 
    wmiclass                  [*] uses wmi and powershell to run a wmi class
    wmicomputer               [*] uses wmi and powerpick to get computer info
    wmils                     [*] uses wmi and powerpick to list files
    wmips                     [*] uses wmi and powershell to list processes
    wmishare                  [*] uses wmi and powerpick to list shares on a system
    wmiuptime                 [*] uses wmi and powershell to show uptime
misc.cna - Miscellaneous CNA scripts.
    massbpstop                [*] used to stop browser pivot for all beacons
    masssocksstop             [*] used to stop socks proxy for all beacons
    qping                     [*] send one ping packet with powershell
    stage                     [*] used to stage a beacon
```
# wmi.cna
WMI scripts used to validate credentials and to move laterally. The strengh of these scripts is that they allow you to use found credentials in order to execute WMI commands on remote systems. 
## wmiclass
```
Synopsis: wmiclass [class] [target] [DOMAIN\user] [password]

Uses PowerPick to run Get-WMIObject with generic class. If creds are supplied, it will run as the user supplied.

[target] IP address of hostname of the target system.
```
## wmicomputer
```
Synopsis: wmicomputer [target] [DOMAIN\user] [password]

Uses PowerPick to run Invoke-WMI and list processes on a remote system. If creds are supplied, it will run as the user supplied.

[target] IP address of hostname of the target system.
```
### Example
```
beacon> wmicomputer
beacon> powerpick Get-WmiObject -Class win32_operatingsystem -ComputerName "." | fl
[*] Tasked beacon to run: Get-WmiObject -Class win32_operatingsystem -ComputerName "." | fl (unmanaged)
[+] host called home, sent: 125001 bytes
[+] received output:


SystemDirectory : C:\Windows\system32
Organization    : Microsoft
BuildNumber     : 7601
RegisteredUser  : 
SerialNumber    : 00392-972-8000024-85020
Version         : 6.1.7601

```
## wmils
```
Synopsis: wmils [target] [path] [DOMAIN\user] [password]

Uses PowerPick to run Invoke-WMI and list processes on a remote system. If creds are supplied, it will run as the user supplied.

[target] IP address of hostname of the target system.
```
## wmips
```
Synopsis: wmips [target] [DOMAIN\user] [password]

Uses PowerPick to run Get-WMIObject Win32_Process to list processes on a remote system. If creds are supplied, it will run as the user supplied.

[target] IP address of hostname of the target system.
```
## wmishare
```
Synopsis: wmishare [target] [DOMAIN\user] [password]

Uses PowerPick to run Get-WMIObject Win32_Share to list shares on a system. If creds are supplied, it will run as the user supplied.

[target] IP address of hostname of the target system.
```
## wmiuptime
```
Synopsis: wmiuptime [target] [DOMAIN\user] [password]

Uses PowerPick to run Get-WMIObject Win32_OperatingSystem to show uptime. If creds are supplied, it will run as the user supplied.

[target] IP address of hostname of the target system.
```
### Example
```
beacon> wmiuptime
beacon> powerpick Get-WmiObject Win32_OperatingSystem -computername "."| select csname, @{LABEL='LastBootUpTime' ;EXPRESSION={$_.ConverttoDateTime($_.lastbootuptime)}}
[*] Tasked beacon to run: Get-WmiObject Win32_OperatingSystem -computername "."| select csname, @{LABEL='LastBootUpTime' ;EXPRESSION={$_.ConverttoDateTime($_.lastbootuptime)}} (unmanaged)
[+] host called home, sent: 125001 bytes
[+] received output:

csname                                            LastBootUpTime                                   
------                                            --------------                                   
IEWIN7                                            4/8/2019 1:26:44 AM                              
```
# misc.cna
## massbpstop
```
Synopsis: massbpstop

Used to stop the browser pivot proxy for all beacons.
```
## masssocksstop
```
Synopsis: masssocksstop

Used to stop the socks proxy for all beacons.
```
## qping
```
Synopsis: qping [target]

Send one ping packet with the PowerShell (powerpick) command: Test-Connection -Count 1 -Computer [target]
```
## stage
```
Synopsis: stage [target] [listener] [x86|x64] (x86 by default)

Used to stage a beacon. Using make_token beforehand may be required.

[target] is an IP address or hostname
```
