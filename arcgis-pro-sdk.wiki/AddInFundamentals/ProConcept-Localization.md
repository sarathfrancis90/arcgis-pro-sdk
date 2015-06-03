```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

## ProConcept Localizing add-ins

Localization of an add-in is the translation of add-in resources into localized versions for the specific cultures and locale the add-in has to support.  Globalization is the process of designing and developing add-ins that function for multiple cultures and locales. In addition to the translation into the target language, the add-in also has to support number, currency, date formats, and sorting rules for each specific region or locale. 

The settings for these regional options are not driven by the language but instead by user settings such as the User Locale (CurrentCulture). The globalization process described in this section addresses both: translation into a target language and supporting a target region or locale.

Globalizing an ArcGIS Pro add-in follows Microsoft's standards for .NET globalization and WPF globalization as defined by Microsoft. For more information, see [WPF Globalization and Localization Overview][1].

The declarative portion of an add-in extension is defined within a DAML file containing a collection of framework elements including those elements' captions and other attributes. Similar to resource files and XAML files in .NET/WPF, DAML files are directly translated. In a Visual Studio project file for an ArcGIS Pro add-in, this results in the following additional resource and DAML files that hold the translated text:
In .NET translation of resource files, adds a translation for each supported language, for example, Resources.resx (default translation) and Resources.de.resx (German Translation). For DAML files, direct translations result in the following translation files: Config.daml (default translation) and Config.de.daml (German translation).

### General WPF design guidelines for globalization

When designing WPF based user interfaces ready for globalization, consider these best practices:

Write your UI in XAML and avoid creating UI in code. When you create your UI by using XAML, you expose it through built-in localization APIs. Avoid using absolute positions and fixed sizes to lay out content; instead, use relative or automatic sizing. Use SizeToContent and and keep widths and heights set to Auto. Avoid using Canvas to lay out UIs. Use Grid and its size sharing feature. Provide extra space in margins because localized text often requires more space. Extra space allows for possible overhanging characters. Enable TextWrapping on TextBlock to avoid clipping. Use standard numeric and date/time formats instead of custom formatting. For example, use Currency ‘C’: 123.456 ("C", en-US) -> $123.46, 123.456 ("C", fr-FR) -> 123,46 €, or Date ‘d’: 2009-06-15T13:45:30(en-US) -> 6/15/2009, 2009-06-15T13:45:30(fr-FR) -> 15/06/2009.

There are various ways to implement localization in WPF (see [Globalization and Localization in WPF – Some thoughts][2] for a detailed discussion on some of the methods). 

However, since ArcGIS Pro add-ins are utilizing the MVVM (Model-View-ViewModel) pattern, the recommended method for localization is to use resource files with static references to the resource properties. Using the resource file methods means that both code and XAML need to reference a string property in your resource files using a code reference like Properties.Resources.[the string’s name] or a static XAML reference like {x:Static prop:Resources.[the string’s name]} with prop defined as ‘xmlns:prop="clr-namespace:Localization.Properties"’. The Localization ProTutorial explains this Localization method step-by-step in more detail.

### Deployment of localized add-ins

If you deploy a globalized add-in that contains multiple languages, ArcGIS Pro utilizes the language matching the Microsoft Windows setting and if there is no matching language in the add-in, the system reverts to the default resources.  For example, if the add-in has these translations: Resources.resx (default translation) and Resources.de.resx (German translation), Resources.de.resx is utilized by Windows installations using the German language preference; whereas, all other Windows language configurations utilize the default translation, Resources.resx. ArcGIS Pro also has an overwrite option that allows changing the default language for ArcGIS Pro:

![package](images/Localization/ArcGISoptions.png)




  [1]: http://msdn.microsoft.com/en-us/library/ms788718(v=vs.110).aspx
  [2]: http://formatexception.com/2014/05/globalization-and-localization-in-wpf-some-thoughts/
