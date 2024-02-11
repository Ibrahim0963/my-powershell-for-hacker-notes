# my-powershell-for-hacker-notes


-> firstly install windows 10 development environment virtual machine
-> we will work with windows terminal
-> there is versions in powershell >`$PSVersionTable`
>`>$Host`
>`>$PSVersionTable.PSVersion` -> show current PS version
>`>powershell.exe -Version 2.0` -> change PS version to 2.0

## cmdlit
>`tasklist` -> show all Process on PC

>`ps` == `Get-Process` -> show all the process (another way)

>`Get-ChildItem` == `dir` == `ls`

>`Get-Help Get-ChildItem` 

>`Get-Help Get-ChildItem -Online`

>`Get-ChildItem -Path 'C:\Users' -Filter *win*`

>`Get-ChildItem -Path 'C:\' -Attribute h` -> get hidden files

>`Get-Help *process*` -> get cmdlit(PS commands) that has the word 
"process"

>`Get-Alias`

>`Get-Help *Alias*`

>`Get-Help -Full Get-Command | more`

>`Get-Command -Type cmdlet -Name *Process`

>`ls | Measure-Object` in PS == `ls | wc -l` in bash

> `Get-Process -Name *explorer*` == `ps -Name *explore*` -> get all processes that belong to explorer

>`Start-Process notepad`

>`Stop-Process notepad` -> close notepad, if we have more the one window notepad then we use the command with Id to close the window/app 

>`Stop-Process notepad -Id Process_Id` -> close specific notepad window

>`(Get-Process -Name notepad)[1]` -> get the second process

>`Get-Command -Type Cmdlet *format*`

>`Get-ChildItem | Format-List -Property Mode` -> show result as a list

>`Get-Command -Type Cmdlet *out*` -> to list commands, that has the word out

>`Get-Process | Out-GridView` -> to show results in a view GUI

