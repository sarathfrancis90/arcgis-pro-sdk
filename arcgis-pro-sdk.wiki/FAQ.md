
* [General](#general)
* [Availability](#availability)
* [Customization](#customization)
* [Framework API](#framework-api)
* [Geometry API](#geometry-api)
* [Geoprocessing API](#geoprocessing-api)
   * [Python](#python)
* [Mapping API](#mapping-api)
* [Migration from 10x](#migration-from-10x)
* [Geodatabase API](#geodatabase-api)
* [ArcGIS Pro SDK templates](#arcgis-pro-sdk-templates)

### General

Question: How do I provide feedback for the ArcGIS Pro SDK?

Answer: Provide feedback for ArcGIS Pro SDK using the ArcGIS Pro Beta forum. To access the forum, log on to the [ArcGIS Pro website](https://pro.arcgis.com/en/pro-app/) and click Forums. This link takes you to the ArcGIS Beta Community. Post feedback or questions to the relevant ArcGIS Pro forum.

Question:  Are there any plans to release a Java SDK for ArcGIS Pro?

Answer: No. Java would logically be the next language supported if the ArcGIS Pro SDK is extended beyond Python and .NET; however, there are currently no plans for this.

Question: Can any namespace with Internal be used to create add-ins?

Answer: Anything with an Internal name component in a namespace (that is, a subpath component that uses the literal string Internal) is unsupported. It should not be used. None of these Internal methods are documented and since they are internal, are subject to change at any release.

### Availability

Question: When will the ArcGIS Pro SDK be available?

Answer: The beta version of the .NET SDK is planned for the 1.1 beta release of ArcGIS Pro. The final version of the .NET SDK is planned for the 1.1 final release of ArcGIS Pro.

Question: What will be the licensing model for the ArcGIS Pro SDK?

Answer: The SDK for ArcGIS Pro comes with each level of ArcGIS 10.3 for Desktop (Basic, Standard, and Advanced). Developers at each level will be able to create customizations that correspond to that level. The end users of a customization need to have the appropriate level of ArcGIS for Desktop required for the customization.

Question: What versions of Visual Studio will be supported for the first release of the ArcGIS Pro .NET SDK?

Answer: The ArcGIS Pro .NET SDK will be certified on Visual Studio 2013, update 4 or later, for Windows 7, Windows 8, and Windows 8.1.

### Customization

Question: How can the Pro App be extended?

Answer: ArcGIS Pro can be extended in several ways. It can be customized through basic app configuration settings or with  **_Tasks_**; automated using Python and extended with a new .NET API and SDK; and through the familiar add-in model.

Question: What kind of add-ins will be supported in ArcGIS Pro?

Answer: All of the ArcGIS Pro UI elements will be supported by the .NET SDK add-ins to include support for custom controls authored by third-party developers. This includes buttons, tools, ribbons, panes and views, dockable panes, menus, galleries, split buttons, split tools, and toolbars. Nonvisual patterns, such as Drag-and-Drop extensibility, are also supported.

Question: Is the new ArcGIS Pro SDK similar to ArcObjects and COM extensions?

Answer: There will be many familiar patterns such as buttons (commands), tools, sketch or construction tools, dockpanes, menus, and menu items. There will be new patterns, too, such as galleries, split buttons, split tools, extensions (via add-in modules), and ribbons. However, the new SDK is not ArcObjects; therefore, existing ArcObjects code needs to be redeveloped.

Question: Will the ArcGIS Pro .NET SDK support stand-alone applications?

Answer: No. There will be no ArcGIS Engine equivalent for ArcGIS Pro. The .NET SDK is designed to work within the ArcGIS Pro application framework and cannot be run independently.

Question: Will the ArcGIS Pro SDK support automation?

Answer: Yes. Automation is supported via Python and the arcpy.mp module.

Question: I am an ArcGIS Runtime developer. What APIs in Pro will be the same? Will I have to learn a new object model?

Answer: A lot of work is going into the ArcGIS Pro .NET SDK API to make it as consistent as possible with the Runtime SDK and vice versa. The geometry API will be very similar to Runtime, though it will be a superset. You will find certain programming patterns, such as tasks and asynchrony, very familiar to your WPF device programming and the new Runtime APIs.

Question: What will be new for developers and ArcGIS Pro?

Answer: The integration of 3D with 2D is new and exciting. ArcGIS Pro is 64 bit, multithreaded, and leverages .NET support for asynchronous programming. ArcGIS Pro will have very tight integration with Portal for ArcGIS and ArcGIS Online content, making cross-platform workflows much easier to implement.

Question: Will the ArcGIS Pro .NET SDK support common editing tasks?

Answer: Yes. The Editor API provides base classes from which custom edit tools and commands can be derived.

Question: Can custom controls in add-ins be referenced in tasks (within ArcGIS Pro Task)?

Answer: Yes. Custom controls can be referenced in tasks the same as core controls.

Question: Will add-ins be supported in project packages?

Answer: Yes. Users will be able to pick which of their add-ins they want to package with their project. When unpacked, the package deploys its add-ins onto the system (for the current user only). This means these add-ins will always be available to the current user. Note: They are not per project.

Question: Can third-party extensions in add-ins use different versions of external assemblies with the same name?

Answer: References to external assemblies are resolved by the .NET runtime as described here http://msdn.microsoft.com/en-us/library/yx7xezcf(v=vs.110).aspx. Similar to 10.x add-ins, ArcGIS Pro can use an app configuration file to auto forward assembly references.

Question: Do I need to recompile my add-ins after installing a new release of ArcGIS Pro?

Answer: Consult http://msdn.microsoft.com/en-us/library/ff602939(v=vs.110).aspx for guidance on assembly version compatibility. ArcGIS Pro always tries to load add-ins compiled and deployed to previous releases of ArcGIS Pro. ArcGIS Pro can use an app configuration file to auto forward add-in assembly references, as mentioned above.

Question: Does ArcGIS Pro .NET SDK follow an existing framework pattern like Prism or MEF?

Answer: There are similarities. The underlying framework for ArcGIS Pro resides in ArcGIS.Desktop.Framework. It provides an add-in extensibility mechanism (via esriAddinX packages) as well as extensibility points via ArcGIS.Desktop.Framework.Contracts. There is also general support for events via an EventAggregator (similar to Prism) and threads via QueuedTask.

### Framework API

Question: Are there only two threads available to developers? Is it possible to use parallel programming techniques in the ArcGIS Pro .NET SDK API?

Answer: There are many threads in use by ArcGIS Pro. However, this is abstracted into either running your code in the foreground (UI) or background. To run in the background use ```QueuedTask.Run```. It takes care of synchronizing your code and changes to the state of ArcGIS Pro with other background threads that are running. Attempting to run tasks in parallel or on system threads (for example, via System.TaskFactory) likely puts ArcGIS Pro in an inconsistent state and crashes it.

### Geometry API

Question: Will the ArcGIS Pro .NET SDK provide functions for fine-grained feature geometry analysis?

Answer: The Managed API provides a geometry API that is symmetrical with the runtime geometry API. It provides a series of geometry classes and analysis methods via the GeometryEngine. These methods support constructions, proximity, hit test, spatial relationships, intersections, and curves.

Question: Why do I get a ArcGIS.Core.ConstructedOnWrongThreadException whenever I call certain methods from the UI thread?

Answer: Most of the ArcGIS Pro API functions that are synchronous have to run on the background thread. The idea is to keep the ArcGIS Pro UI responsive. This includes all of the ArcGIS.Core.Data classes and methods for geodatabase access as well as all of the classes and methods on the ArcGIS.Core.Geometry Geometry Builders. Here is an example of QueuedTask.Run. Anything placed within the lambda passed as the argument to ```Run()``` executes on the background thread:

```C#
await QueuedTask.Run(() => {

//your code here

   var queryFilter = new QueryFilter() { ... };
   featureLayer.Search(queryFilter,...);

});
```
There will be many samples provided showing the use of QueuedTask across all the ArcGIS Pro APIs.

### Geoprocessing API

Question: Are custom geoprocessing function tools written with ArcObjects (.NET, C++, Java) supported in ArcGIS Pro?

Answer: No. There is no COM support or ArcObjects for developers in ArcGIS Pro. You can develop custom geoprocessing tools with Python or ModelBuilder.

Question: How can I programmatically run geoprocessing tools?

Answer: All geoprocessing tools can be run in Python using the arcpy site package. Additionally, the geoprocessing API in the ArcGIS Pro .NET SDK provides the method ExecuteToolAsync. This method allows you to run any system or custom geoprocessing tool by specifying its name and location, parameters, and environments

### Mapping API

Question: How can I navigate from a feature layer to an underlying feature class?

Answer: Use the ```GetTable``` method on the feature layer.

```C#

public abstract class BasicFeatureLayer : Layer {
		public ArcGIS.Core.Data.Table GetTable();
                ...

public class FeatureLayer : BasicFeatureLayer {
   ...
}

var featureClass = featureLayer.GetTable() as ArcGIS.Core.Data.FeatureClass;
```

Question: Can I add layers to my project based on service URLs or base map URLs?

Answer: Yes. Use the static ```Layer.Create``` method with the URL to the service or base map. For more information, consult the **API Reference Guide** for the ArcGIS Pro .NET SDK.

#### Python

Question: Will the Python script tools I’ve built to use in ArcMap be supported in ArcGIS Pro?

Answer: Yes. Your Python scripts can be used in many cases with few or no changes. The only exception is if your script uses the arcpy.mapping module, which is not available in Pro and has been replaced by arcpy.mp. You can use a new geoprocessing tool, "**_Analyze Tools For Pro_**", to get a log of all required changes as well as a  Python 3 syntax guide.

Question: Will Python add-ins be supported in ArcGIS Pro?

Answer: No. Add-ins in ArcGIS Pro must be authored using .NET.

Question: Can I run Python scripts using the .NET SDK?

Answer: Yes, but first your Python script must be built as a script tool in a toolbox. Python script tools can be used and run just like all other geoprocessing tools, including being run from the .NET SDK using the Geoprocessing API ```ExecuteToolAsync``` method.

Question: Why does ArcGIS Pro use Python 3?

Python 3 has been in active development for over five years, coexisting with Python 2 as people have slowly ported to the new version. The Python 3.x series is the future. Most major third-party libraries (including all of our core dependencies) have been ported to Python 3. In the Python community, the current consensus is all new development should be done in Python 3.x; 2.x is just for legacy projects. In moving to Python 3, ArcGIS is in a position to use new, modern Python libraries and tools in the version that will be continually supported by the Python community.

### Migration from 10x

Question: Is the ArcGIS Pro SDK a replacement for ArcObjects?

Answer: No. There will continue to be an ArcObjects SDK for customizing and extending ArcMap, ArcCatalog, and ArcScene. This new SDK is only for the new ArcGIS Pro application.

Question: Will my Arcobjects code work in ArcGIS Pro?

Answer: No. ArcObjects will not be exposed in ArcGIS Pro. ArcObjects code must be rewritten to take advantage of the new classes and features in Pro.

Question: Will geodatabase extensions (class and workspace extensions) be supported in Pro?

Answer: No. Since ArcObjects will not be exposed in ArcGIS Pro, class and workspace extensions will not be supported. The ArcGIS Pro SDK includes a geodatabase API that exposes the necessary datasets and functionality for sophisticated analytical applications against the geodatabase.

Question: Will plug-in data sources be supported in Pro?

Answer: Not at 1.1. It is planned for 1.2.

Question: Will custom ArcObjects raster functions be supported in Pro?

Answer: No. Another mechanism, independent of ArcObjects, is being determined.

Question: Are add-ins created with the ArcGIS Pro .NET SDK deployed using the same approach as previous releases of desktop products; for example, ArcMap, ArcCatalog, ArcScene, and so on.

Answer: Yes. Add-ins follow the same deployment model as was used at 10x, even though the underlying framework is different. An add-in package (".esriAddInX") is deployed to a well-known folder. By default, this is C:\\Users\\\<Username\>\\Documents\\ArcGIS\\AddIns\\ArcGISPro1.0. There is an add-in manager utility for ArcGIS Pro the same as there is for 10x.

### Geodatabase API

Question: Will the geodatabase API support geometries with m- and z-values?

Answer: Yes. This includes support for spatial references with z- and m-resolution and domains.

Question: Are DDL operations supported through the geodatabase API for ArcGIS Pro?

Answer: No, only DML operations are supported. DDL operations must be implemented via geoprocessing and Python.

Question: How do you access a local file geodatabase?

Answer: There are classes in the ArcGIS Pro geodatabase API to connect to a local file geodatabase and open it. Example below:

```C#
   Geodatabase geodatabase = new      Geodatabase(“C:\\path\\to\\localgdb\\File.gdb”);
   //Also, you can open a FeatureClass named “FC” like this:
   FeatureClass featureClass =   geodatabase.OpenDataset<FeatureClass>(“FC”);

```

### ArcGIS Pro SDK templates

Question: How do you fix the broken references in your ArcGIS Pro add-in solution?

Answer: ArcGIS Pro assemblies are not stored in a GAC. They are referenced via file paths in the add-in's .csproj or .vbproj file. This means that if you share ArcGIS Pro add-ins as Visual Studio projects, your assembly references may be broken if you open a project created by another colleague or user. For example, some users may install ArcGIS Pro to the C: drive while others install to the D: drive.

To fix the broken ArcGIS Pro references in an add-in, use the Fix ArcGIS Pro Reference utility included in the proapp-sdk-utilities.vsix. The steps below explain how to use the tool.

1.  Install the ArcGIS Pro SDK Utilities vsix package (proapp-sdk-utilities.vsix).  
2.  Open the add-in project that has broken references.  
3.  Right-click the add-in project in the Visual Studio Solution Explorer and click **_Pro Fix References_**.  
4.  Click Yes on the dialog box that asks you to proceed. Your broken references will now be fixed.  

Question: I changed the namespace in my ArcGIS Pro add-in project. Now ArcGIS Pro crashes when I attempt to load the add-in.

Answer: The Config.daml file also has references to the namespace used in the ArcGIS Pro add-in solution. It is not sufficient to only change the namespace in your class files. The Config.daml file namespace references also need to be changed, even though there will be no compilation errors if they are different.

