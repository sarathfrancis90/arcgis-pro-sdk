```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

Check box controls allow you to add a check box on the ribbon. They are declared in DAML using the _checkBox_ element within the _controls_ container. 

A check box is declared using the **id**, **caption**, **className**, and **tooltip** attributes. 

The check box is referenced on a group using the _checkBox_ element with the **refID** attribute.
```xml
<groups>
   <group id="MyGroup" caption="Group 1">
      <checkBox refID="MyCustomCheckBox" size="small" />       
   </group>
</groups>

<controls>    
   <checkBox id="MyCustomCheckBox" caption="My Checkbox" className="MyCheckBox" >
      <tooltip heading="click me"></tooltip>
   </checkBox>    
</controls> 
```

The **className** attribute references the check box implementation. This is a class that inherits from the **ArcGIS.Desktop.Framework.Contracts.CheckBox** base class. Override the virtual method **OnClick** to perform whatever custom behavior is desired.

Use the **IsChecked** runtime property to set the checked state. Toggle the IsChecked property between true and false for the normal dual-state check box or between true, false, and null for a tri-state check box.  
```c#
internal class MyCheckBox : ArcGIS.Desktop.Framework.Contracts.CheckBox
{
   public MyCheckBox()
   {
      IsChecked = true;
   }

   protected override void OnClick()
   {
      // TODO - add specific customization here as necessary
   }
}
```