> `Get-Process | Out-File c:\Users\ibrahim\` -> save the results in a file
> `Get-Process | Format-Table -Property Name, id | Out-File c:\Users\process.txt`

>`echo "My Text" > info.txt` == `Write-Output "My Text" > info.txt`


## Veriables
```
> $Greeting = "Welcome to my PS!"
> $Greeting
> $Greeting.Length 
> $Greeting.GetType()
> $Number = 1.2
> $Number.GetType() // output: Double
> [int]$Number -> change Number type to integer
> Remove-Variable Number == > Remove-Variable -Name Number
> $Names = "Ibrahim", "Alex", "Andra" -> array
Note: Array can hold many values with different data type + Array in PS is like array in other langauges in how to deal with it
```

## Operators
`+ - * / and so on`

```
> $Name = "Ibrahim"
> $Age = 20
> $info = $Name + " " + [String]$Age // Output: Name 20
```
Note: to bind(+) 2 variable, the 2 variable have to have the same data type 
-> Boolean: -eq(equal to), -le(less then or equal), -ne(not equal to), -gt(greater then), -like, -notlike, -match, -contains, -notcontains, -in, -notin
>`"Ibrahim" -like "Alex"` // output: False 

>`"I am going" -match "am"` // output: True

>`"I am using Python" -replace "Python", "Powershell"`

>`"Powershell" in "I am using Powershell"` // Output: False
We got in the privious command false, because in works only in array

>`"Powershell" in ("PHP","C#","Powershell")` // Output: True

>`(1 -like 1) -and (2 -gt 1)`

>`(1 -like 1) -or (2 -gt 1)`

>`"welcome to my powershell" -split " "`

>`("https://facebook.com" -split "/")[-1]` // output: facebook.com

>`("https://facebook.com" -split "/",3)` -> make only 3 elements

>`"www","example","com" -join "."` // output: www.example.com

>`2 -is "int"` -> to check if 2 is an integer. Output: True

>`"2" -isnot "int"` ->  to check if 2 is not an integer. Ouput: True

> `0x00 -as "int"` -> print 0x00 as an integer

## Statement
>`>if (1 -gt 1) {"Yes"} elseif (1 -eq 1) {"Same"} else {"No"}`
>`>if (((Get-Process).count) -gt 200) {"Too Many Processes Running.."} else {"Ok"}`

>`>switch (1) {1 {"One"} 2 {"Two"}}`
>`>switch -Wildcard ("abc") {a* {"A"} *b* {"B"} c* {"c"} default {"no value found.."}}`
>`>switch -Regex -File c:\Users\test.txt {"SearchedWord"{$_}}`


## functions
>`function add {1 + 2}` -> create function called add
>`add` -> calling the function "add"

>`function SayHelloSimp {"Hello, "+ $args}` 
>`function SayHello {"Hello, "+ $args[0] + $args[1]}` 
>`SayHello "Ibrahim" "Alakeel"`

>`function add($n1, $n2){$n1 + $n2}` 
-> `add 2 3` == `add -n1 2 -n2 3` == `add -n2 2 -n1 3`

>`function add($name, $args){"Name :"+$name+ " " +$args}` 
-> args here is dynamic, so user can enter as many parameters as he like

>`function add([String]$name="ibrahim", [int]$age){$name+" "+$age}` -> specify the data type of a parameter + adding a default value to it

```
function SubOrSum($n1,$n2,[switch]$sub){
	if($sub){$n1 - $n2}
	else{$n1 + $n2}
}
SubOrSum 3 5 // output: 7
SubOrSum 3 5 -sub // output 2
```

>`ls function:` -> to show all declared function 

```
function myfunc{
	param(
	[parameter (Mandatory = $true)] // parameter must have a value
	$a,
	[parameter (Position = 0)] // making first args value to $b
	$b
	[parameter (ValueFromPipeline = $true)] //> 5 | myfunc 1 2
	[AllowNull ()]
	[ValidateSets(1,4,6)] // make sure paramters value 1,4 or 6
	$c
	)
}
```

```
function Remove-File{
	[CmdletBinding()] // add cmdlet features in my cmd
	param(
		[Parameter()]
		$Global:$FilePath
	)
	Write-Verbose "Deleting the File $FilePath" // contents of -v 
	Remove-Item $FilePath
}
> ./script.ps1
>. ./script.ps1 -> to load the function to the PS
>Remove-File -FilePath c:\Users -Verbose 
>Remove-File -FilePath c:\Users -Whatif // will show a msg about the command, so to change the message we can add this
`
if($PSCmdlet.ShouldProcess("$FilePath","Deleting Permanetly"))
{Remove-Item $FilePath}
`
```

## Scripting-Execution
we will use the ISE PS or Studio code
Firstly what is ExecutionPolicy ? 
by default you can not execute ps scripts from the ps terminal.
executionPolicy -> restricted, unrestricted, undefined
>`powershell -exec bypass` -> to enable running unknow scripts
>`Get-ExecutionPolicy`
>`Get-ExecutionPolicy -Scope CurrentUser`

## Modules
>`Get-Help *module*`
>`Get-Module` -> show all installed modules
> difference .psm1 vs. psd1 ? -> google
powerpreter is a ps script for helping in post explotation
>`Invoke-WebRequest -Uri myurl -OutFile powerpreter.psm1` -> to download a script with ps 
>`Import-Module .\powerpreter.psm1`
>`Get-Command -Module powerpreter` -> to show all functions in module
>`Remove-Module powerpreter.psm1`
>`env:PSModulePath` -> to see all modules, that load automatically
Note, you can go to path, where modules are loaded automatically, then create a folder contains all your scripts or specific script to make it easier for you, so you can import module directly >`Import-Module YourModul`


```
function Test-FileExists{
	Test-Path $FilePath
}
function DontInteractWithMe{
	Write-Output "I am not Here"
}
Export-ModuleMember -Function *-*
```
-> Interact only with functions that has - in it

>`New-ModuleManifest MyScriptModule.psd1`

## Remote PS
>`$credentials = Get-Credential` -> save specific credentials in a variable

>`Test-WSMan machine_name.domainName.com -Credential $credentials -Authentication Negotiate`

>`Test-NetConnection -Computer machine_name.domainName.com -Port 123`

>`Enter-PSSession -ComputerName machine_name.domainName.com -Credential $credentials -Authentication Negotiate` -> enter another ps session on another device

>`$session = New-PSSession -ComputerName machine_name.domainName.com -Credential $credentials -Authentication Negotiate`
-> we make a connection to that machine and save it in a variable

>`Invoke-Command -Session $session -ScriptBlock {(Get-CimInstance Win32_ComputerSystem).NumberOfLogicalProcessors}`

## Jobs
>`Get-Help *job*` -> to get all commands contains the word job

>`Start-Job -ScriptBlock {whoami}`
>`Start-Job -FilePath myScript.ps1`

>`Get-Job`

>`Get-Job | Recieve-Job`

Note: For more details look youtube videos

# Net in PS
