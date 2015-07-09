#ProConcepts Framework
ArcGIS Pro is a highly configurable and extensible application. All software modifications and enhancements are achieved using add-ins. The add-in model provides a declaratively-based framework for creating a collection of customizations conveniently packaged within a single compressed file. Add-ins are easily shared as they do not require installation programs or registration; add-ins are added to a system by simply copying them to a well-known folder and removed by deleting them from this folder. Add-ins can also be shared between users within an organization using a centralized network share.  
 
Add-ins are authored using .NET along with Esri’s Desktop Application Markup Language (DAML). The DAML (an XML language created by Esri) describes the customizations; the .NET classes provide the custom behavior. ArcGIS Pro’s software development kit (SDK) includes an Add-In Wizard that integrates with Microsoft Visual Studio to simplify development.  

##Add-In Extensibility Points  

ArcGIS Pro’s add-in framework supports a host of extensibility points. The list below shows the most common customizations.  

* Ribbon
	* Tabs / Contextual Tabs
	* Groups
	* Controls
		* Buttons
		* Split Buttons
		* Button Palettes
		* Tools
		* Tool Palettes
		* Toolbars
		* Galleries
		* Combobox
		* Editbox
		* Menus
		* Dynamic Menus
		* Custom Controls
* Panes
* DockPanes
* Property Sheets / Pages
* Backstage tabs
* Component Categories

##Introduction to DAML (Desktop Application Markup Language)  

As mentioned earlier, add-ins have declarative and active aspects. The declarative portion of an add-in is defined within a DAML file containing a collection of framework elements (plug-ins) that describe the customizations in application terms. For example, an add-in might be comprised of a dock pane and a new tab with two groups and a collection of buttons. These DAML elements also describe the static aspects of the components including their captions, tooltips, images, and layout (position) details. The declarative portion also contains information needed by the framework to activate (create) the associated objects when appropriate. This portioning into declarative and programmatic aspects increases flexibility, simplifies the deployment and distribution of customizations, decreases the amount of the code that needs to be written, and better leverages the just-in-time (JIT) strategy for intelligent activation and resource utilization.  

The DAML below shows the beginnings of a new add-in; in this case, a new Tab with a single Button is added to the application.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<ArcGIS defaultAssembly="Acme.dll"
        defaultNamespace="Acme"
        xmlns="http://schemas.esri.com/DADF/Registry"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://schemas.esri.com/DADF/Registry file:///C:/arcgis/bin/ArcGIS.Desktop.Framework.xsd">

  <AddInInfo id="{3329a7d3-9f16-4642-9a70-475b421c77b5}" 
             version="1.0" desktopVersion="1.1.2829">
    <Name>Acme</Name>
    <Description>Acme Extension</Description>
    <Image>Images\AddinDesktop32.png</Image>
    <Company>Acme</Company>
    <Date>5/28/2015 10:28:50 AM, 2015</Date>
  </AddInInfo>

  <modules>
    <insertModule id="acme_MainModule" className="MainModule" 
                  autoLoad="false" caption="Acme">
      <tabs>
        <tab id="acme_MainTab" caption="Acme Tools">
          <group refID="acme_mainGroup"/>
        </tab>
      </tabs>
      <groups>
        <group id="acme_mainGroup" caption="Tools">
          <button refID="acme_FullExtent" size="large" />
        </group>
      </groups>
      <controls>
        <button id="acme_FullExtent" caption="FullExtent "
                className="FullExtent" loadOnClick="true"
                smallImage="Images\FullExtent16.png" 
                largeImage="Images\FullExtent32.png"
                condition="esri_mapping_mapPane">
          <tooltip heading="Full Extent">
            Displays the current map at its full extent.<disabledText />
          </tooltip>
        </button>
      </controls>
    </insertModule>
  </modules>
</ArcGIS>
```
Note, DAML elements within one file can alter or remove elements in other files. For example, an add-in may simply inject a single button into a ribbon group defined by another add-in. The example below shows a Button being added into an existing Tab beside a specific control.  

```xml
<updateModule refID="esri_mapping">
  <groups>
    <updateGroup refID="esri_mapping_navigateGroup">
      <insertButton refID="acme_FullExtent" 
                    insert="before" 
                    placeWith="esri_mapping_zoomFullButton" 
                    separator="true"/>
    </updateGroup>
  </groups>
