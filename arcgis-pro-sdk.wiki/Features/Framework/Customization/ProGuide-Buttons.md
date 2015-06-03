```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

This ProGuide explains the step by step process on how to declare a button. However this is for reference only. **The recommended approach is to run the [button item template](home#arcgis-pro-sdk-templates).** 

#####In this topic

[How to declare a button](#how-to-declare-a-button)<br>
[Control the enabled state of a button using a condition](#control-the-enabled-state-of-a-button-using-a-condition)<br>
[Show custom text on a tooltip when the button is disabled](#show-custom-text-on-a-tooltip-when-the-button-is-disabled)<br>
[Set the extended caption for a button](#set-the-extended-caption-for-a-button)<br>
[Add a key tip to a button](#add-a-key-tip-to-a-button)<br>
[Preload a button](#preload-a-button)<br>
[Use a static module method as a button class definition](#use-a-static-module-method-as-a-button-class-definition)<br>
[Change a button caption](#change-a-button-caption)<br>
[Change a button condition](#change-a-button-condition)<br>
[Remove a button](#remove-a-button)<br>
[Position a button into a group owned by a different module](#position-a-button-into-a-group-owned-by-a-different-module)<br>
[Access a button at runtime](#access-a-button-at-runtime)<br>
[Execute a button at runtime](#execute-a-button-at-runtime)<br>
[Create a checked button](#create-a-checked-button)<br>

***

###How to declare a button
Buttons are the least complex of all controls and are declared in DAML using the _button_ element within the _controls_ container. 

A button is declared using the **id**, **caption**, **className** and **tooltip** attributes. You can also specify the **largeImage** and **smallImage** attributes if your button is to have an image associated with it.

The button is referenced on a group using the _button_ control with the **refID** attribute.  
```xml
<groups>
   <group id="MyGroup" caption="Group 1">
      <button refID="MyCustomButton" size="large" />
   </group>
</groups>
<controls>
   <button id="MyCustomButton" caption="My Custom Button" className="MyCustomButton" largeImage="Images\GenericButtonBlue32.png" smallImage="Images\GenericButtonBlue16.png">
      <tooltip heading="Custom Button"><disabledText /></tooltip>
   </button>
</controls>
```
The button implementation is specified with the **className** attribute. The MyCustomButton class specified above inherits from the **ArcGIS.Desktop.Framework.Contracts.Button** base class. Override the virtual methods **OnClick** and **OnUpdate** to perform whatever custom behavior is desired.
```c#
internal class MyCustomButton : ArcGIS.Desktop.Framework.Contracts.Button
{
   protected override void OnClick()
   {
      // TODO - add specific code here 
   }

   protected override void OnUpdate()
   {
      // TODO - add specific code here to control enabled state
   }
}
```
 
###Control the enabled state of a button using a condition
In most instances, the enabled state of a button relies upon certain conditions within the application. For example, the Add Data button is only enabled when a map pane is opened. Use the **condition** attribute to assign a predefined condition. For more information about defining states and conditions within DAML, see that section. 
```xml
<button id="MyCustomButton" caption="My Custom Button" className="MyCustomButton" largeImage="Images\GenericButtonBlue32.png" smallImage="Images\GenericButtonBlue16.png" condition="esri_mapping_mapPane">
   <tooltip heading="Custom Button"><disabledText /></tooltip>
</button>
```

###Show custom text on a tooltip when the button is disabled
In addition to controlling the enabled state of a button, you can also add a custom message on the tooltip to explain why the button is disabled. This is achieved using the **disabledText** attribute of the button's tooltip.
```xml
<button id="esri_workflow_addTaskFileContextMenuButton" className="workflow_module:AddTaskFile" caption="Workflow Manager" condition="esri_workflow_canAddTaskFileCondition">
   <tooltip>
            Add the task item to the Workflow Manager repository.
      <disabledText>Not connected to a Workflow repository, or the current user does not have privileges to add tasks to the Workflow Manager repository.</disabledText>
   </tooltip>
</button>
```
 
![disabled_tool_tooltip](images/How to Work with Buttons/disabled_tool_tooltip.png)

###Set the extended caption for a button
Many captions are ambiguous and duplicated many times, for example, FORMAT. To resolve this, use the **extendedCaption** attribute on controls to provide more details. You do not need to provide an extended caption if there are no name collisions and the meaning of the control is clear based on the regular caption.
The **extendedCaption** appears alongside the caption—in parentheses—in the Customize and Task dialog boxes.
 
![extended_caption](images/How to Work with Buttons/extended_caption.png)

###Add a key tip to a button
The ribbon control allows keyboard access to ribbon tabs, groups, and items by means of key tips. Using the ALT key, you can see the key tips associated with various tabs and buttons. 

Key tips for the application (using ALT):
![keytips](images/How to Work with Buttons/keytips.png)

Key tips for the View tab (using ALT+V):<br>
![keytips_ViewTab](images/How to Work with Buttons/keytips_ViewTab.png)

Use the **keyTip** attribute to add a key tip to your custom button (if required).  Ensure that the key tip chosen is unique for the tab on which it appears. 
```xml
<button id="MyCustomButton" caption="My Custom Button" className="MyCustomButton" largeImage="Images\GenericButtonBlue32.png" smallImage="Images\GenericButtonBlue16.png" keytip="CB">
   <tooltip heading="Custom Button"><disabledText /></tooltip>
