A GUID, or Globally Unique Identifier, is a 128-bit binary number that acts as a unique identifier for objects: 

* **What it is**
	* A GUID is a unique identifier that can be used across networks and computers to identify objects such as software, hardware, accounts, and documents. GUIDs are also known as Universally Unique Identifiers (UUIDs). 

* **How it looks**
	* A GUID is a string of 32 hexadecimal digits, arranged in groups of 8, 4, 4, 4, and 12. For example, a GUID might look like this: 6B29FC40-CA47-1067-B31D-00DD010662DA. 

* **How to generate it**
	* GUIDs can be generated using online tools or dedicated libraries for programming languages. 

* **How to use it**
	* In SQL Server, the uniqueidentifier data type represents a GUID. In Active Directory, the objectGUID attribute is assigned a GUID when an object is created. 

GUIDs are useful because they have a very low probability of being repeated. They also allow vendors to create and register unique IDs without contacting a central authority. 

Ref: [Microsoft](https://learn.microsoft.com/en-us/dotnet/api/system.guid?view=net-9.0), [Microsoft](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-adts/dd4dc725-021b-4c8c-a44a-49b3235836b7), [Calendia](https://catenda.com/glossary/globally-unique-identifier-guid/), [TechTarget](https://www.techtarget.com/searchwindowsserver/definition/GUID-global-unique-identifier)