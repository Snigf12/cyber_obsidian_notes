NT objects are a way to represent and access resources and abstractions in the Windows NT operating system: 

- **What they are**:
	- NT objects represent system resources and kernel abstractions, such as files, events, devices, and ports. 
 
- **How they are used**:
	- Users access objects via the per process handle table. When an object is opened, a pointer to the object is added to the handle table. The user then references the object using the handle.

- **How they are created and destroyed**:
	- The Object Manager is responsible for creating and destroying objects.

- **How they are protected**:
	- NT objects are protected from unauthorized use while still allowing for data and resource sharing.

 - **Examples**:
	 - Some examples of NT objects include:
     - **File objects**: Represent a user-mode application's connection to a device 
    - **Device objects**: Represent each driver's logical, virtual, or physical devices 
    - **Driver objects**: Represent each driver's load image 

NT objects are part of the object-oriented design paradigm, which makes the operating system flexible and portable.

Ref: [Microsoft](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/object-based), [University of Miami](https://www.cs.miami.edu/home/burt/journal/NT/handle_table.html), [University of Buffalo](https://cse.buffalo.edu/~bina/cse421/fall99/lectures/lec4/tsld040.htm)