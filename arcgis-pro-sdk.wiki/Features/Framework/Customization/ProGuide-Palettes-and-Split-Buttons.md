```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

This ProGuide explains the step by step process on how to declare palettes and split buttons. However this is for reference only. **The recommended approach is to run the [palette and split button item templates](home#arcgis-pro-sdk-templates).** 

#####In this topic

[How to declare a tool palette](#how-to-declare-a-tool-palette)<br>
[How to declare a button palette](#how-to-declare-a-button-palette)<br>
[How to declare a split button](#how-to-declare-a-split-button)<br>

***

###How to declare a tool palette
Tool palettes are ostensibly menus, but designed specifically for grouping a set of related tools together. When a tool is selected from the palette of tools, it becomes the active tool and displays on the collapsed palette.

Tool palettes are defined at the same level as controls in the DAML file.

```xml
<groups>
   <group id="sdk_exampleToolPalette" caption="Example Tool Palette" >
      <toolPalette refID="exampleToolPalette" size="middle" />
   </group>
</groups>
<controls>
   <tool id="tool1" className="Tool1" caption="Tool 1" condition="esri_mapping_mapPane"/>
   <tool id="tool2" className="Tool2" caption="Tool 2" condition="esri_mapping_mapPane"/>
</controls>
<palettes>
   <toolPalette id="exampleToolPalette" caption="Example" showItemCaption="true" itemsInRow="1">
      <tool refID="tool1"/>
      <tool refID="tool2"/>
   </toolPalette>
</palettes> 
```

The first tool listed in the palette is the initial tool displayed in the panel.

###How to declare a button palette
Similar to tool palettes, button palettes are designed for grouping a set of related buttons together. When a button is selected from the palette, it is executed and displays on the collapsed palette.

Button palettes are defined at the same level as controls in the DAML file.

```xml
<groups>
   <group id="esri_mapping_syncViewGroup" caption="Link">
      <buttonPalette refID="esri_mapping_syncViewExtentPalette" size="large" />
   </group>
</groups>
<controls>
   <button id="esri_mapping_syncCenterButton" className="Ribbon.SyncCenterButton" caption="Center" largeImage="pack://application:,,,/ArcGIS.Desktop.Resources;component/Images/SyncCenter32.png" loadOnClick="false" condition="esri_mapping_mapPane">
      <tooltip heading="">Link all views with the active view by centering on the same point. Scale does not change in the linked views.<disabledText></disabledText></tooltip>
   </button>
   <button id="esri_mapping_syncCenterAndScaleButton" className="Ribbon.SyncCenterAndScaleButton" caption="Center And Scale" largeImage="pack://application:,,,/ArcGIS.Desktop.Resources;component/Images/SyncExtent32.png" loadOnClick="false" condition="esri_mapping_mapPane">
      <tooltip heading="">Link all views with the active view by centering on the same point and displaying at the same scale. 

Rotation of the linked views dynamically changes to match the rotation of the active view.<disabledText></disabledText></tooltip>
   </button>
</controls>
<palettes>
   <buttonPalette id="esri_mapping_syncViewExtentPalette" caption="Link Views" extendedCaption="Open link views palette" keytip="LV" dropDown="false" menuStyle="false" itemsInRow="1" showItemCaption="false">
      <button refID="esri_mapping_syncCenterButton" />
      <button refID="esri_mapping_syncCenterAndScaleButton" />
   </buttonPalette>
</palettes> 
```

Set the **menuStyle** attribute to true, if you want your palette to appear as a menu.

###How to declare a split button
Similar to button palettes, split buttons group related buttons together. The associated group displays when the arrow portion of the split button is clicked. Once a control is selected from the drop-down list, it becomes the active control in the split button. Split buttons can contain either basic button controls or galleries.

As per button and tool palettes, split buttons are defined at the same level as controls in the DAML file.

```xml
<splitButtons>
   <splitButton id ="exampleSplitButton">
      <button refID="button1"/>
      <gallery refID="gallery1"/>
   </splitButton>
</splitButtons>
```
