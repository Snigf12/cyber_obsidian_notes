Go back to [[Index]]
Go to [[Chapter 2 - The Windows Kernel]]

# Basic concepts:
In PowerShell, the terms **functions**, **methods**, **cmdlets**, and **modules** refer to different components used to create, organize, and execute scripts and commands. Here's a brief explanation of each:

#### **Functions**
A **function** is a block of reusable code defined by the user to perform a specific task.

- **Syntax:** `function FunctionName { <script block> }`
- Example:
    ```powershell
    function Greet {
        param([string]$Name)
        "Hello, $Name!"
    }
    Greet -Name "Alex"
    ```

#### **Methods**
A **method** is a function that belongs to a .NET object and is used to perform operations on that object.

- Accessed using the `.` notation.
- Example:
    ```powershell
    $date = Get-Date
    $date.AddDays(5)  # Adds 5 days to the current date
    ```
#### **Cmdlets**

A **cmdlet** is a built-in PowerShell command designed to perform specific tasks.

- Always follow the **Verb-Noun** naming convention.
- Example:
    ```powershell
    Get-Process  # Lists all running processes
    Set-Content -Path "file.txt" -Value "Hello, World!"  # Writes to a file
    ```

#### **Modules**

A **module** is a collection of related cmdlets, functions, variables, and resources organized into a single package.

- Example modules: `Microsoft.PowerShell.Management`, `ActiveDirectory`.
- Load a module using:
    ```powershell
    Import-Module ActiveDirectory
    ```

These components allow PowerShell to be flexible and extensible for automation tasks and scripting.
# Install [[NtObjectManager]] module

