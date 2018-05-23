# ETL using SSIS

ToC

## Introduction

In my experience, there is too little material to fully harness the true power of SSIS. Microsoft has done a nice effort to document SSIS for basic usage, but when things get more complex, the documentation does not provide a full overview. Therefore I set out to create a more complete guide on how to create SSIS packages, with a focus on understanding and automating the technology. In this guide you will find how SSIS packages are structured and how they can be generated through C#. At the time of writing, VB still seems to be an option, however, I would always prefer C# over VB for obvious reasons.

Holy wars aside, be sure to practice creating SSIS packages as there are many pitfalls to avoid and you will only truely learn by working in the field.

My hope is that this guide will save you from having to take the hard way to SSIS glory. Enjoy!

### What is SSIS and why would I use it?

Microsoft SQL Server Integration Services (SSIS) at its core is a framework of libraries and tools within the Microsoft .Net universe which empowers its users to process large amounts of data in a convenient way and to perform actions accordingly, such as generating reports or sending emails. It is part of the Microsoft Integration Services platform. It also provides a graphical representation of the business logic, making things a lot easier when to grasp, especially for non-technical stakeholders. SSIS provides a layer between data sources and data warehouses or other systems such as for instance Microsoft Identity Manager (MIM). 

In the past, data processing was often done directly in the database itself by virtue of stored procedures. However, these stored procedures would then grow out of control as data analysts would force SQL to do their bidding. SQL is not always the optimal language to perform data processing. Consider for instance data validation using regular expressions. It would be much more straightforward to perform such validations in a richer language such as C#.

