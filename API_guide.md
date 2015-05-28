##ArcGIS XI DotNet API Documentation Guide
The guide is split into three parts:
* A Quick Reference Guide
* Setting up for check in/out of Star Team
* A Quick Reference Guide
* A more detailed API Documentation Reference documentation generation process
* An Appendix containing a code comment formatting reference:

There are also numerous examples of code comments in this guide. Additionally, there is an example csproj illustrating different examples of code comment formatting and structuring that can be downloaded from the <a href="https://devtopia.esri.com/ArcGISPro-SDK/win-net-documentation">Pro App SDK github repository</a> . You will need to login to devtopia first.

##Purpose of This Guide
The purpose of this guide is to provide you, the author, with step by step instructions on how to document the Reference API using csharp code comments. Any publicly scoped type (to include protected base classes) and corresponding public and protected members in the ArcGIS Professional Managed Assemblies must be documented. The DotNet Reference API is one of the primary extensibility points for ArcGIS Professional available to 3rd party developers (the other extensibility points currently being Arcpy.Mapping via Python and Geoprocessing tools and toolboxes). 

##Prerequisites

You will need the following software installed to author Reference API content:
* Either Visual Studio 2012 and Visual Studio Update 4 (or the latest update if it has since rev’d since this document was written). Note: the version of Visual Studio should always exactly match the version of the solution (.sln) files used for the ArcGIS Professional code base (not the version used for the SDK or samples). This version will change over time.
* [Visual Studio Spell Checker extension](http://visualstudiogallery.msdn.microsoft.com/7c8341f1-ebac-40c8-92c2-476db8d523ce)
* <a href = "//esri.com\Software\Desktop\DesktopA-L\Innovasys\DocumentX\2013">Document! X for Innovasys.</a>. This requires a license. You must request this software through your Manager
* [Star Team – the install document](http://devinfo/sites/DeveloperCentral/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2fsites%2fDeveloperCentral%2fShared%20Documents%2fStarTeam%20Documents&View=%7b4652831D%2d65B5%2d47FC%2d9D7D%2d4388EC6DFC2A%7d)

Additionally, you may choose to download the [CSharp code comment examples](https://devtopia.esri.com/ArcGISPro-SDK/win-net-documentation) on devtopia. It is a Visual Studio solution containing various csharp source files each illustrating different formatting and structuring techniques for code comments.

##Setting up for check in/out of Star Team
In order to successfully update Triple Slash “\\\” changes to source code, PE’s need to understand a few things about the development process like:
* Installing and then updating ArcGIS Pro using GetBuild batch files,
* Pulling source code from StarTeam and checking it back in,
* Keeping source code synchronized with Pro build numbers.
Note to Product Engineers: 
* Triple Slash “\\\” documentation requires that you read and write source code files from StarTeam.  This means that you need the appropriate StarTeam permissions.  
* It is recommended that you have direct access to a machine in Redlands (it can be via remote desktop) because you will be running GetBuild  batch scripts that update your ArcGIS Pro build with current files.  If you are updating a remote machine, it could take a very long time.
* It is also recommended that you uninstall ArcMap from the machine that is being used to update “\\\” doc.  It may not affect the “\\\” process but it does have some negative affect on the registry for some developers.
The following steps assume you do NOT have ArcGIS Pro installed on your machine and there is NOT a folder called C:\ArcGIS already on your local machine.

###Install ArcGIS Pro

You will only install Pro manually from a .MSI file once.  All subsequent updates will be done using GetBuildRelease batch scripts. </br>
Step 1. Determine the most current build by opening the following file:</br>
	[\\archive\11Builds_64\ReleaseOK.txt](\\archive\11Builds_64\ReleaseOK.txt)  - note the build number.</br>
Step 2. Based on the step above, install Pro from the appropriate folder in:</br>
	[\\archive\11NonDevSetups\XXXX\ArcGISPro](\\archive\11NonDevSetups\XXXX\ArcGISPro)</br>
	Be sure to install into the C:\ArcGIS folder (NOT the default Program Files folder)</br>
