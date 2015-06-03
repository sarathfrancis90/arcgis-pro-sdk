```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          01\20\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

This ProGuide explains the step by step process on how to declare a dockpane. However this is for reference only. **The recommended approach is to run the [dockpane item template](home#arcgis-pro-sdk-templates).** 

#####In this topic

* [How to declare a dockpane](#how-to-declare-a-dockpane)<br>
* [Find a dockpane](#find-a-dockpane)<br>
* [Customize initial position](#customize-initial-position)<br>
* [Customize loading message](#customize-loading-message)<br>
* [Support drag and drop](#support-drag-and-drop)<br>

###How to declare a dockpane
Dockpanes are modeless dialogs that can be docked at the top, right, left, or bottom of the view area within the application. Dockpanes can also be grouped with other dockpanes and docked relative to each other (that is, below, above, and so on), and can also be undocked and floated. Finally, dockpanes can also be pinned or unpinned so that they slide back into the frame to save space.  For each dockpane — each a logical singleton — the framework persists and preserves the docking state so that when shown in subsequent sessions, dockpanes appear in the same positions and in the same state.

The example below shows the declaration of a dockpane. This dockpane specifies a content element that has only a className attribute. When the framework creates the dock pane, it also creates its content class and will set the content’s data context to the dockpane. This automatic binding allows you to use the MVVM pattern when developing dockpanes. For example, a well-designed dockpane will have as little logic as possible in its XAML code behind; instead, all of the controls in the XAML should be bound to properties in the dockpane base class. The dockpane bases classes should be regarded as view-models.

```xml
<dockPanes>
   <dockPane id="esri_core_TOCDockPane" caption="Contents" className="TOC.TOCDockPaneViewModel" dock="group" visible="true" condition="esri_core_MapPane" dockWith="esri_core_ProjectDockPane">
      <content className="TOC.TOCDockPane"/>
   </dockPane>
</dockPanes>
```

Use the **initiallyVisible** attribute to determine whether your dockpane is visible on the first startup of ArcGIS Pro. Remember, the docking state is preserved for subsequent ArcGIS Pro sessions; so, if your pane has become visible in a session, it will be visible on the next session regardless of the initiallyVisible attribute. Dockpanes can also be established with a **condition** so that their content only appear when the condition is satisfied. If the dockpane is visible and the condition is not met, use the **delayLoadMessage** attribute to display a message in the dockpane UI.

```xml
<dockPanes>
   <dockPane id="esri_mapping_symbologyDockPane" caption="Symbology" className="ArcGIS.Desktop.Internal.Mapping.Symbology.SymbologyDockPaneContentProvider" dock="group" dockWith="esri_core_projectDockPane" initiallyVisible="false" condition="esri_mapping_mapPane" showLoadingMessage="true" delayLoadMessage="Select a layer in the contents pane”>
      <content className="TOC.TOCDockPane"/>
   </dockPane>
</dockPanes>
```

###Find a dockpane
Use the following code to find a dockpane (you need the DAML ID):
```c#
var pane = FrameworkApplication.FindDockPane("esri_core_ProjectDockPane");
```

###Customize initial position
Use the **dock**, **dockWith**, **autoHide** attributes to set the initial docking state of your dockpane. Dock allows values of left, right, top, bottom, float, or group. Use the **dockWith** attribute to specify a dockpane that yours is to be positioned relative to. 

####Dock to the left
```xml
<dockPanes>
   <dockPane id="esri_core_TOCDockPane" caption="Contents" className="TOC.TOCDockPaneViewModel" dock="left" initiallyVisible="true" condition="esri_core_MapPane">
      <content className="TOC.TOCDockPane"/>
   </dockPane>
</dockPanes>
```

####Float
```xml
<dockPanes>
   <dockPane id="esri_core_TOCDockPane" caption="Contents" className="TOC.TOCDockPaneViewModel" dock="float" initiallyVisible ="true" condition="esri_core_MapPane">
      <content className="TOC.TOCDockPane"/>
   </dockPane>
</dockPanes>
```

####Dock with project dockpane
```xml
<dockPanes>
   <dockPane id="esri_core_TOCDockPane" caption="Contents" className="TOC.TOCDockPaneViewModel" dock="group" visible="true" condition="esri_core_MapPane" dockWith="esri_core_ProjectDockPane">
      <content className="TOC.TOCDockPane"/>
   </dockPane>
</dockPanes>
```

###Customize loading message
During initialization, the framework provides a circular progressor that draws on top of the dockpane window. By default, the progressor displays the loading message as Loading _Caption_. The loading message can be optionally set in DAML using the **loadingMessage** attribute and updated at runtime. You can also show the progressor without a message by setting the **showLoadingMessage** attribute to false. 

###Support drag and drop
Dockpanes have the option of allowing the drop target to be either the entire dockable window or a specific control such as treeview or listview on the dockable window.

####Allow drop to entire dockable window
To allow dropping data to the entire dockable window, set the DAML attribute **isDropTarget** to true. 
```xml
<dockPane id="TOC" caption="Contents" className="DockPanes.TOCContents" dock="left" initiallyVisible="true" isDropTarget="true">
```
The dockpane’s view model also needs to override the **OnDragOver** and **OnDrop** methods of the base ArcGIS.Desktop.Framework.Controls.DockPane class.

####Only allow drop to a specific control on a dockable window
By default, the **isDropTarget** attribute is set to false, meaning the dockpane should specify a UI element in XAML as the drop target. In the example below, the listbox handles drop:    

```xml          
<UserControl x:Class="ArcGIS.Desktop.Mapping.TOC.TOCDockPane"
xmlns:dragDrop="clr-namespace:ArcGIS.Desktop.Framework.DragDrop;assembly=ArcGIS.Desktop.Framework" >
……….
   <Style x:Key="bookmarkListBoxStyle" TargetType="{x:Type ListBox}">
      <Setter Property="dragDrop:DragDrop.IsDragSource" Value="True" />
      <Setter Property="dragDrop:DragDrop.IsDropTarget" Value="True" />
      <Setter Property="dragDrop:DragDrop.DropHandler" Value="{Binding}" />
      <Setter Property="dragDrop:DragDrop.DragHandler" Value="{Binding}" />
    </Style>
```

Since the drop handler binds to the dockpane’s view model, the view model needs to override the **OnDragOver** and **OnDrop** methods of the base ArcGIS.Desktop.Framework.Controls.DockPane class.

However in some cases, you may want to set the drop handler to a different object other than the dockpane’s view model. 

```xml
<TreeView x:Name="TOC" Grid.Row="0" ItemsSource="{Binding Maps}" 
              dragDrop:DragDrop.IsDropTarget="True"
              dragDrop:DragDrop.DropHandler="{Binding DropHandler}" 
              dragDrop:DragDrop.DragHandler="{Binding DropHandler}"
              >
</TreeView>
```

In this case, the DropHandler function should return an instance of DropHandlerBase, which has **OnDragOver** and **OnDrop** methods.
```c#
internal class TOCMapViewDropHandler : DropHandlerBase,  IDragSource
{
   public override void OnDragOver(DropInfo dropInfo)
   {
      // TODO - add specific code here 
   }
   public override void OnDrop(DropData dropData)
   {   
      // TODO - add specific code here 
   }
}

_tocDropHandler = new TOCMapViewDropHandler(this);

public TOCMapViewDropHandler DropHandler
{
   get { return _tocDropHandler; }      // Instance of DropHandlerBase 
}
```