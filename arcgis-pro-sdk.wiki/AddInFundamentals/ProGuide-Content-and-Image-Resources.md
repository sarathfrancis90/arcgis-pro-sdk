```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

Often you need to include content and resources within your add-in. For example, you need to distribute a PDF or text file with your add-in, or your controls and custom UI will commonly reference image content. To include content in your ArcGIS Pro add-in, follow the steps shown in these examples:

* [Image content](#image-content)
* [Images as AddinContent](#images-as-addincontent)
* [Images as resources](#images-as-resources)
* [Images as embedded resources](#images-as-embedded-resources)
* [Referencing images from ArcGIS Pro](#referencing-images-from-arcgis-pro)

***

* [Document content](#document-content)
* [Accessing document content](#accessing-document-content)



### Image content

This can be a confusing topic given that there are so many different options. In general, follow these guidelines:

* If you have an image control or image source property on a control, use **Resource**.
* If you have a control in DAML that uses an image (for example, a button), use **AddinContent**.

Embedded resources are not really useful for ArcGIS Pro add-ins. However, if you need to use them, refer to [Images as Embedded Resources](#images-as-embedded-resources). If you want to use an image from the ArcGIS Pro UI in your own add-in, refer to [Referencing Images from ArcGIS Pro](#referencing-images-from-arcgis-pro). 

### Images as AddinContent

_AddinContent_ is a custom Build Action introduced to use with add-ins. It is **_exclusively_** intended for image resources referenced by Addin controls in DAML.

Because of the delay load capability of add-ins, the images associated with a button, tool, menu item, and so on, need to be shown on the UI **_before_** the add-in assembly that contains them is loaded (images are accessed in WPF from resources in a _loaded_ assembly).

Resources with a Build Action of AddinContent are added to your add-in archive root when your add-in is built. They are accessed directly from within the add-in archive by Pro so that the add-in assembly does not have to be loaded to render the add-ins on the UI.

To use AddinContent:

1. Set the Build Action to AddinContent. Set Copy to Output Directory to Do not copy.
2. Specify the _relative_ path to the image resource in your DAML. Because you are specifying a path to a resource in the add-in archive, you must use "\" (backslash) as the path separator (and not "/" (forwardslash) as with pack URI syntax.

For example, given an image AddinDesktop32.png in a project folder called Images, to reference it in DAML:

```XML
<!-- Note the backslash -->
<button id="acme_module1_Button1" className="Button1" caption="Button1"
               largeImage="Images\AddinDesktop32.png"/>

```

To access in code:
```C#
using System.Windows.Media.Imaging;
using System.IO.Compression;

