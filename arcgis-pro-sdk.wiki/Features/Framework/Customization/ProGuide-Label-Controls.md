```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          10\29\2014
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

Labels are declared in DAML using the _labelControl_ element within the _controls_ container. 

A label is declared using the **id** and **caption** attributes. You can also specify the **largeImage** and **smallImage** attributes if your label is to have an image associated with it. 

Labels are declarative only; they do not have a class implementation.

The label is referenced on a group using the _labelControl_ element with the **refID** attribute.
```xml
<groups>
   <group>
      <labelControl refID="esri_mapping_labelClassNameLabel" size="small"/>
   </group>
</groups>

<controls>    
   <labelControl id="esri_mapping_labelClassNameLabel" caption="Class" />      
</controls>    
```
