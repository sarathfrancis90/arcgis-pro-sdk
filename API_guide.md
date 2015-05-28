##ArcGIS XI DotNet API Documentation Guide
The guide is split into three parts:
* A Quick Reference Guide
* Setting up for check in/out of Star Team
* A Quick Reference Guide
* A more detailed API Documentation Reference documentation generation process
* An Appendix containing a code comment formatting reference:

There are also numerous examples of code comments in this guide. Additionally, there is an example csproj illustrating different examples of code comment formatting and structuring that can be downloaded from the <a href=" https://devtopia.esri.com/ArcGISPro-SDK/win-net-documentation">Pro App SDK github repository</a> . You will need to login to devtopia first.

##Purpose of This Guide
The purpose of this guide is to provide you, the author, with step by step instructions on how to document the Reference API using csharp code comments. Any publicly scoped type (to include protected base classes) and corresponding public and protected members in the ArcGIS Professional Managed Assemblies must be documented. The DotNet Reference API is one of the primary extensibility points for ArcGIS Professional available to 3rd party developers (the other extensibility points currently being Arcpy.Mapping via Python and Geoprocessing tools and toolboxes). 

##Prerequisites

You will need the following software installed to author Reference API content:
* Either Visual Studio 2012 and Visual Studio Update 4 (or the latest update if it has since rev’d since this document was written). Note: the version of Visual Studio should always exactly match the version of the solution (.sln) files used for the __ArcGIS Professional code base__ (not the version used for the SDK or samples). This version will change over time.
* [Visual Studio Spell Checker extension](http://visualstudiogallery.msdn.microsoft.com/7c8341f1-ebac-40c8-92c2-476db8d523ce)
* <a href = "\\esri.com\Software\Desktop\DesktopA-L\Innovasys\DocumentX\2013">Document! X for Innovasys.</a>. This requires a license. You must request this software through your Manager
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
The following steps assume you do NOT have ArcGIS Pro installed on your machine and there is NOT a folder called **C:\ArcGIS already on your local machine**.

###Install ArcGIS Pro

You will only install Pro manually from a .MSI file once.  All subsequent updates will be done using GetBuildRelease batch scripts.  

**Step 1**: Determine the most current build by opening the following file:  
	[\\archive\11Builds_64\ReleaseOK.txt](\\archive\11Builds_64\ReleaseOK.txt)  - note the build number.

**Step 2**: Based on the step above, install Pro from the appropriate folder in:  
	[\\archive\11NonDevSetups\XXXX\ArcGISPro](\\archive\11NonDevSetups\XXXX\ArcGISPro)  
	Be sure to install into the C:\ArcGIS folder (NOT the default Program Files folder)






###Run GetBuildRelease.bat


The GetBuildRelease batch file will run a process that will replace many of the release DLLs with build DLLs as well as install many additional support files.  These files make it possible to compile your updated source code files.  
First, you need to download the batch files from Star Team onto your local drive.  

**Step 1**: Open Star Team to the ArcGIS11-Gold view.  

Next you will build a folder structure on your local drive (within C:\ArcGIS) to store Star Team GetBuild batch files.  

**Step 2**:  Find the ArcGIS\SharedArcGIS folder in StarTeam. Right-click the **SharedArcGIS** folder and select “Create Working Folders”.  


**Step 3**: In Star Team, make sure the file tab is active, then highlight the SharedArcGIS folder (in the TOC), then click the All Descendants button to display the StarTeam files. Your screen should look similar to the graphic below.  

There are over 16K file that will be downloaded.  Many of the files in this shared folder are needed so you can complile your modified solutions.  There is also an SDK subfolder that will provide examples and code snippets.  

**Step 4**: Right-click Status: Missing (xxx items) and select **Check Out All …,** select Current revision and click OK.  

This can take a while but it is a one time check-out.  

**Step 5**: In file explorer, browse to C:\ArcGIS\SharedArcGIS\Build\GetBuildScripts.  Double-click the GetBuildRelease.bat.  

**Step 6**: When finished, you will be prompted to press enter to close the CMD.


**IMPORTANT NOTE (Running GetBuild for a specific build):**  

This may not happen often, but sometimes you don't want to move to the currently available build of Pro (because there could be known issues with it, you may want to roll back to another build for testing, etc).  For these cases, there is a file you can edit that allows you to control exactly which build of the software gets installed locally when you run GetBuildRelease.bat.  

To do this – Open the C:\ArcGIS\SharedArcGIS\Build\GetBuildScripts\GetBuildScript.ps1 file in NotePad and modify the second line to include the build number between the two double quotes.  

If there is no value between the quotes (just two double quotes), then the current release is installed when GetBuildRelease is executed.  

###Check out source code and other support files


Similar to the steps above, you are going to create additional Working folders.  But this time, before checking out the files, you will make sure that the source code files are synchronized (i.e., same build) with the build of ArcGIS Pro you currently have installed.  

**Step 1**: If necessary, open Star Team to the ArcGIS11-Gold view.  

**Step 2**: **Go to View--> Select Configuration…** then check the Labeled configuration / build number that matches your ArcGIS Pro install build number.  In the example below, the local install is for build 2971.  

The status bar in the lower left-hand corner should update with the appropriate label.  
	 
**Step 3**: Create a working folders for:  

ArcGIS\ArcGISDesktop\ServiceContracts – everyone needs this  

ArcGIS\ArcGISDesktop\ArcGISXXXX *  -  team specific folder(s)  

Create a working folder for your respective ArcGIS\ArcGISDesktop\ArcGISXXXX content (e.g. ArcGISLayout, ArcGISMapping, ArcGISCatalog, etc.), depending on which sub-folder pertains to your content.  

**Step 4**: Again, for each folder above make sure the file tab is active, then highlight that folder (in the TOC), then click the All Descendants button to display the StarTeam files. Finally right-click Status: Missing (xxx items) and select Check Out All and click OK.  

**Note**: you don’t need to set the label since you’ve already configured it in Star Team.  I.e., the Current revision will use the label you set.  Setting the label (to the same one) won’t hurt either.  


##Quick Reference Guide
 
**Step 1**: Run GetBuildRelease.bat from C:\ArcGIS\SharedArcGIS\Build\GetBuildScripts  

[\\archive\11Builds_64\ReleaseOK.txt](\\archive\11Builds_64\ReleaseOK.txt)  - note the build number.  

See notes above to run a GetBuild on a specific build number.  

**Step 2**: Set your Star Team label to match the ArcGIS Pro build number from above.  

Go to View --> Select Configuration.  

**Step 3**: Check out all “Missing” and “Out Of Date” files for all appropriate folders.  Be sure to check the All Descendants button first.  Use “Check Out…” on the Status: Missing (xxx items) or Status: Out of Date (xx items) groups.

Basic rules:  

* Be cautious to check out “modified” files from a labeled configuration because you may undo your changes.  
* Never check out “out of date” files from the “current” configuration because the files may not have been compiled.  
* Anything that is marked “Not in View” is something that got removed. You will need to manually remove that file from your local folder structure.  

To avoid these conflicts, it is best not to increment GetBuild by just a single build after changes were installed.  You want to make sure your changes have been installed into the source code.

**Step 4**: Click **F5** to refresh each folder to make sure files are not missing or out of date.  

**Step 5**: In File Explorer, double-click the solution file in the sub-folder you checked out. For example: ArcGISLayout.sln.  

 
**Step 6**: If you open the solution, make sure the solutions configuration is set to Release.  
	 
**Step 7**: Open the Configuration Manager (from the pull-down arrow next to Release in the graphic above) and make sure you 1) set the Active solution platform = x64 and 2) you are only building the solution for the managed code, not the native code.  In the example below, there is only a checked checkbox for ArcGISDesktop.Layouts.

 
Rule – you will make changes to the solution, but never check it into StarTeam, only check in .cs files.