One competitor which you may encounter in the wild is SAS. SAS is an ancient technology (it dates back to the 1970's) which at its time was the world leader in data procesing. Even nowadays there will be data analysts swearing high and low for their SAS. Once, I encountered it in the field with the goal to decommission it in favour of SSIS and to finally put an end to the wild growth of the SAS scripts with their cryptic and archaic syntax. 

SSIS was first introduced in SQL Server 2005 as a replacement for the Data Transformation Services (DTS). Note that Microsoft is notouriously bad at renaming their technologies. As such, you will encounter the DTS name in a lot of places when working with SSIS.  

### Licensing

SSIS can be used on any licensed Microsoft Windows Server.

To be verified. Wiki says only for SQL Server Standard, BI or Enterprise editions.

### Why SSIS?

Now the question becomes, why would one choose to use SSIS? Doesn't it mean that Microsoft thightens its grip on the company? Indeed it does, but then again if the company already uses Microsoft Windows servers (on-site or in the cloud) then there's nothing to lose, right?

Using SSIS also entails that it will be easier to find new developers to join the team. The only requirements for maintaining SSIS packages are a basic understanding of SQL and C#. For more advanced use of SSIS, such as developing and deploying custom components, a more experienced developer might be in order.

The lack of additional annual license costs also appeals to the business. I say additional, as you still need to have valid licenses for some Microsoft Windows server.

## Designing an ETL Process

Before we start hacking away at SSIS packages, we need to make some major decisions on the architecture we will use. In this chapter we will look into several architectural measures that can be tken to improve the performance and extensibility of our SSIS process.

The Extract-Transform-Load (ETL) process is an industry standard way of transferring and processing data from one or multiple sources to one or more destination systems.

In the Extract stage, the data is retrieved from the different source systems. The Transform stage processes the extracted data by applying validation, filtering and business logic rules. Once processed, the Load stage transfers the data to the destination systems.

Insert figure and reference.

### Staging Tables

An extension to the ETL scheme is to use so-called staging tables. These tables contain the data after the extract phase of the process. Many argue that this is no longer an ETL process but rather a ELETL process, as the data is first extracted from the data source and then loaded into the staging tables. However, due to the benefits of this approach, it is commonly used in ETL processes and can even be necessary.

So what are these benefits? First and foremost, the staging tables function as a proxy for the original source system. Consider for instance that the source system is a heavily used SQL database. Instead of keeping a connection open to perform the transfer and processing of data, a single export can be performed. After this export, the source system is no longer bothered and the connection can be closed.

The second main benefit of staging tables is the decoupling of the different ETL stages. Consider for example an ETL process which needs to process data every five minutes. In order to cope with a growing amount of data, staging tables would be used between both theextract and transform stages as well as between the transform and load stages (the staging tables are then often called 'transformed' tables). As the data is now stored intermediatelly, the three stages can be run independently and asynchonously. This parallelisation allows the throughput of the ETL process to be tripled at the cost of the resources required for the staging tables.

Insert link to blog.

### Slowly Changing Dimensions (SCDs)

Todo: Explain concept and elaborate on how to do this in SSIS.

## SSIS Environment Setup

Working with SSIS packages requires a special toolset. The tool of choice in this book is Visual Studio 2017, although older versions can be used. Visual Studio is extended with the .Net framework and SQL Server Data Tools (SSDT). The latter are the actual tools and packages related to SSIS and can be downloaded separately from (https://docs.microsoft.com/en-us/sql/ssdt/download-sql-server-data-tools-ssdt). Note that this only installs the 32-bit version of the SSIS tools and components. In order to do a full installation, the SQL Server installer should be used, as is dicussed later on in this section.

An alternative would be to use ... (BIDS & SSDT-BR)

Next to Visual Studio, we will also need a SQL Server installation. SSIS is compatible with many versions of SQL Server, but here we will only discuss the 2016 and 2017 versions. Evaluation versions of these can be downloaded from the Microsoft Evaluation Center (https://www.microsoft.com/evalcenter/evaluate-sql-server). The table below indicates the differences between versions of SQL Server.

Insert table (include folder paths GAC and Program Files)
2017	14	140
2016	13	130

To schedule, run and debug the SSIS packages, it might also be useful to have the SQL Server Management Studio (SSMS) installed. This can be downloaded from its Microsoft website (https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms). As stated by the Microsoft docs page, SSMS is an integrated environment for managing any SQL infrastructure.

SSIS can be installed on SQL Servers through the SQL Server installer, listed as Integration Services under the Shared Features. If the server is to host the SSISDB catalog, the Database Engine Services should also be selected from the Instance Features. There are two scenarios when installing SSIS, either the server does not have any version of SSIS installed (standalone mode) or there are other versions of SSIS present (side-by-side mode). In case of an upgrade to a newer version of SSIS, a side-by-side installation is done, retaining the previous version. 

To install the tools for programming packages and components select the Client Tools SDK item from the Shared Features in the SQL Server installer.

Note that in order to be able to set up a dedicated ETL server, a local instance of the SQL Server Database Engine should be installed when installing the SSIS components. Otherwise, the server will only be able to issue packages to other servers without actually executing them itself, impacting the performance of the target server.

Todo: SSDT available for community editions?

### Setup Logging

By default, not all events generated by SSIS are being logged. Examples of these events are the start and finish of running packages. This has been done to prevent excessive logging. To enable this level of logging, the registry should be tweaked so that the key `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS\LogPackageExecutionToEventLog` is set to 1.

### Run as Administrator

One of the pitfalls is that the Visual Studio instance used to run and create SSIS packages and components should be run as administrator. For instance, when creating a custom component, we need to write to the Program Files directory. In order to run SSIS packages, the DTExec program, which runs the packages, requires administrator privileges. 

### Requirements

On the technical side, there are some requirements to start working with SSIS.

Todo: Explain hardware requirements

## The SSIS Architecture

SSIS uses a custom architecture as can be seen in the XML version of an SSIS package. In this chapter we will dive into this architecture to gain the insight we need to design and create packages. In chapter X we will also depend on this insight when we look into creating a package programmatically.

Everything begins with a solution. Solutions contain projects and projects in turn contain packages.

### Packages

Packages are stored by default in an XML format (.dtsx). In order for these packages to perform an action, they need to be loaded into an executable environment. In the case of SSIS, this environment is the DTExec executable.

Todo: What about the .ispac format?

SSIS packages can be split up into two planes of execution: control flows and data flows 

### Control Flows
The control flow provides the execution flow on a high level. Each package has a single control flow. Control flows consist of tasks. These tasks usually do not process data directly but instead process the control over which data flows will be executed, similar to the concept of control flow in programming as a whole. To connect to the data flow plane, a so-called data flow task needs to be added in the control plane. 

List tasks

### Data Flows
The data flow plane handles the actual low level processing of the data on a buffer by buffer basis. It contains the different steps the data is run through. Here the flows consist of components instead of tasks.

List components

## Creating SSIS Packages using the Designer

The easiest way to create packages is by using the designer in Visual Studio or ... . First you need to create an Integration Services project. This project template is only available if you have installed the SSDT requirement. Visual Studio will automatically wrap the project in a new solution if there was no solution opened yet. Note that the solution is not shown in the solution explorer by default. This behaviour can be changed by changing this option in the Tools > Options > Projects & Solutions > General section. There you will find an option called "Always show solution". In the new project, you can create a new empty package which can then be filled by dragging and dropping tasks onto the control flow screen of the package. 

The project settings contains a setting for the target version of the project (General > Configuration Properties > TargetServerVersion). Changing the target version will also change the version of the SSDT being used and the folders in which Visual Studio will look for tasks and components. 

Todo: add reference to table with versions 

To connect the control flow to the data flow, so-called dataflow tasks are used.

In the case of dataflow tasks, the data flow screen can be used to drag and drop components into the flow. Tasks and components are configured by double clicking them and changing their settings in the window that pops up. These windows are called the custom editor of the task or component. There is also an advanced editor for dataflow components, which allows us to inspect the component properties and the individual input/output columns. When creating a custom component, it is optional to create a custom editor as the advanced editor will always be provided by default.

One thing you should get used to when working with the designer is that it tends to crash often. The dataflow components need to refresh their metadata on the data sources frequently.  Whenever the sources referenced change, e.g. a source csv file or SQL database, a refresh is triggered. When a refresh operation is performed, the screens opened in the designer will go blank several times. Depending on the position of several celestial entities, the designer may or may not seize up on one of the blank screens. In this case, the project needs to be closed and restarted completely. 

Also beware the undo/redo functionality. It remembers up to 20 operations, but not all operations can be reverted. Therefore, it can be quite confusing and even dangerous at times.

## Creating SSIS Packages Programmatically

Now you know how to create packages in the designer, we'll take it to the next level. Consider the following situation. A customer has an extensive set of tables in a database and requests you to create an ETL process to pump this data across to a data warehouse. You think: "I can do that! I'll whip up an ETL flow in no time with the designer!". But then reality kicks in when you need to write over a hundred near-identical data flows because there are that many source tables to extract. But wait, the real fun begins when you need to adjust all of the due to a change in the requirements or data sources. A truely unmaintainable setup which can only lead to planning disasters.

To avoid such tragedies, we will now look at how we can leverage the power of C# to generate SSIS packages programmatically. 

### Project Setup

The type of project used to create packages programmatically is a SSIS project. In this project you will need to add some references to the SSIS assemblies. These assemblies include the different COM objects and classes needed to create packages. They can be added by right-clicking the references item of the project in the solution explorer and selecting "add reference". 

Insert table of references and their locations.

### Custom Components

Even though the SSIS default toolbox provides a nice set of components as is, you will eventually reach a point at which you want to use more complex logic. An example of such complex logic which is not available programmatically is validation using regular expressions. Using the designer, you can use a script component, however, this component is not available when taking the programmatical approach. To still be able to harness the power of C# and the CLR, custom components can be created.

#### Deployment

##### Adding the Custom Components to the GAC

In order to use custom components, they need to be installed in the global assembly cache (GAC) of your machine with a strong name. The latter is done by signing the assembly of the custom component. Note that there are multiple GAC's, each with a division between the CPU architectures for which assemblies are installed. For each architecture the assemblies are contained in separate folders for each target version of the assembly. In the case of SSIS, there are multiple assemblies which we have included during the development of the custom components. Note that not all of them are made for the same architecture. The target version of these assemblies should all be the same (version 14 for SQL Server 2017, version 13 for SQL Server 2016).

To automatically deploy the component to the GAC, a post-build option can be added in the project settings to include the command to unregister the previous version of the component and to register the newly built version. The following commands are use to do this:

```
gacutil.exe /u componentName 
gacutil.exe /iF <path to component>

```

Note that the version of the gacutil can be troublesome, as with every version of .Net there is a different gacutil in a different location. To determine which version is used by Visual Studio, open the developer console for Visual Studio from the start menu and issue the command `where gacutil`.

##### Signing the Assembly with a Strong Name

Signing the assembly is very easy in Visual Studio. Simply open the project properties of your custom component project and select the signing tab from the left-hand navgation. There you can check the box to sign the assembly and select either to create a new key pair or to reuse an existing one. Using a password for the key pair is optional. 
 
##### Adding Custom Components to the SSIS Toolbox

Next to the GAC, the custom component assembly should also be present in the SSIS component folder (C:\Program Files(x86)\...). This is the folder where Visual Studio will look for components to display in the SSIS toolbox. When a component does not show up in the toolbox, the issue might be that the target version of the SSIS project is incorrect. As before, if the target version is SQL Server 2017, the assembly should be present in the 140 folder. For SQL Server 2016 this is the 130 folder.  

On the project settings page, under the build options, the output folder can be changed to directly build the component in the SSIS folder. Do keep in mind that doing so requires you to run Visual Studio as an Administrator.

## Debugging

Whether you create packages graphically or programmatically, debugging them will always be a necessary evil. There are several ways to do this and when working with custom components, debugging can become quite a challenge. In this chapter we will look at how this can be done.

A first and simple way of debugging the execution of packages is to add data viewers. In the designer, they can be added on the data flow level by right-clicking a connection between two components and selecting "enable data viewer". Once enabled, the execution of the package will halt at the data viewer, displaying the buffer of records which is sent through the connection in a pop-up. This data can then be sent on its way by pressing the green play button in the top-left corner of the pop-up window. 

Things could already be going sideways long before the execution of the SSIS packages. When generating the packages programmatically, Visual Studio can be use to debug. One way is to catch and inspect exceptions. In doing so, you will quickly find that the error messages are not particularly helpful. Often the issue is related to some COM objects, as the SSIS framework heavily depends on them, resulting in an error code which is not human-readible. In this case the error code can be looked up online. Alternatively, you can translate the error code with the following function:

```
COM object debugging code goes here...
```

Now imagine that you would actually use the custom components you create. In this case you might have to debug your custom code during execution. One way is to write a lot of debugging information to the Windows event log, but it might be more interesting to single-step your code. As this code is located in a separate assembly, doing so requires some more advanced debugging.

First, open your package you want to debug in Visual Studio. Next open the source code for your custom component in a second instance of Visual Studio. Set a breakpoint in the package execution, such as a data viewer, and attach the second instance of Visual Studio to the first one. This is done by selecting the Attach option from the Debug menu. In the list of processes to attach to, select the devenv process which corresponds to the first Visual Studio instance. If done correctly, any breakpoints set in the source code of the component should be triggered when the code is called by the first instance of Visual Studio. Note that the version of the source code needs to be exactly the same as the one used to build the assembly of the component. 

## Deployment of an SSIS Project

### Using the SSMS Wizard

SSIS projects are deployed to the SSIS Catalog of the SQL database. SSMS provides an Integration Services Wizard to this end.

First, connect to the SQL server hosting the SSISDB database in SSMS as the server administrator. In the object explorer, the SSISDB is located under the Integration Service Catalogs node. The projects node provides the option Deploy Project to launch the wizard.

The wizard starts by asking which source needs to be deployed. The files used to perform a deployment have the extension .ispac. Alternatively, a project can be selected from the Integration Services Catalog of another server. Once the source is selected, the wizard asks about the destination.

After completing the wizard, the results page opens, listing the status of the different actions in the deployed project. For steps which failed, a more detailed report can be retrieved by clicking the failed state. The results page also allows generating reports and storing them to XML files. 

### Using T-SQL in SSMS

Again, we first need to connect to the SSISDB catalogue database in SSMS. Once connected, open a new query window and paste the following code (replacing the parameters between angle brackets):

```
DECLARE @ProjectBinary AS varbinary(max)
DECLARE @operation_id AS bigint
SET @ProjectBinary = 
	(SELECT * FROM OPENROWSET(BULK '<project_file_path>.ispac', SINGLE_BLOB) AS BinaryData)

EXEC catalog.deploy_project @folder_name = '<target_folder>',
	@project_name = '<project_name',
	@Project_Stream = @ProjectBinary,
	@operation_id = @operation_id out
```

The above code opens the ispac file specified and loads its contents into the catalog using the stored procedure deploy\_project. It could also be run using Visual Studio Code, but that is out of the scope of this book.

### Using the command prompt

There is an executable which can be run from the command line to deploy packages. This executable is called ISDeploymentWizard and can be found in the DTS installation folder (e.g. C:\Program Files (x86)\Microsoft SQL Server\140\DTS\Binn for SQL Server 2017). As the name indicates, running this command will trigger a wizard to guide us through the deployment. On the select source page, either an .ispac file or a package in an existing catalog can be provided. As destinations even Azure databases can be provided using the format `<server_name>.database.windows.net`.

### Using PowerShell

Deploying a package can also be scripted using PowerShell, as is shown in the following code:

```
# Variables
$SSISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"
$TargetServerName = "localhost"
$TargetFolderName = "Project1Folder"
$ProjectFilePath = "C:\Projects\Integration Services Project1\Integration Services Project1\bin\Development\Integration Services Project1.ispac"
$ProjectName = "Integration Services Project1"

# Load the IntegrationServices assembly
$loadStatus = [System.Reflection.Assembly]::Load("Microsoft.SQLServer.Management.IntegrationServices, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91, processorArchitecture=MSIL")

# Create a connection to the server
$sqlConnectionString = "Data Source=" + $TargetServerName + ";Initial Catalog=master;Integrated Security=SSPI;"
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString

# Create the Integration Services object
$integrationServices = New-Object $SSISNamespace".IntegrationServices" $sqlConnection

# Get the Integration Services catalog
$catalog = $integrationServices.Catalogs["SSISDB"]

# Create the target folder
$folder = New-Object $SSISNamespace".CatalogFolder" ($catalog, $TargetFolderName,"Folder description")
$folder.Create()
Write-Host "Deploying " $ProjectName " project ..."

# Read the project file and deploy it
[byte[]] $projectFile = [System.IO.File]::ReadAllBytes($ProjectFilePath)
$folder.DeployProject($ProjectName, $projectFile)
Write-Host "Done."
```

As we can see in the code, the `Microsoft.SQLServer.Management.IntegrationServices` assembly provides us with the operations we need to deploy packages. As an alternative to the authentication used above, SQL Server authentication could be used as well. To do so, replace the `Integrated Security=SSPI;` argument with `User ID=<user name>;Password=<password>;`. 

### Using C# in a .NET application

Deploying a package can also be done in C# natively. As a first step we will need the connection information to connect to the destination database. This can be either a local database or one hosted on Azure. In both cases you will need an ADO.Net connection string.

Note that if you are connecting to an Azure database from behind a firewall, you need to ensure that port 1433, which is used by SQL Server is opened. Otherwise you will not be able to reach the server.

Next we need to create a new console application in Visual Studio. We need to add the following references:

- Microsoft.SqlServer.Management.Sdk.Sfc
- Microsoft.SqlServer.Smo
- Microsoft.SqlServer.Management.IntegrationServices

The first two can be found in the Reference Manager under assemblies > extensions, while the last one is installed only in the Global Assembly Cache (GAC). In the add Reference Manager select browse and nnavigate to the folder `C:\Windows\assembly\GAC_MSIL\Microsoft.SqlServer.Management.IntegrationServices\14.0.0.0__89845dcd8080cc91`. There you should find the correct DLL.

Next we can put the following code in the Program.cs file:

```
using Microsoft.SqlServer.Management.IntegrationServices;
using System;
using System.Data.SqlClient;
using System.IO;

namespace deploy_ssis_project
{
	class Program
	{
		static void Main(string[] args)
		{
			// Variables
			string targetServerName = "localhost";
			string targetFolderName = "Project1Folder";
			string projectName = "Integration Services Project1";
			string projectFilePath = @"C:\Projects\Integration Services Project1\Integration Services Project1\bin\Development\Integration Services Project1.ispac";
			
			// Create a connection to the server
			string sqlConnectionString = "Data Source=" + targetServerName + ";Initial Catalog=master;Integrated Security=SSPI;";
			SqlConnection sqlConnection = new SqlConnection(sqlConnectionString);
			// Create the Integration Services object
			IntegrationServices integrationServices = new IntegrationServices(sqlConnection);
			// Get the Integration Services catalog
			Catalog catalog = integrationServices.Catalogs["SSISDB"];
			// Create the target folder
			CatalogFolder folder = new CatalogFolder(catalog, targetFolderName, "Folder description");
			folder.Create();
			Console.WriteLine("Deploying " + projectName + " project.");
			byte[] projectFile = File.ReadAllBytes(projectFilePath);
			folder.DeployProject(projectName, projectFile);
			Console.WriteLine("Done.");
		}
	}
}
```

As you can see, this code is very similar to the one we used to deploy from PowerShell. As was the case there, the type of authentication can be changed to SQL authentication by replacing the `Integrated Security=SSPI;` argument with `User ID=<user name>;Password=<password>;`.

## Executing an SSIS Package

### Using SSMS

First connect to the database server on which the SSIS package has been deployed. In Object Explorer, expand Integration Services Catalogs and then expand SSISDB to view the objects in the SSIS Catalog database. Next, the package can be run by right-clickinng it and selecting execute. Any parameters can be set using the Parameters, Connection Managers and Advanced tabs of the window that opens.

Note that this technique will generate a command to call DTExec, the utility used to execute the actual SSIS package.

### Using T-SQL in SSMS

As before, connect to the database. Create a new query window with as target database SSISDB and insert the following query:

```
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx',
	@execution_id=@execution_id OUTPUT,
	@folder_name=N'Deployed Projects',
	@project_name=N'Integration Services Project1',
	@use32bitruntime=False,
	@reference_id=Null
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,
	@object_type=50,
	@parameter_name=N'LOGGING_LEVEL',
	@parameter_value=@var0
EXEC [SSISDB].[catalog].[start_execution] @execution_id
GO
```

### Using the command prompt

On the command line, the executable DTExec.exe can be used to execute SSIS packages. This executable can be found in the DTS folder (e.g. C:\Program Files (x86)\Microsoft SQL Server\140\DTS\Binn for SQL Server 2017).

The command should then look like this: 

`dtexec /ISServer "\SSISDB\Project1Folder\Integration Services Project1\Package.dtsx" /Server "localhost"`

To use SQL Authentication, specify the /User and Password parameters with appropriate values. A detailed explanation of the parameters for DTExec can be found on: https://docs.microsoft.com/sql/integration-services/packages/dtexec-utility.

### Using PowerShell

The following script will execute a deployed SSIS package:

```
# Variables
$SSISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"
$TargetServerName = "localhost"
$TargetFolderName = "Project1Folder"
$ProjectName = "Integration Services Project1"
$PackageName = "Package.dtsx"

# Load the IntegrationServices assembly
$loadStatus = [System.Reflection.Assembly]::Load("Microsoft.SQLServer.Management.IntegrationServices, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91, processorArchitecture=MSIL")

# Create a connection to the server
$sqlConnectionString = "Data Source=" + $TargetServerName + ";Initial Catalog=master;Integrated Security=SSPI;"
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString

# Create the Integration Services object
$integrationServices = New-Object $SSISNamespace".IntegrationServices" $sqlConnection
# Get the Integration Services catalog
$catalog = $integrationServices.Catalogs["SSISDB"]
# Get the folder
$folder = $catalog.Folders[$TargetFolderName]
# Get the project
$project = $folder.Projects[$ProjectName]
# Get the package
$package = $project.Packages[$PackageName]
Write-Host "Running " $PackageName "..."
$result = $package.Execute("false", $null)
Write-Host "Done."
```

### Using C# in a .Net Application

This approach is similar to the deployment of a package using a .Net application. Only the code used slightly differs:

```
using Microsoft.SqlServer.Management.IntegrationServices;
using System.Data.SqlClient;

namespace run_ssis_package
{
	class Program
	{
		static void Main(string[] args)
		{
			// Variables
			string targetServerName = "localhost";
			string folderName = "Project1Folder";
			string projectName = "Integration Services Project1";
			string packageName = "Package.dtsx";
			// Create a connection to the server
			string sqlConnectionString = "Data Source=" + targetServerName + ";Initial Catalog=master;Integrated Security=SSPI;";
			SqlConnection sqlConnection = new SqlConnection(sqlConnectionString);
			// Create the Integration Services object
			IntegrationServices integrationServices = new IntegrationServices(sqlConnection);
			// Get the Integration Services catalog
			Catalog catalog = integrationServices.Catalogs["SSISDB"];
			// Get the folder
			CatalogFolder folder = catalog.Folders[folderName];
			// Get the project
			ProjectInfo project = folder.Projects[projectName];
			// Get the package
			PackageInfo package = project.Packages[packageName];
			// Run the package
			package.Execute(false, null);
		}
	}
}
```

## References
To piece together all the information presented in this book, I used many different sources. The are provided here as references so that you might be able to find additional information when needed.

- SSIS MSDN Forum: https://social.msdn.microsoft.com/Forums/home?forum=sqlintegrationservices
- StackOverflow questions related to SSIS: http://stackoverflow.com/questions/tagged/ssis
- SSIS MSDN Blog: https://blogs.msdn.microsoft.com/ssis/
- Issue and feature request tracker: https://feedback.azure.com/forums/908035-sql-server

Note that you can also have the SSIS documentation installed on your machine as an offline version. (To be detailed...)

The list below provides some sample code scattered all over the internet in which I was able to find answers to my questions.

Todo: add references for example code.

## Glossary
## Index
## Appendices
