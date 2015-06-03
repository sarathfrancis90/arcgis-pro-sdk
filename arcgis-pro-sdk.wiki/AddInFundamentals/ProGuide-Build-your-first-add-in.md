This guide demonstrates how to build an ArcGIS Pro add-in. 

```
Language:      C#
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

**Step 1**

Open Visual Studio and select New Project.

![New Project](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/new-project.png "New Project")

**Step 2**

On the New Project dialog box, expand the Templates\Visual C#\ArcGIS or Templates\Visual Basic\ArcGIS folder. Select the ArcGIS Pro Add-ins folder (you may have other folders there if you also have the ArcObjects 10.x SDK for .NET installed).

**Step 3**

The ArcGIS Pro Module Add-in Visual Studio project item template displays. Select it and type _ProAppModule_ for the name. Change the location or accept the default. Click OK.

![pro-app-module](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/pro-app-module.png "Module")

**Step 4**

Visual Studio creates a new Visual C# or Visual Basic project for you. For the purposes of this document, Visual C# will be assumed; however, the procedure is almost identical for Visual Basic. You will notice the following content in your new project:

![project-content](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/project-content.png "Add-in Project content")


The Config.daml file opens (by default) in Visual Studio. The Module1.cs file contains your add-in module code. The code will be similar to this:

```csharp
internalclassModule1 : Module {

    private static Module1 _this = null;

	/// <summary>
    /// Retrieve the singleton instance to this module here
    /// </summary>

    publicstaticModule1 Current {
      get 
	  {
        return _this ?? (_this = (Module1)FrameworkApplication.FindModule("ProAppModule_Module"));
      }
    }
  }
```

Note that the module contains a private reference to your module instance, which is a singleton. The string ProAppModule_Module is your module ID. Framework uses this ID to reference your module in the DAML file and to find your associated module instance in the FindModule method of the FrameworkApplication module. By default, your module class is called Module1. The default namespace is the name you entered on the New Project dialog box, which in this case, is ProAppModule. Refer to the [ArcGIS Pro Extensibility document](ArcGIS-Pro-Extensibility) for more information on the role of a module.

Note also in the Config.daml file that the id attribute of the insertModule tag matches the ID within the Module1.cs file and the className attribute also matches the class name of the module.

```xml

<modules>
    <insertModule id="ProAppModule_Module" className="Module1" autoLoad="false" caption="Module1">

```

**Step 5**

Compile and build the project. If you have errors in your output window, check that you have the ArcGIS Pro application and the ArcGIS Pro SDK for .NET correctly installed. As long as you have not changed the syntax in any of the generated files (from the Project template), there should be no compilation errors.

**Step 6**

Open Windows Explorer. Navigate to the `C:\Users\<Your user name>\Documents\ArcGIS\AddIns\ArcGISPro` folder. You should see the add-in folder for your newly compiled add-in. The name of the folder corresponds to the ID (GUID) of your add-in.

```xml
<AddInInfo id="{c1968fdf-0368-45a2-8fa5-4f079c016b53}" ...
```
Double-click the folder. You will see your add-in file within the folder.

![guid-folder](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/guid-folder.png "Add-in guid")

**Step 7**
Right-click the project title within Visual Studio and select Add > New Item from the Project Context menu.

![new-item](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/new-item.png "Add new item")

From the Visual Studio Add New Item dialog box, choose ArcGIS Pro Button from the list of available ArcGIS Pro SDK Item templates. Keep the default name Button1.cs and click OK.

![item-templates.png](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/item-templates.png "Item template")

Note: If your Config.daml file is open in Visual Studio, you get the following prompt:

![config-daml-warning](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/config-daml-warning.png "DAML warning")

Click Yes or Yes to All.

**Step 8**

Within Visual Studio you should now see a new file, Button1.cs, that has been added to your project. Double-click that file and type the following code into the OnClick method:

```csharp
	internalclassButton1 : Button {

	protectedoverridevoid OnClick() 
	{
		string uri = ArcGIS.Desktop.Core.Project.Current.URI;
		ArcGIS.Desktop.Framework.Dialogs.MessageBox.Show( string.Format("Project uri {0}", uri));
	}
}
```

**Step 9**

Build and compile your add-in.

**Step 10**

In the Solution Explorer, click the the Config.daml file to open it. Find this line in the Config.daml file that defines the button:

```xml
<button id="ProAppModule1_Button1" caption="Button 1" className="Button1" loadOnClick="true" smallImage="Images\GenericButtonBlue16.png" largeImage="Images\GenericButtonBlue32.png" condition="esri_mapping_mapPane">
```

Delete the condition attribute. The completed line should look like this:

```xml
<button id="ProAppModule1_Button1" caption="Button 1" className="Button1" loadOnClick="true" smallImage="Images\GenericButtonBlue16.png" largeImage="Images\GenericButtonBlue32.png">
```

Rebuild the project.

**Step 11**

Place a breakpoint in the margin of your OnClick method and run your add-in in the Visual Studio debugger.

![debug](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/debug.png "Break point")

**Step 12**

ArcGIS Pro starts. Log in with your Organization ID when prompted, and allow ArcGIS Pro to complete its initialization. Create a new empty project using the template of your choice or open one of your existing projects.
![new-pro-project](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/new-pro-project.png "New ArcGIS Pro Project")

**Step 13**

Click ADD-IN TAB and click the Button 1 add-in button.

![add-in-tab](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/add-in-tab.png "Add-in tab")

![new-button](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/new-button.png "New button")

**Step 14**

Your breakpoint will be hit in Visual Studio. Press F5 or click the Continue button to continue execution. The following message box displays.

![code-break-point](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/code-break-point.png "Code break point")
![msg-box](images/How to Build an Add-in with the ArcGIS Pro SDK for .NET/msg-box.png "Message Box")

Click OK. Stop debugging.
