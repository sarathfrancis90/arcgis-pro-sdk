```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

This ProGuide explains the step by step process on how to declare galleries. However this is for reference only. **The recommended approach is to run the [gallery item template](home#arcgis-pro-sdk-templates).** 

#####In this topic

[How to declare a gallery](#how-to-declare-a-gallery)<br>
[Customize the gallery item UI](#customize-the-gallery-item-ui)<br>
[Gallery feedback during initialization](#gallery-feedback-during-initialization)<br>
[Inline galleries](#inline-galleries)<br>



***

###How to declare a gallery
The Gallery is a new type of control available for the ribbon that behaves similarly to menus. Galleries typically provide a richer representation of the choices offered, each often representing a preview of the result if chosen. Galleries can be organized to show multiple rows and columns simultaneously, and are excellent choices when you do not want to be constrained by the smaller one dimensional area offered by a menu. 

Galleries are declared in DAML using the _gallery_ element within the _galleries_ container. Static aspects such as the **id**, **caption**, **className**, **itemWidth**, **itemHeight**, **tooltip**, and so on are specified declaratively. Use the **largeImage** and **smallImage** attributes to specify the drop-down image. The **itemSizeString** is used to specify the maximum width of items displayed in the gallery.

It is referenced on a group using the gallery control with the **refID** attribute.
```xml
<groups>
   <group id="MyGroup" caption="Group 1">
      <gallery refID="esri_mapping_tocFilterGallery" size="large" />
   </group>
</groups>

<galleries>
   <gallery id="esri_mapping_tocFilterGallery" className="TOC.TOCFilterGalleryViewModel" caption="Filter" extendedCaption="Filter layer in contents pane" itemsInRow="1" loadingMessage="Loading..." itemWidth="150" itemHeight="20" itemSizeString="12" showItemCaption="true" showItemCaptionBelow="true" resizable="true">
      <tooltip heading="">
         Filter<disabledText></disabledText>
      </tooltip>
   </gallery>
</galleries>

```

The gallery implementation is specified with the **className** attribute. The TOCFilterGalleryViewModel class specified above inherits from the **ArcGIS.Desktop.Framework.Contracts.Gallery** base class. The actual contents of a gallery are normally populated at runtime. Gallery items are modeled through the **GalleryItem** class. GalleryItems have the following properties: Icon or imagePath, Text, Group, and Tooltip. Custom GalleryItems can be created through inheritance to encapsulate any additional properties and/or behavior as needed.

```c#
internal class TOCFilterGalleryViewModel: ArcGIS.Desktop.Framework.Contracts.Gallery
{
   public TOCFilterGalleryViewModel()
   {
   }
}
```

###Customize the gallery item UI
Gallery items are represented in the UI via an ItemTemplate. A basic default template is used internally for all galleries that do not specify one. Use the **templateID** and the **dataTemplateFile** attributes to specify a custom template. 

```xml
<gallery id="esri_core_BasemapGallery" className="Ribbon.BasemapGalleryViewModel" caption="Basemap" itemsInRow="3" itemWidth="140" itemHeight="110" dataTemplateFile="pack://application:,,,/ESRI.AD.Resources;component/Styles/GalleryTemplates.xaml" templateID="BasemapItemTemplate"
          showItemCaption="true" showItemCaptionBelow="true" resizable="true" largeImage="pack://application:,,,/ESRI.AD.Resources;component/Images/Basemap32.png">
  <tooltip>Choose a basemap for your map.&#xd;&#xd;The basemap is the reference data that displays under the notes and other GIS data you have added to the map.</tooltip>
</gallery>
```

In GalleryTemplates.xaml:
```xml
<DataTemplate x:Key="BasemapItemTemplate">
  <Grid>
    <StackPanel Orientation="Vertical" Margin="5,0,5,0">
      <Grid Margin="5">
        <Image Source="{Binding Thumbnail}" MaxHeight="87" MaxWidth="120">
          <Image.Effect>
            <DropShadowEffect Color="#FF565454" Opacity="0.4" />
          </Image.Effect>
        </Image>
      </Grid>
      <TextBlock Text="{Binding Title}" HorizontalAlignment="Center" />
    </StackPanel>
  </Grid>
</DataTemplate>
```

Alternatively, you can create a DataTemplateSelector and specify its resource key as templateID if you have more than one data template for your data. To do that, implement a **DataTemplateSelector** class, for example:

```c#
public class BaseMapTemplateSelector : DataTemplateSelector
{
   public DataTemplate BaseMapTemplate { get; set; }
   public DataTemplate OtherTemplate { get; set; }

   public BaseMapTemplateSelector(){}

   public override DataTemplate SelectTemplate(object item, DependencyObject container)
   {
      BaseMap basemap = item as BaseMap;
      if (basemap != null)
        return BaseMapTemplate;

      return OtherTemplate;
   }
}
```

The DataTemplateSelector can then specify which template to use in GalleryTemplates.xaml:
```xml
<BaseMapTemplateSelector  x:Key="BaseMapTemplateSelector"
     BaseMapTemplate="{StaticResource BasemapTemplate}"
     OtherTemplate="{StaticResource otherItemTemplate}"/>
```

You can then specify the resource key as the templateID attribute in the gallery DAML definition: 
```xml
templateID="BaseMapTemplateSelector"
```

###Gallery feedback during initialization
To better support a responsive UI experience, the framework provides a waiting feedback with a spinner and loading message on the gallery’s drop-down when a gallery is trying to asynchronously load large number of items. The loading message can be either updated at runtime through code or statically set using the **loadingMessage** attribute. Note that to get this default behavior, no heavy code should be put in the gallery’s constructor since it blocks the UI thread and prevents the spinner from showing.

###Inline galleries
Galleries can also present a condensed grid within the ribbon using the inline gallery representation. The items presented in this way are often the most common or most recently used items depending on the implementation. Set the **inline** attribute on the gallery control to true to achieve an inline gallery.

```xml
<groups>
   <group id="esri_editing_EditGallery" caption="Tools" smallImage="pack://application:,,,/ArcGIS.Desktop.Resources;component/Images/EditingToolBarShow16.png">
      <gallery refID="esri_EditFeaturesGallery" inline="true" />
   </group>
</groups>

<galleries>
   <gallery id="esri_EditFeaturesGallery"  className="EditFeatures.GalleryCommandListViewModel"                   caption="Editor tool gallery" keytip="ETG" 
                 itemWidth="72" itemsInRow="6" showItemCaption="true" showGroup="false" showItemCaptionBelow="true" resizable="true" 
largeImage="pack://application:,,,/ArcGIS.Desktop.Resources;component/Images/EditingToolBarShow32.png"
                 condition="esri_editing_EditingCondition">
          <tooltip heading="">View a gallery of editing tools.<disabledText></disabledText></tooltip>
   </gallery>
</galleries>
```
