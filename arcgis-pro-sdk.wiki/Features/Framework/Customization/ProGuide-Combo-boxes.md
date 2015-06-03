```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

This ProGuide explains the step by step process on how to declare a combo box. However this is for reference only. **The recommended approach is to run the [combo box item template](home#arcgis-pro-sdk-templates).** 

#####In this topic

* [How to declare a Combo box](#how-to-declare-a-combo-box)<br>
* [Populating a Combo box](#populating-a-combo-box)<br>
* [Determine when a selection occurs](#determine-when-a-selection-occurs)<br>

###How to declare a Combo box
Combo box controls are similar to Edit box controls, but also provide a drop-down list. They are declared in DAML using the _comboBox_ element within the _controls_ container. 

A Combo box is declared using the **id**, **className**, and **sizeString** attributes. It can be configured to appear with or without a **caption**. Use the **readOnly** attribute to set the Combo box to read-only. Use the **condition** attribute to assign a predefined condition to control the enabled state.

The **sizeString** attribute is used to establish the width of the control. This string is representative of the kind of input that appears in the control. The sizeString attribute can also be updated at runtime.

Use the **ItemHeight** and **ItemWidth** attributes to customize the width and height of the control on the ribbon. Use the **rows** attribute to set the number of rows that display on the drop-down list.

The Combo box is referenced on a group using the _comboBox_ control with the **refID** attribute.
```xml
<groups>
   <group>
      <labelControl refID="esri_mapping_labelClassFieldLabel" size="small"/>
      <comboBox refID="esri_mapping_labelClassExpressionComboBox" size="small" />
      <button refID="esri_mapping_labelClassExpressionButton" size="small" />
   </group>
</groups>

<controls>  
   <comboBox id="esri_mapping_labelClassExpressionComboBox" 
             condition="esri_mapping_singleFeatureLayerSelectedCondition" caption=""             
             className="Labeling.Ribbon.LabelClassExpressionComboBox" isEditable="false" isReadOnly="true" 
             itemWidth="140">
          <tooltip heading="Label Class">Set the label expression for the current label class.
               <disabledText></disabledText></tooltip>
   </comboBox>
</controls>  
```

The **className** attribute references the Combo box implementation. This is a class that inherits from the **ArcGIS.Desktop.Framework.Contracts.ComboBox** base class. Combo boxes populate dynamically at runtime by this derived class. The list consists of ComboBoxItem objects, each with a caption, optional image, and generic tag.  

```c#
internal class LabelClassExpressionComboBox : ArcGIS.Desktop.Framework.Contracts.ComboBox
{
   public LabelClassExpressionComboBox ()
   {
      // TODO : add code here to dynamically populate the combobox
   }
}
```

###Populating a Combo box
Combo boxes populate dynamically at runtime by their derived class. The Combo box list consists of ComboBoxItem objects, each with a caption, optional image, and generic tag.  

```c#
internal class MyComboBox : ArcGIS.Desktop.Framework.Contracts.ComboBox
{
   public MyComboBox()
   {
      this.Add(new ComboBoxItem("Item1"));
      this.Add(new ComboBoxItem("Item2"));
      this.Add(new ComboBoxItem("Item3"));
      this.Add(new ComboBoxItem("Item4"));
      this.Add(new ComboBoxItem("Item5"));
   }
}
```

###Determine when a selection occurs
The base **ArcGIS.Desktop.Framework.Contracts.ComboBox** class provides an **OnSelectionChange** method that can be overridden by your implemented class to provide a custom action when the user selects an item from the Combo box. See the code snippet below for how this looks.

```c#
internal class MyComboBox : ArcGIS.Desktop.Framework.Contracts.ComboBox
{
   public MyComboBox()
   {
      this.Add(new ComboBoxItem("Item1"));
      this.Add(new ComboBoxItem("Item2"));
      this.Add(new ComboBoxItem("Item3"));
      this.Add(new ComboBoxItem("Item4"));
      this.Add(new ComboBoxItem("Item5"));
   }

   protected override void OnSelectionChange(ComboBoxItem item)
   {
      // TODO - add specific custom code here
   }
}
```