The [[Script execution policy]] in PowerShell 5.1 is by default set as restricted. To change this policy to accept unsigned scripts, and being able to use the [[PowerShell Module]] [NtObjectManager](https://www.powershellgallery.com/packages/NtObjectManager/2.0.1), which has a lot of tools (cmdlets) to monitor and interact with Windows [[NT Objects]].

To set the policy to RemoteSigned mode, we use the following cmdlet:

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
```

Then, ensuring we have the [Script execution policy] set up, we proceed installing the [[PowerShell Module]] we need to interact more easily, we can execute the following commands to install the module, and to update it to the last version:

```powershell
#Installing the module:
Install-Module NtObjectManager -Scope CurrentUser -Force

#Updating it to the last version:
Update-Module NtObjectManager
```

Now, to import the module to load it to our environment, we run:
```powershell
Import-Module NtObjectManager
```

> [!tip] If you get any errors and asked to install Nuget.
> You will need to do it running PowerShell as admin to have enough permissions to do it. In that case you must run:
> 
> ```Install-PackageProvider -Name NuGet -Force```

# How PowerShell variables work (Types and Examples)

| Type      | .NET type               | Examples           |
| --------- | ----------------------- | ------------------ |
| int       | System.Int32            | 142, 0x8E, 0216    |
| long      | System.Int64            | 142L, 0x8EL, 0216L |
| string    | System.String           | "Hello", "World!"  |
| double    | System.Double           | 1.0, 1e10          |
| bool      | System.Boolean          | $true, $false      |
| array     | System.Object[]         | @(1, "ABC", $true) |
| hashtable | System.Object.Hashtable | @{A=1; B="ABC"}    |

# Construct almost any .NET type variables

We can create [[Globally Unique Identifier GUID]] variables or almost any type of .NET variables, by specifying the variable type inside square brackets:

```powershell
[System.Guid]"6c03a3a17-4459-4339-a3b6-1ack1f3e8921"
```

We can also use methods, by using :: like to create a new [[Globally Unique Identifier GUID]]:
```powershell
[System.Guid]::new("6c03a3a17-4459-4339-a3b6-1ack1f3e8921")
```

Or to create a random [[Globally Unique Identifier GUID]]:
```powershell
[System.Guid]::NewGuid
```

# Discovering commands and getting help

To find commands that matches certain words, you can use the Get-Command cmdlet, and replace the keyword with what you're looking for:
```powershell
Get-Command -Name *keyword*
```

For example If I search for the word security I get a list of functions and cmdlets:
```powershell
Get-Command -Name *security*
```
Result:
```text
CommandType     Name                                   Version    Source
-----------     ----                                   -------    ------
Function        Add-NtSecurityDescriptorControl       2.0.1     NtObjectManager
Function        Add-RpcClientSecurityContext          2.0.1      NtObjectManager
Function        Clear-NtSecurityDescriptorDacl        2.0.1      NtObjectManager
.
.
.
Cmdlet          Add-NtSecurityDescriptorAce           2.0.1      NtObjectManager
Cmdlet          Add-NtTokenSecurityAttribute          2.0.1      NtObjectManager
Cmdlet          Compare-NtSecurityDescriptor          2.0.1      NtObjectManager
.
.
.
Application     SecurityHealthHost.exe                10.0.26... C:\WINDOWS\system32\SecurityHealthHost.exe
Application     SecurityHealthService.exe             10.0.26... C:\WINDOWS\system32\SecurityHealthService.exe
```

To enumerate commands from one of those functions, cmdlets or applications we can use the Get-Command -Module *keyword*. In this case, the Source of the previous result is the module we need to use:

For example, using:
```powershell
Get-Command -Module NtObjectManager
```

Will list all commands (cmdlets, functions and alias) that the module has, and to get the commands filtering by the name of the function or cmdlet, we can use the -Name at the end, like this command that will list all commands that start with Set:
```powershell
Get-Command -Module NtObjectManager -Name Set*
```

To see how to use the commands, we can use the Get-Help command, which will display the different flags (parameters) and ways to use those commands. Like for example:
```powershell
Get-Help Set-NtFileEa
```

The commands usually have parameters, if we want to check what they do and how they work, we can add a second parameter -Parameter to the previous command as follows:
```powershell
Get-Help Set-NtFileEa -Parameter Win32Path
```

We can see a nice window with the information if we use the -ShowWindow parameter:
```powershell
Get-Help Set-NtFileEa -ShowWindow
```

# PowerShell Functions

To define a function, example:
```powershell
function GetNameValue {
	param(
		[string]$Name = "",
		$Value
	)
	return "We've got $Name with value $Value"
}
```

An example calling this function would be:
```powershell
Get-NameValue -Name "Hello" -Value "World"
```

Or another way would be:
```powershell
Get-NameValue "Goodbye" 12345
```

A function or script can be also be defined as a variable, like for example:
```powershell
$script = {Write-Output "Hello"}
```

And to invoke that script, we need to use & to run that script as shown:
```powershell
& $script
```

That will execute the script saved in $script

# Displaying / Manipulating objects

Objects allow you to work with data in a more meaningful way than plain text.
### Key Features of Objects in PowerShell:

1. **Properties**:
    - Attributes that describe the object.
    - Example: A file object has properties like `Name`, `Length`, `CreationTime`.
2. **Methods**:
    - Actions you can perform on the object.
    - Example: A string object has methods like `.ToUpper()` or `.Replace()`.
### Example:
When you run a cmdlet, it often outputs objects.
```powershell
$file = Get-Item "C:\example.txt"
$file.Name        # Property: File name
$file.Length      # Property: File size
$file.Delete()    # Method: Deletes the file
```

### Why Objects are Powerful:
- They allow you to manipulate and access specific parts of data without needing to parse text manually.
- You can pipe objects between cmdlets for processing.

Example of Piping Objects:

```powershell
Get-Process | Where-Object {$_.CPU -gt 100} | Select-Object Name, CPU
```

Here, processes with `CPU` greater than 100 are filtered and specific properties (`Name`, `CPU`) are selected. Each step works with objects, not plain text.

To get an object returned by a Cmdlet we can use as an example Get-Process, which returns the list of processes running in the system:
```powershell
Get-Process
```

To list only two properties, we can use the Select-Object command, like this:
```powershell
Get-Process | Select-Object Id, ProcessName
```

We can display the objects also using Format-List to display the result as a list or Format-Table to display it as a table, which is the default:
```powershell
Get-Process | Format-List
Get-Process | Format-Table
```

There is a Cmdlet Get-Member, that lists the properties, events, methods, and other MemberTypes of an object. For example, using
```powershell
Get-Process | Get-Member -Type Property
```
Will list all the properties that can be used with Get-Process objects. To get all MemberTypes we can remove the -Type Property of the previous command. This way we can see methods, which are functions that can modify the object itself.

There's a Cmdlet called Out-Host, that can manage how to display Objects and using the parameter -Paging, that can help to show not all the results at once, but by groups, like for example:
```powershell
Get-Process | Out-Host -Paging
```
This will list all processes until the screen is full, and to scroll to see more, we can press Ener to see the next line or space to display another page until the screen its full again.

To display an object in a nice way, there's also a Cmdlet Out-GridView that will open a window with the result, with a more GUI friendly way:
```powershell
Get-Process | Out-GridView
```

We can also use filters using the Cmdlet Where-Object. for example, to list only the explorer object, we can use either of this commands:
```powershell
Get-Process | Where-Object ProcessName -eq "explorer"
Get-Process | Where-Object {$_.ProcessName -eq "explorer"}
```

Or we can sort also this result by the Handles column or the CPU(s) column by using:
```powershell
Get-Process | Sort-Object Handles
Get-Process | Sort-Object CPU
```

We can also specify if it's ascending (default) or descending order by using the parameter -Descending:
```powershell
Get-Process | Sort-Object Handles -Descending
Get-Process | Sort-Object CPU
```

To export data into a file, we can use the Out-File Cmdliet and print it using Get-Content:
```powershell
Get-Process | Out-File processes.txt
Get-Content process.txt
```

We can simplify the Out-File command using > instead like:
```powershell
Get-Process > processes.txt
Get-Content process.txt
```

We can also export a csv file using the Export-Csv Cmdlet and print it using Get-Content:
We can simplify the Out-File command using > instead like:
```powershell
Get-Process | Select-Object Id, ProcessName | Export-Csv processes.csv -NoTypeInformation
Get-Content process.csv
```

Same thing to export it as xml file using Export-CliXml:
```powershell
Get-Process | Select-Object Id, ProcessName | Export-CliXml processes.xml
Get-Content process.xml
```

Go to [[Chapter 2 - The Windows Kernel]]