</updateModule>
All DAML elements must fall under the root ArcGIS element. The first child node is the AddInInfo element which holds the metadata about the add-in itself. This includes a unique GUID identifier, its version, a name, and description.
  <AddInInfo id="{3329a7d3-9f16-4642-9a70-475b421c77b5}" 
             version="1.0" desktopVersion="1.1.2829">
    <Name>Acme</Name>
    <Description>Acme Extension</Description>
    <Image>Images\AddinDesktop32.png</Image>
    <Company>Acme</Company>
    <Date>5/28/2015 10:28:50 AM, 2015</Date>
  </AddInInfo>
```

Customizations follow and these are broken up in several main classifications including modules, categories, conditions, propertySheets, backstage, and dropHandlers. As we’ll see, most customizations appear under the modules element.  

All root nodes perform one or more of three distinct actions: inserts, updates, and deletes. The type of operation is determined by the element name. For example, a new module is declared using the insertModule element; similarly, a module is updated using the updateModule element. Note, in cases where no other operation except inserts is valid, the insert prefix has been dropped.  

```xml
  <modules>
    <insertModule id="acme_mainModule" caption="Acme" 
                  className="MainModule" autoLoad="false">
      ...
    </insertModule>
  </modules>
```

Each type of customization must be assigned a unique ID by the author. IDs are established when the element is inserted; customizations are referenced by their ID when they are being updated or deleted.  

In the excerpt below, the caption of a previously inserted button is updated using the updateButton element. Note that the id attribute is used when declaring new objects, while the refID attribute is used to reference existing elements.  

```xml
     <updateButton refID="esri_SubSystem_Button1" caption="New Caption"/>
```

Similarly, a button would be deleted using deleteButton.  

```xml
  <deleteButton refID="esri_SubSystem_Button1"/>
```

DAML instructions (insertions, updates, and deletes) are processed from a variety of sources and combined by the framework at run-time into a single in-memory representation for all customizations submitted by all parties without altering any of the original DAML files.  

##Plug-Ins

Some customizations, like menus, are purely declarative—their definition in DAML is all that is necessary for the Framework to create and present them. Most customizations, however, have an active (code-behind) component and most of these inherit from the common base class PlugIn.  

```C#
  public abstract class PlugIn : PropertyChangedBase
  {
    public string Caption { get; set; }
    public string DisabledTooltip { get; set; }
    public bool Enabled { get; set; }
    protected internal string ID { get; }
    public object LargeImage { get; set; }
    public object SmallImage { get; set; }
    public string Tooltip { get; set; }
    public string TooltipHeading { get; set; }

    protected internal virtual void OnUpdate();
  }
```

Note that many of the methods and properties on PlugIn and its derived classes do not need to be overridden or implemented by the developers; for instance, the implementation of the Caption property—found on many plug-ins—is provided by the framework and will return whatever caption was supplied when the plug-in was declared using DAML.  Only the protected virtual overrides where a specific behavior is required—such as OnClick—need be supplied by the developer.  

As previously mentioned, all plug-ins require an alphanumeric identifier (ID).  This ID is specified within the plug-in DAML when the plug-in is declared, and is conceptually similar to the GUID used to uniquely name COM CoClasses.  

All plug-ins with an active component use the class and assembly attributes to connect the DAML to the managed code. The class name is the full class name which includes the namespace. The assembly is expected to reside in the same folder as the DAML file so do not provide a path. Note, the root ArcGIS node has the defaultNamespace and defaultAssembly attributes to mitigate unnecessarily repeating this information throughout the DAML.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<ArcGIS defaultAssembly="Acme.dll"
        defaultNamespace="Acme"
```

If your plug-in belongs to a different namespace, specify it as part of the class name. If the namespace is part of the default namespace, provide only the missing portion. In the example below the FullExtentButton class is added to the Acme.Controls namespace; since the default name is just Acme, the DAML class entry needs to read Controls.FullExtentButton.  

```C#
namespace Acme.Controls
{
  sealed class FullExtentButton : Button
  {
  }
}
```
```xml
<button id="acme_FullExtent" caption="Full Extent" className="Controls.FullExtentButton" 
        loadOnClick="true" 
        smallImage="Images\GenericButtonBlack16.png" 
        largeImage="Images\GenericButtonBlack32.png">
</button>
```

##Modules  

Modules act as the hub and central access point for their subsystem; if you need access to the functionality within a subsystem, you start with the Module. Modules are singletons that are instantiated automatically by the framework when access is explicitly requested in executing code, or when the Module becomes “relevant” due a context shift.  All program elements that are part of the Module are explicitly declared as such; these elements include Ribbon Buttons, Tools, Galleries, Combo Boxes, Edit Boxes, Palettes, and other controls, as well as application Panes and Docking Panes.  The well-defined relationship between a Module and its associated components lets the application initialize, un-initialize, and remove entire subsystems as a whole.  

