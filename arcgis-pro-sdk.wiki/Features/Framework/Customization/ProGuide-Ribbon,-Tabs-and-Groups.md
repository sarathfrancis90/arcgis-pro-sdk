```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

#####In this topic

[Overview](#overview)<br>
[Groups](#groups)<br>
[How to add a popup to a group](#how-to-add-a-popup-to-a-group)<br>
[How to modify the contents of a group](#how-to-modify-the-contents-of-a-group)<br>
[Tabs](#tabs)<br>
[Tab groups](#tab-groups)<br>
[Toolbars](#toolbars)<br>
 
***

###Overview
ArcGIS Pro uses the ribbon paradigm popularized by newer versions of Microsoft Office. The ribbon consists of a single fixed toolbar containing one or more tabs. The number of available (visible) tabs may vary dynamically depending on the state of the application. Tabs are activated through user interaction (clicks), or when directed by code running within the application.

Each tab is composed of one or more groups, small rectangular regions having a caption, and containing one or more controls. The representation of controls within groups varies depending on how frequently the control is expected to be used, and is configured declaratively in the DAML customization file.  Frequently used controls should be large and obvious, while less frequently used tools should be smaller. In addition, controls are typically paired with a caption to make their function more obvious.

The location of a control within the ribbon, including its size and relative placement, is established using the group and tab elements. 

![sampleGroup](images/Ribbon Tabs and Groups/SampleGroup.png)

###Groups
Groups are declared as lists of controls within a _groups_ container element within the _module_ DAML element :

```xml
<groups>
   <group id="esri_core_DockWindows" caption="Windows">
      <button refID="esri_core_ShowProjectDockPane" size="large" />
   </group>
   <group id="esri_core_ProjectFile" caption="Project">
      <button refID="esri_core_OpenProjectButton" size="large" />
      <button refID="esri_core_NewProjectButton" size="middle"/>
      <button refID="esri_core_SaveProjectButton" size="middle"/>
      <button refID="esri_core_SaveProjectAsButton" size="middle"/>
      <button refID="esri_core_CloseProjectButton" size="middle"/>
      <button refID="esri_core_ProjectSettingsButton" size="middle"/>
   </group>
   <group id="esri_core_ProjectImport" caption="Import">
      <button refID="esri_core_ImportMapButton" size="large" />
   </group>
   <group id="esri_core_ProjectNewItem" caption="New">
      <button refID="esri_core_NewMapButton" size="large" />
   </group>
</groups>
```

In the example above, four groups are declared. Note that the button elements refer to previously declared buttons. While the button image is supplied elsewhere in the button declaration, the size of the button is specified on the group. Note that the same button could appear in more than one group. Any control can be added to the group; label controls, checkboxes, editboxes, and so on.  The **separator** attribute can be used to segregate multiple controls in the same group.  

```xml
<group id="esri_core_ProjectFile" caption="Project">
   <button refID="esri_core_OpenProjectButton" size="large" />
   <button refID="esri_core_NewProjectButton" size="middle"/>
   <button refID="esri_core_SaveProjectButton" size="middle"/>
   <button refID="esri_core_SaveProjectAsButton" size="middle" separator="true"/>
   <button refID="esri_core_CloseProjectButton" size="middle"/>
   <button refID="esri_core_ProjectSettingsButton" size="middle"/>
</group>
```

The visibility of groups cannot be controlled independently from their parent tab; that is, groups do not appear, disappear, or move on their parent tab while the application is running. This restriction keeps the GUI's surface familiar as the user interacts with the application.

###How to add a popup to a group
Groups may optionally support a “more” button—a small link widget located next to the group caption—that is used to show a dialog box where more obscure functions may be accessed. Use the **launcherButtonID** attribute on the group element to reference a button function.

```xml
<group id="esri_mapping_navigateGroup" caption="Navigate" launcherButtonID="esri_mapping_navigationOptionsButton" smallImage="pack://application:,,,/ArcGIS.Desktop.Resources;component/Images/3DNavigationTool16.png">
   <tool refID="esri_mapping_exploreSplitButton" size="large" />
   <button refID="esri_mapping_zoomFullButton" size="small" />
   <button refID="esri_mapping_fixedZoomInButton" size="small" />
   <button refID="esri_mapping_prevExtentButton" size="small" />
   <button refID="esri_mapping_zoomToSelectionButton" size="small" />
   <button refID="esri_mapping_fixedZoomOutButton" size="small" />
   <button refID="esri_mapping_nextExtentButton" size="small" />
   <gallery refID="esri_mapping_bookmarksNavigateGallery" inline="false" size="large" />
</group>
```

![GroupWithPopup](images/Ribbon Tabs and Groups/GroupWithPopup.png)

###How to modify the contents of a group
Like controls, existing groups can be modified using DAML files. The following example removes a button from an existing group:

```xml
<updateModule refid="esrimappingExtension">
   <groups>
      <updateGroup refid=" ESRImappingFindGroup">
         <deleteButton refID=" ESRImappingFindButton"/>
      </updateGroup>
   </groups>