</button>
```

###Preload a button
The **loadOnClick** attribute determines when the button should be created by the framework. By default, controls appear enabled, but are not actually instantiated until they are clicked. This JIT strategy improves resource utilization and startup time by deferring the instantiation of controls until they are initiated by the end user. The default value of the **loadOnClick** attribute is true. Change this attribute to false to preload the button.

###Use a static module method as a button class definition
By default, the **className** attribute links to a complete button plug-in class (that inherits from the base button class). However, there is another pattern that can be used for simplifying the creation of buttons in the ribbon. This uses the concept of DelegateCommands that the module class supports. Instead of linking to the button plug-in class, you can instead declare that your button is just a function inside your module. This is achieved by referencing a static method on the module.  
```xml
<button id="MyCustomButton" caption="MyCustomButton " className="MyModule:OnMyCustomButtonClick" smallImage="Images\GenericButtonBlue16.png" largeImage="Images\GenericButtonBlue32.png" condition="esri_mapping_mapPane">
   <tooltip heading="Tooltip Heading">Tooltip text<disabledText /></tooltip>
</button>
```

DelegateCommands also support OnUpdate via a naming convention. If your function is called OnMyCustomButtonClick, you can add CanOnMyCustomButtonClick for your update logic. Note that these kinds of command response methods should normally be private or internal.  

Note that this DelegateCommand pattern is only suitable for buttons that appear on the ribbon.

###Change a button caption
Update operations can be achieved using the update prefix. In the example below, the caption of a previously inserted button is updated using the _updateButton_ element. Note also the **refID** attribute that is used to reference existing objects. Updates should exist within an _updateModule_ element, referencing the appropriate module to which the button belongs. 

```xml
<updateModule refID="acme_MainModule">
   <controls>
      <updateButton refID="esri_SubSystem_Button1" caption="New Caption"/>
   </controls>
</updateModule>
```

###Change a button condition
Not all attributes can be updated. Altering a button's condition is not possible.  Conditions can only be set through an insert DAML declaration.

###Remove a button
Delete operations can be achieved using the delete prefix. In the example below, a previously inserted button is removed from the UI using the _deleteButton_ element. Note also the **refID** attribute that is used to reference existing objects.

```xml
<updateModule refID="acme_MainModule">
   <controls>
      <deleteButton refID="esri_SubSystem_Button1"/>
   </controls>
</updateModule>
```

###Position a button into a group owned by a different module
Use the _updateGroup_ and _insertButton_ elements to add a button into a group owned by a different module; as this is an update operation your DAML needs with an existing _updateModule_ element, referencing the appropriate module to which the group belongs. The **placeWith** and **insert** attributes allow you to position your item relative to another within the group. 

```xml
<modules>
   <updateModule refID="esri_mapping">
      <groups>
         <updateGroup refID="esri_mapping_selectionGroup">
            <insertButton refID="esri_editing_ShowAttributes" placeWith="esri_mapping_clearSelectionButton" insert="before" size="middle" />
         </updateGroup>      
      </groups>    
   </updateModule>    
</modules>
```

###Access a button at runtime
All controls can be accessed and updated at runtime using the **FrameworkApplication.GetPlugInWrapper** function.
```c#
protected override void OnClick()
{
   IPlugInWrapper wrapper = FrameworkApplication.GetPlugInWrapper("acme_ZoomBtn");
   wrapper.Caption = "New Caption";
   wrapper.Tooltip = "New Tooltip";
}
```

###Execute a button at runtime
```c#
protected override void OnClick()
{
   IPlugInWrapper wrapper = FrameworkApplication.GetPlugInWrapper("esri_mapping_zoomToSelectionButton");
   var command = wrapper as ICommand; // tool and command(Button) supports this

   if ((command != null) && command.CanExecute(null))
      command.Execute(null);
}
```

###Create a checked button
Define the button in DAML as normal. Override the **OnClick** event in your button class to alter the **IsChecked** runtime property to set the checked state.
```xml
<button id="AppCustomization_CheckedButton" caption="Lock" className="MyCheckedButton" loadOnClick="false" >
   <tooltip heading="Checked button"></tooltip>
</button>
```
```c#
internal class MyCheckedButton : Button
{
   public MyCheckedButton()
   {
      IsChecked = false;
      Caption = "Lock";
      SmallImage = new System.Windows.Media.Imaging.BitmapImage(
                  new Uri(@"pack://application:,,,/ArcGIS.Desktop.Resources;component/Images/GenericUnLockNoColor16.png"));
   }

   protected override void OnClick()
   {
      IsChecked = !IsChecked;

      if (IsChecked)
      {
         Caption = "Unlock";
         Uri uriSource = new Uri("pack://application:,,,/ArcGIS.Desktop.Resources;component/Images/GenericLockNoColor16.png");
         SmallImage = new System.Windows.Media.Imaging.BitmapImage(uriSource);
      }
      else
      {
         Caption = "Lock";
         SmallImage = new System.Windows.Media.Imaging.BitmapImage(
                    new Uri(@"pack://application:,,,/ArcGIS.Desktop.Resources;component/Images/GenericUnLockNoColor16.png"));
      }
   }      
}
```