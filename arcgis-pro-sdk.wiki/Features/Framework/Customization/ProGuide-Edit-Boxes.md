```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          10\29\2014
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

#####In this topic

* [How to declare an Edit box](#how-to-declare-an-edit-box)<br>
* [Control the enabled state of an Edit box using a condition](#control-the-enabled-state-of-an-edit-box-using-a-condition)<br>
* [Restricting entered text to a particular data type](#restricting-entered-text-to-a-particular-data-type)<br>
* [Setting the initial value](#setting-the-initial-value)<br>
* [Providing validation](#providing-validation)<br>

###How to declare an Edit box
Edit boxes provide a convenient means for users to enter text within a control on a ribbon tab. They are declared in DAML using the _editBox_ element within the _controls_ container.  

An Edit box is declared using the **id**, **className**, and **sizeString** attributes. It can be configured to appear with or without a **caption**. Use the **smallImage** attribute to have an image displayed with the control. The image is positioned to the left of any caption.

The **sizeString** attribute is used to establish the width of the control. This string is representative of the kind of input that appears in the control. The sizeString attribute can also be updated at runtime.

The Edit box is referenced on a group using the _editBox_ control with the **refID** attribute.
```xml
<groups>
   <group id="esri_domainview_filterGroup" caption="Filter">
      <editBox refID="esri_domainview_filterEditBox" size="medium"/>
   </group>
</groups>

<controls>    
   <editBox id="esri_domainview_filterEditBox" caption="Filter Text:" keytip="F" className="ArcGIS.Desktop.Internal.Mapping.DesignView.FilterDomainsEditBox" disableIfBusy="false" sizeString="1234567890123456789012" condition="esri_mapping_domainsPane">
      <tooltip heading="">Type filter text.<disabledText></disabledText></tooltip>
   </editBox>
</controls>    
```

The **className** attribute references the Edit box implementation. This is a class that inherits from the **ArcGIS.Desktop.Framework.Contracts.EditBox** base class.
```c#
internal class FilterDomainsEditBox : ArcGIS.Desktop.Framework.Contracts.EditBox
{
   public FilterDomainsEditBox()
   {
      Text = "";
   }
}
```

###Control the enabled state of an Edit box using a condition
Use the **condition** attribute to assign a predefined condition. For more information about defining states and conditions within DAML, see that section. 
```xml
<editBox id="esri_domainview_filterEditBox" caption="Filter Text:" keytip="F" className="ArcGIS.Desktop.Internal.Mapping.DesignView.FilterDomainsEditBox" disableIfBusy="false" sizeString="1234567890123456789012" condition="esri_mapping_domainsPane">
   <tooltip heading="">Type filter text.<disabledText></disabledText></tooltip>
</editBox>
```

###Restricting entered text to a particular data type
The editBox control provides a **dataType** attribute that allows the control to restrict the text entered by the user. The default value is string.  

####Allow integers only
```xml
<editBox id="MyCustomEditBox" caption="Filter Text:" className="MyEditBox" disableIfBusy="false"
                 smallImage="Images\GenericButtonBlue16.png" dataType="int64"
                 sizeString="1234" >
   <tooltip heading="">
      Type filter text.<disabledText></disabledText>
   </tooltip>
</editBox>
```

####Allow double only
```xml
<editBox id="MyCustomEditBox" caption="Filter Text:" className="MyEditBox" disableIfBusy="false"
                 smallImage="Images\GenericButtonBlue16.png" dataType="double"
                 sizeString="1234" >
   <tooltip heading="">
      Type filter text.<disabledText></disabledText>
   </tooltip>
</editBox>
```

####Allow all input
```xml
<editBox id="MyCustomEditBox" caption="Filter Text:" className="MyEditBox" disableIfBusy="false"
                 smallImage="Images\GenericButtonBlue16.png" dataType="string"
                 sizeString="123456789" >
   <tooltip heading="">
       Type filter text.<disabledText></disabledText>
   </tooltip>
</editBox>
```

###Setting the initial value 
In the constructor of your Edit box class use the **Text** runtime property to set the initial value.
```c#
internal class MyEditBox : EditBox
{
  public MyEditBox()
  {
    Text = "";
  }
}
```

###Providing validation
The base **ArcGIS.Desktop.Framework.Contracts.EditBox** class provides **OnEnter** and **OnTextChange** methods that can be overridden by your implemented class to provide validation and/or custom action when the user enters text into the Edit box. See the code snippets below for how this looks.

```xml
<groups>
   <group id="esri_domainview_filterGroup" caption="Filter">
      <editBox refID="MyCustomEditBox" size="medium"/>
   </group>
</groups>

<controls>  
   <editBox id="MyCustomEditBox" caption="Filter Text:" className="MyEditBox" disableIfBusy="false"
                 smallImage="Images\GenericButtonBlue16.png" dataType="string"
                 sizeString="1234678901234567890" >
      <tooltip heading="">
            Type filter text.<disabledText></disabledText>
      </tooltip>
   </editBox>
</controls>
```
```c#
internal class MyEditBox :  ArcGIS.Desktop.Framework.Contracts.EditBox
{
  public MyEditBox()
  {
    Text = "";
  }

  protected override void OnEnter()
  {
    // TODO - add specific validation code here 
  }

  protected override void OnTextChange(string text)
  {
    // TODO - add specific validation code here
  }
}
```