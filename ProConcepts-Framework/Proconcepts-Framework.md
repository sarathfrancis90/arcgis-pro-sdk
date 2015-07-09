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