</updateModule>
```

###Tabs
Once a group is declared, it can be referenced and placed on tabs. Tabs are declared as lists of groups within a tab's collection element. In the following example, previously declared groups are added to a new tab labeled “Map”.

```xml
<tabs>
   <tab id="esri_core_MapTab" caption="Map" condition="esri_core_MapPane" keytip="M">
      <group refID="esri_core_NavigateGroup" />
      <group refID="esri_core_MapGroup" />
      <group refID="esri_core_ViewGroup" />
      <group refID="esri_core_SelectionGroup"/>
      <group refID="esri_core_InquiryGroup"/>
      <group refID="esri_core_MapWindowsGroup"/>
   </tab>
</tabs>
```

Like controls, a tab’s relevance can be governed using a condition. The **condition** attribute is used to control whether or not the tab is visible, and thus available. Conditions are not used to control tab activation, only tab availability. If no condition is specified, the tab is always visible.

To support accessibility, an appropriate **keytip** should be supplied for each tab. The characters chosen should make sense and should conflict with any existing keytips; keytips can consist of multiple characters to disambiguate if necessary.

###Tab groups
Collections of related tabs can be grouped together in colored, named groups to improve clarity. These collections are called tab groups. In the ribbon displayed below, there is a tab group containing three tabs called FEATURE LAYER. 

![TabGroup](images/Ribbon Tabs and Groups/TabGroup.png)

Tab groups are declared within a tabGroups collection element and consist of an **ID**, a **color**, and a **caption**. Tabs are associated with a tab group using the **tabGroupID** attribute. The DAML below creates the tabs and tab group as displayed above. 

```xml
<tabGroups>
   <tabGroup caption="FEATURE LAYER" id="esri_mapping_featureLayerTabGroup">
      <color A="255" R="238" G="170" B="90" />
      <borderColor A="0" R="251" G="226" B="195" />
   </tabGroup>
</tabGroups>
<tabs>
   <tab id="esri_mapping_featureLayerAppearanceTab" caption="APPEARANCE" condition="esri_mapping_onlyFeatureLayersSelectedCondition" tabGroupID="esri_mapping_featureLayerTabGroup">
      <group refID="esri_mapping_layerScaleVisibilityGroup"/>
      <group refID="esri_mapping_layerEffectsGroup" />
      <group refID="esri_mapping_layerSymbology" />
      <group refID="esri_mapping_layerExtrusion" />
      <group refID="esri_mapping_layer3DGroup" />
   </tab>
   <tab id="esri_mapping_labelingTab" caption="LABELING" condition="esri_mapping_onlyFeatureLayersSelectedCondition" tabGroupID="esri_mapping_featureLayerTabGroup">
      <group refID="esri_mapping_labelingLayerGroup" />
      <group refID="esri_mapping_labelingLabelClassGroup" />
      <group refID="esri_mapping_labelingScalesGroup" />
      <group refID="esri_mapping_labelingTextSymbolGroup" />
      <group refID="esri_mapping_labelingPlacementStyleGroup" />
      <group refID="esri_mapping_labelingMapGroup" />
   </tab>
   <tab id="esri_mapping_featureLayerDataTab" caption="DATA" condition="esri_mapping_onlyFeatureLayersSelectedCondition" tabGroupID="esri_mapping_featureLayerTabGroup">
      <group refID="esri_mapping_layerDefQueryGroup" />
      <group refID="esri_mapping_featureLayerAttributeGroup" />
      <group refID="esri_mapping_layerSelectionGroup"/>
      <group refID="esri_mapping_designViewGroup"/>
      <group refID="esri_mapping_layerRelationshipsGroup"/>
      <group refID="esri_mapping_layerToolsGroup"/>
   </tab>
</tabs>
```

Tab groups are typically used in situations where the user temporarily enters a mode, such as, editing graphic elements. Tab groups are not typically used with tabs that are perpetually visible (global tabs).

###Toolbars
The ribbon group supports an inner collection and grouping of buttons into a toolbar. Toolbars are purely declarative and support multiple groups. The group definition describes what the toolbar looks like. Use the appropriate control element with a refID to specify the elements you want to appear in the toolbar. The example below shows a toolbar with several comboboxes and buttons.

![Toolbar](images/Ribbon Tabs and Groups/Toolbar.png)

```xml
<toolbars>
   <toolbar id="esri_mapping_labelTextSymbolFontToolbar">
      <group>
         <comboBox refID="esri_mapping_labelTextSymbolFontFamilyComboBox" />
         <comboBox refID="esri_mapping_labelTextSymbolFontSizeComboBox" size="small" />
         <button refID="esri_mapping_labelTextSymbolIncreaseSizeButton" size="small" />
         <button refID="esri_mapping_labelTextSymbolDecreaseSizeButton" size="small" />
      </group>
      <group>
         <customControl refID="esri_mapping_labelTextSymbolColorPicker" size="small" />
         <button refID="esri_mapping_labelTextSymbolBoldButton" size="small" />
         <button refID="esri_mapping_labelTextSymbolItalicButton" size="small" />
         <button refID="esri_mapping_labelTextSymbolUnderlineButton" size="small" />
      </group>
   </toolbar>
</toolbars>
```
