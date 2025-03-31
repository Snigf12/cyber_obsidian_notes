Go to [[1_Index]]
Previous chapter [[Chapter 1 - Setting up a PowerShell Testing Environment]]

The [[Windows NTOS kernel executive]] or kernel for short, it is split into multiple subsystems and it connects the user applications with the hardware. Each subsystem exposes [[API]]s for other subsystems to call. To know what subsystem each API belongs to it can be identified by its prefix like shown in the table below:

| Prefix   | Subsystem                  | Example                   |
| -------- | -------------------------- | ------------------------- |
| Nt or Zw | System call interface      | NtOpenFile/ZwOpenFile     |
| Se       | Security Reference Monitor | SeAccessCheck             |
| Ob       | Object Manager             | ObReferenceObjectByHandle |
| Ps       | Process and thread manager | PsGetCurrentProcess       |
| Cm       | Configuration manager      | CmRegisterCallback        |
| Mm       | Memory manager             | MmMapIoSpace              |
| Io       | Input/Output Manager       | IoCreateFile              |
| Ci       | Code integrity             | CiValidateFileObject      |
## The security Reference Monitor

The [[Security Reference Monitor (SRM)]] is the most important subsystem in the kernel. It implements the security mechanisms that restrict which users can access what.

Each process running on the system has an [[Access Token]] when it is created and is managed by the SRM and it defines the identity of the user associated with that process. The SRM can also perform an operation called an [[Access Check]]. This operation queries a resource's security descriptor, compares it to the process's *access token*, and either calculates the level of granted access or indicates that access is denied to the caller.

The SRM also generates audit events whenever a user accesses a resource. Auditing is disabled by default due to the volume of events it can produce. This can be used to identify malicious behaviour or diagnose security misconfigurations.

The SRM expects users and groups to be represented as binary structures called [[Security Identifier (SID)]]. However, for users this is not very convenient, as they use meaningful names, so they need to be converted to SIDs before the SRM can use them. This translations are done by the [[Local Security Authority Subsystem (LSASS)]], which runs inside a privileged process independent from any logged-in users.

It is not possible to represent every possible SID as a name, so Microsoft defined the [[Security Descriptor Definition Language (SDDL)]] format to represent a SID as a string. SDDL can represent an entire [[Security Descriptor]] of a resource, but for now it will be use to show the SID which is a security descriptor itself.

To get the SID of a group or user, we can use the following PS command:
```PowerShell
Get-NtSid -Name "group/user"
```

For example, if we use "Users" to get the SID of that group, we get the SID S-1-5-32-545 which is the string in SDDL format.

This SID can be broken into:
* S character prefix indicates that the string is an SID in SDDL format.
* The version of the SID structure in decimal. Fixed value of 1.
* The security authority. Authority 5 indicates it is a built-in NT authority.
* Two relative identifiers (RIDs), in decimal. The RIDs 32 and 545 represent the NT authority group.

The command to retrieve the name from an SID, would be:
```PowerShell
Get-NtSid -Sdd1 "SID"
```

In this case, if we use "S-1-5-32-545" instead of "SID", we will obtain the same result as using the name, getting the pair:
```
Name          Sid
----          ---
BUILTIN\Users S-1-5-32-545
```

This is an SID that is common in all Windows endpoints.

## The Object Manager

On windows everything is an object. Every file, process, and thread is represented as an object structure in kernel memory. Each of the objects have assigned [[Security Descriptor]], restricting which user can access the object and the type of access (read, write, execute, etc).

The Object Manager is the component of the kernel responsible for managing the memory allocation and lifetimes of the objects. The kernel objects can be opened through a naming convention using system call and there are ways to use a [[Handle]] returned by the system call to access the object.

### Object types

The kernel mantains a list of all the types of objects it supports. Each object type has different supported operations and security properties. The Get-NtType command lists all supported types in the kernel.


```PowerShell
Get-NtType
```
Result:
```
Name
---
Type
Directory
Symboliclink
Token
Job
Process
Thread
.
.
.
```

The object type "Type" is this list of kernel types. Even this list is built from objects.

The properties of a type can be displayed with the following command:
```PowerShell
Format-List
```

To access each of these types, it's important to know about The Object Manager Namespace.

### The Object Manager Namespace