//Image is defined as AddinContent.
//AddinContent is ESRI custom build action. Cannot use without Addin
BitmapImage forAddinContent = new BitmapImage();
string id = "{49545510-2f4a-4e9c-bc12-3ea79a330fa0}";//From your module
using (ZipArchive zip = ZipFile.OpenRead(@"C:\Users\my_username\Documents\ArcGIS\AddIns\ArcGISPro1.0\" + id + @"\AddinContent.esriAddInX")) {
	ZipArchiveEntry zipEntry = zip.GetEntry("Images\AddInDesktop32.png");
	MemoryStream ms = new MemoryStream();
	using (Stream stmZip = zipEntry.Open()) {
		stmZip.CopyTo(ms);
	}
	
	forAddinContent.BeginInit();
	forAddinContent.StreamSource = ms;
	forAddinContent.EndInit();
}

```

ZipArchive and ZipArchiveEntry can be found in System.IO.Compression. Add references to System.IO.Compression.dll and System.IO.Compression.FileSystem.dll.

#### Using image as resource for add-in control content

If you do want to use an image in your assembly with a Build Action of Resource and **not** AddinContent as the image for a button, tool, and so on, your add-in assembly must load at startup (and not delay load). Set the **insertModule** autoLoad attribute to **true** in your Config.daml (the default is false for delay loading).

```XML
<!-- in your Config.daml: note autoLoad attribute -->
<insertModule id="ProModule_Module1" className="Module1" autoLoad="true" caption="Module1">

<!-- now you can use a pack uri on your control DAML declaration -->
<button id="ProModule_Button1" caption="Button1" className="Button1" loadOnClick="true"
          largeImage="pack://application:,,,/Module1;component/images/AddinDesktop32.png"/>

```

### Images as resources

Add the image to your Visual Studio project and:

1. Set its Build Action to _Resource_. 
2. Set the Copy to Output Directory property to _Do not copy_. 

(Refer to [MSDN](http://msdn.microsoft.com/en-us/library/0c6xyb66(v=vs.100).aspx) for more information on Build Action settings for images.)

Image resources can be accessed in code or in XAML. For example, given an image called Resource32.png in a project folder called Images in an add-in assembly called AddinContent.dll. To access:

**In code:**
```C#
BitmapImage fromResource = new BitmapImage(new Uri("pack://application:,,,/AddinContent;component/Images/Resource32.png", UriKind.Absolute));

```

**In XAML:**
```XML
<!-- Assume we have an Image Control -->
<Image Stretch="None" Source="pack://application:,,,/AddinContent;component/Images/Resource32.png"/>

```

Usually, an image source in your add-in's custom UI binds to an _ImageSource_ property on a view model. Assuming you have a view model set on the DataContext of your window or user control, then:

```C#
public class MyViewModel : INotifyPropertyChanged {
    private BitmapImage _img = null;

    public ImageSource ContentImageSource {
        get {
            if (_img == null)
                _img = new BitmapImage(new Uri(  "pack://application:,,,/AddinContent;component/Images/Resources32.png", UriKind.Absolute));
            return _img;
        }
    }    
```
In the control XAML:
```XML
<Image Stretch="None" Source="{Binding Path=ContentImageSource}"/>

```

### Images as embedded resources

Embedded resources are really designed to use with Windows and System.Drawing.Bitmap, whereas resources are designed to use with WPF and System.Windows.Media.Imaging.BitmapImage. In Arcobjects extensions, you will have used _Embedded Resources_ extensively but in Pro, you will instead be working mostly with _Resources_.

If you do need to use embedded resources with Pro, you can only access them in code. For example, given an an image called EmbeddedResource32.png in a project folder called Images that is an embedded resource:

```C#
//Image is defined as Embedded Resource. Copy to Output Directory = Do not copy
var asm = System.Reflection.Assembly.GetExecutingAssembly();
var stm = asm.GetManifestResourceStream(this.GetType(), "Images.EmbeddedResource32.png"));
BitmapImage embeddedResource = new BitmapImage();
embeddedResource .BeginInit();
embeddedResource .StreamSource = stm;
embeddedResource .EndInit();
```

### Referencing images from ArcGIS Pro

Assume you want to reuse an image on the ArcGIS Pro ribbon for your own add-in, then:

1. Locate the image's URI for the image you want to use in the relevant Pro extension DAML file.
2. Copy the image URI into your config.daml, XAML, or code.

Note: Unlike add-in image content, the Pro image resources _are_ loaded at startup; therefore, you can safely use a pack URIto reference them. Pro resources can be found in ArcGIS.Desktop.Resources.dll.

For example, assume you want to reference the &quot;ColorSelector&quot; image from the color selector button in the mapping module within your add-in:

Find the URI in ADMapping.daml (located in your ArcGIS Pro bin\Extensions\Mapping folder), and copy and paste it into your code or DAML.

```XML
<!-- Use a Pro image for a custom Add-in button -->
<button id="acme_module1_Button1" className="Button1" caption="Button1"
               largeImage="pack://application:,,,/ArcGIS.Desktop.Resources;component/Images/ColorSelector32.png"/>

```

***

### Document content

To deploy file content with your add-in:

1. Set its **Build Action**  = _Content_ or **Build Action** = _None_ (makes _**no**_ difference which). 
2. Set **Copy to Output Directory** = _Copy always_. 

<table>
<tr><td><b>Build Action None</b></td><td><b>Build Action Content</b></td></tr>
<tr><td><img src="images/working_with_content/file_props.png"/></td>
    <td><img src="images/working_with_content/file_props_content.png"/></td>
</tr>
</table>

Add-in file contents are copied to a temporary output folder by the ArcGIS Pro framework at runtime when the add-in loads. Any folder hierarchy within your add-in's Visual Studio project containing content is preserved in the output.

### Accessing document content

To locate the document content, get the path to the add-in assembly location at runtime and append to it the relative path to the document content to be accessed.

Step 1: To get the location of your assembly use:
```C#
string uri = System.Reflection.Assembly.GetExecutingAssembly().Codebase;
```
You can also use this handy function:
```C#

public static string AddinAssemblyLocation() {
    var asm = System.Reflection.Assembly.GetExecutingAssembly();
    return System.IO.Path.GetDirectoryName(
                      Uri.UnescapeDataString(
                              new Uri(asm.CodeBase).LocalPath));
}
```
Step 2: Append the relative path to the document content. For example, assume you have a content file called Screenshot_doc.docx in a folder called ThisIsContent that has been deployed as content:

```C#
string contentPath = System.IO.Path.Combine(AddinAssemblyLocation(), "ThisIsContent", "Screenshot_doc.docx");
//open it
ProcessStartInfo psi = new System.Diagnostics.ProcessStartInfo(contentPath);
psi.UseShellExecute = true;
Process.Start(psi);

```
