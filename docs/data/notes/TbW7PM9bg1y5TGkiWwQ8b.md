
.NET is a framework that contains a large set of programs to be called from the program you are building. These functions/programs can be as simple as *join 2 arrays*, or can be as complex as *translate voice to text*/*recognize red object in an image*.

.NET is made up of an SDK and a runtime environment

the languages that are used in the framework can all be compiled down to a common language
- ex. [[c#]], VB, IronPython (a .NET implementation of Python)

The computers that run a .NET application need to have the corresponding .NET framework installed prior to being able to run it.

### Major components
Common Language Runtime (CLR) – This allows the executions of programs written in the .NET framework using C#, VB, Visual C++
- also used to provide services such as memory management, security, exception handling, loading and executing of the programs.

Framework Class Library (FCL) – This is integrated with the CLR of the .NET framework and allows writing programs using .NET supporting programming languages such as C#, Visual C++, VB, etc.

User and Program Interfaces – This provides tools to develop desktop and windows applications. Windows forms, web services, Console applications and web forms are some examples of the user and program interfaces.

### ASP.NET vs .NET
.NET is a (software) development platform that is used to develop, run and execute the applications
- unified environment is the key value adder of .NET

ASP.NET is a web framework that is used to build dynamic web applications
- ASP.NET is part of the .NET framework
    - because of this, developers have access to all of the .NET classes and features
- applications developed by ASP.NET are largely component-based and built on the top of the common language runtime (CLR) and can be written in any of the .NET languages.
- ASP stands for Active Server Pages

### .NET Framework vs .NET Core
Both are are .NET implementations for building server-side applications.
- the biggest advantage of .NET Core is that it is cross-platform, unlike the .NET Framework which is tied to Windows.

.NET 5 is the successor to both .NET Core and .NET Framework


## Example projects
- https://github.com/dotnet-architecture/eShopOnWeb