**Step 8**: __KNOW WHAT *.CS FILES TO EDIT!__  Talk to your developer. For the Layout team, these files are located under API\Models:

	 
**Step 9**: Edit your “\\\” documentation.  __Suggested workflow__ - the Layout team API developer, stubs out the Triple Slash comments in all the appropriate places.  This process makes it much easier for the PE to know exactly where they need to insert their documentation.

	 
**Step 10**: Correct typos and spelling using the Visual Studio spell checker extension.

**Step 11**: Optionally, use the “Edit Documentation” Document! X Visual Studio extension to preview how your documentation will look on the output.

**Step 12**: Save your solution.

**Step 13**: Build your solution and verify there are no errors and close.

**Step 14**: Reset your Label back to __Current Configuration__.  Go to View --> Select Configuration.  You must be in the current view to create CRs and check in changes.  You always check into the current release.

**Step 15**: All source code changes must be associated with a CR.  __Create a CR__ for the Doc change (or use an existing CR). If it is a blanket CR for Doc changes that is fine. Assign yourself to the CR. 

**Step 16**: Compare contents.  Right-click your modified item and select __Compare Contents__.  This allows your to verify the changes made.  If things don’t look right, work with your developer.  In the graphic below, you see the original on the left and the modified file on the right.  In this example, the Dev only stubbed out the ///summary, not the remarks.  The revised version made the correction and added “///” doc.

 

**Step 17**: If the contents are OK, right-click on your modified items and select __Check In__
 
Again, only check in .cs files.

**Step 18**:  Associate CR with Check In and add comments.
	 
**Step 19**: Hope you did not break the build!!!

**Step 20**: Begin documenting your code comments. You must comment all public and protected types in the managed code only. You must comment all public and protected members on public and protected types (to include statics) with the following exception: protected members on public sealed classes should be __excluded__ from the API documentation. Refer to the section __Excluding types and members from the API Reference__ for more information.

You do NOT need to comment any internal classes or methods. For details on how to format and structure comments consult the Appendix and Reference sections of this document.