Most of the logic in the UI level should reside in a module or a helper (non-UI) class the module directly manages. For example, when writing a button, the button class itself should have no business logic in it; all the logic should be centralized in its parent module. Centralizing the business logic reduces the spaghetti-effect. For example, instead of several buttons each listening to a particular event, it’s better to have one module listen to the event and each button instead polls the module in OnUpdate.  

Modules also support several patterns to make centralizing business logic much easier; for instance, modules automatically load whenever one of their plug-ins load.  For example, in most cases a button on the ribbon doesn’t load until it is clicked. When this happens, the button’s parent module is also loaded; similarly, when a dock pane loads, its parent module also loads.  
Modules also have a pattern for working with panes. Modules are automatically notified whenever one of its panes are activated, deactivated, opened, or closed. This means your module doesn’t have to listen to the Framework’s ActivePaneChanged event and filter for the relevant panes; instead, modules are given direct notification that one of their panes changed.

##Declaring Modules in DAML

Modules are declared within the root ArcGIS element, but must be further enclosed within a modules container element.  The autoLoad attribute is used to control whether the module is loaded just-in-time (JIT)—the default—or loaded automatically when the application starts.  In almost all cases, autoLoad should be set to false.  

```xml
<modules>
  <insertModule id="acme_mainModule" caption="Acme" 
                className="MainModule" autoLoad="false">
    <!--Declare additional customizations here..-->
  </insertModule>
</modules>
```
Note that if declaring a new module, all constituent plug-in declarations contained within the insertModule element are implicitly inserts, and so the insert prefix on element names can be omitted; e.g., insertButton becomes simply button.  

##Controls

Controls include any of the simple widgets that can appear on the application Ribbon, including: Button, Tool, CheckBox, ComboBox, EditBox, LabelControl, DynamicMenu, and CustomControl.  All simple ribbon controls are declared in a module’s controls element. Although controls are declared in a controls section, they are referenced when defining a group. This allows the same control to appear in multiple groups without having to re-declare the entire control.  

The following declaration includes a few of the most common attributes associated with buttons:  

```xml
<insertModule id="acme_MainModule" className="AcmeMain" caption="Acme">
  <controls>
    <button id="acme_AddFromFileButton"
            className=" AddFromFile"
            caption="From File"
            largeImage="Images\AddFromFile32.png"
            smallImage="Images\AddFromFile16.pngg">
      <tooltip heading="Add from file" image="Images\AddFromFile16.png">
        Add spatial data files to the project
        <disabledText>Requires an open project with a map</disabledText>
      </tooltip>
    </button>
  </controls>
</insertModule>
```

As previously stated, framework plug-ins are not permitted to exist un-tethered, and must be linked with a module.  In the case illustrated above, a new button—made by ACME—is declared and added to a module. This logically links the button to the ACME Module.  If at some point, the framework determines that the module should be unloaded, the newly added button will also be unloaded.   

Existing controls can also be updated or deleted using a plug-in configuration file, but note that not all attributes can be updated: those that might alter the run-time behavior of the plug-in such as class, assembly, and condition, can only be set through an insert.  

The following example updates the caption on the button added in the example above.  

```xml
<updateModule refID="acme_MainModule">
  <controls>
    <updateButton refID=" acme_mapping_AddFromFileButton" caption="New Caption"/>
  </controls>
</updateModule>
```
All control elements share several attributes.  The loadOnClick attribute determines when the button should be created by the framework. By default, controls appear enabled, but are not actually instantiated until they are clicked. This simple JIT strategy improves resource utilization and startup time by deferring the instantiation of controls until they are initiated by the end user. Note that non-visible controls are never loaded until they become visible (or are executed programmatically), regardless of the value assigned to loadOnClick.  

Tooltips are defined using the tooltip sub-element and may span as many lines as necessary. The image attribute is used to supply an image that will appear next to the tip text. Command tooltips also support a disabledText element; this string becomes the tooltip whenever the command is disabled.  

Most controls support multiple sizes in the ribbon. For example, a button can render small (small icon only), middle (small icon with text), and large (large icon over text). Use the smallImage and largeImage attributes to specify unique images for the different sizes. Images don’t have to be graphics, you can also use XAML. Note, images are not flipped when running right-to-left; if the image should flip when running in this mode, e.g. arrow buttons, set the flipImageRTL attribute to true. The graphic below shows three buttons (in blue squares) in the three supported sizes.  

![Ribbon1.png](../images/Proconcepts-Frameworks/Ribbon1.png "Framework_Guide Ribbon1.png")  
