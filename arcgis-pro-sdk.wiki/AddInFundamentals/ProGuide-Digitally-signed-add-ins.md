```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

ArcGIS Pro add-ins can be digitally signed to improve security in cases where add-in files are published and shared among users. With digitally signed add-in files, you can confirm the source of the file and verify that the original file contents were not modified after the signature was applied. However, the presence of a digital signature on an add-in file does not signify that the add-in is free of program errors.

Once an add-in file has been digitally signed, modifying or removing any of its contents, regardless of type, breaks the digital signature. Broken add-in file signatures are prominently indicated in the user interface when installing add-in files, and when reviewing existing add-in files using the Add-In Manager.

Every digital signature is linked to a standard ITU X.509 digital certificate that is used to apply the signature during the signing process. By default, signing with a digital certificate only ensures the installer that the content of the signed add-in didn't change since it was signed, but the installer can't be sure of the real identity of the add-in publisher. To be sure of the signed add-in publisher’s identity, the publisher must use a trusted digital certificate issued by a Commercial Certification Authority (such as VeriSign, Thawte, or Geotrust).  These types of certificates provide a trustworthy signature. Signed add-ins published by Esri fall into this category.

#####In this topic

[Signed add-ins in the enterprise](#signed-add-ins-in-the-enterprise)<br>
[Configure security policies for add-ins using the ArcGIS Pro installation program](#configure-security-policies-for-add-ins-using-the-arcgis-pro-installation-program)<br>
[Configure security policies for add-ins using ArcGIS Pro](#configure-security-policies-for-add-ins-using-arcgis-pro)<br>
[Installing add-in files with digital signatures](#installing-add-in-files-with-digital-signatures)<br>
[Applying digital signatures to an add-in](#applying-digital-signatures-to-an-add-in)<br>

***

####Signed add-ins in the enterprise

To keep an enterprise network secure from unauthorized use, system administrators have the ability to restrict files (executables, scripts, and so on) that can be deployed throughout the enterprise. This includes limiting the type of add-ins that are used in ArcGIS Pro. ArcGIS Pro uses Windows registry entries that can only be controlled by an administrator, to restrict access to certain add-ins. The following restrictions can be enforced starting with the least secure option:
* Load all add-ins regardless of whether they have digital signatures.
* Only load and use add-ins that are digitally signed by a trusted certificate authority.
* Only load and use add-ins that have been published by Esri.
* Only load add-ins from the administrator folders and all add-ins published by Esri.
* Do not load or execute add-ins in this application.

####Configure security policies for add-ins using the ArcGIS Pro Installation program

Organizations will sometimes need to deploy policies within their network to load only certain type of add-ins. This can be done by executing the ArcGIS Pro installation program with a command line switch.  Note: You need admin rights to perform this task. 

Refer to <a href="http://pro.arcgis.com/en/pro-app/beta/get-started/arcgis-pro-installation-administration.htm" target="_blank">this link</a> to run Pro installation program with the appropriate command line switches in order to configure the add-in deployment security policies.

####Configure security policies for add-Ins using ArcGIS Pro

The Add-In Manager dialog box—accessible from ArcGIS Pro, by clicking  Project > Add-In Manager—displays a list of all the add-in files currently installed on the user’s machine.

Along with other add-in file information, the digital signature status displays in the area indicated in the following screen capture:

![Add-in-manager](images/Digitally signed add ins/add-in-manager.png)

The signature status is determined as shown in the following table:

Status  | Description
------------- | -------------
None  | The selected add-in file does not contain any digital signatures.
Un-trusted  | The digital signature applied to the add-in file is not from a trusted source.
Invalid  | The digital signature has been invalidated due to manipulation of the add-in file contents, or the certificate has expired.
Authenticated  |The file is digitally signed with a signature that is valid and from a trusted source.

On the Options tab of the Add-In Manager dialog box, users can view and change options concerning how add-in file security is handled. The options, ranging from most secure to least secure, are as follows:
* Load only Esri provided add-ins (most secure)
* Require add-ins to be digitally signed by a trusted publisher
* Load all add-ins without restrictions (least secure)

![add-in-manager-options.png](images/Digitally signed add ins/add-in-manager-options.png)

```
Users without administrator privileges cannot modify these settings in a way that makes 
the system less secure than the settings an administrator has already established on 
the machine. Options that are not available to non-administrators are disabled on this tab.
```

####Installing add-in files with digital signatures 

Although add-in files can be installed by copying them to an appropriate well-known folder, doing so without first verifying the source and contents of the file is not recommended. Double-clicking an add-in file link from within a web browser, an email client, or Windows Explorer automatically opens the Esri ArcGIS Add-In Installation Utility dialog box. 

![Add-Installation Utility](images/Digitally signed add ins/dig-sign-install.png)

This dialog box displays pertinent add-in information, such as the name, date, author, version, and description of the add-in. If the add-in file is digitally signed, signature information also displays in this dialog box. If any of the presented information is unsatisfactory, the installation process can be cancelled by the user and the add-in file will not be installed.

Add-in files can be signed with more than one digital signature, depending on the internal policies of the authoring organization. Within the digital signature area of the Esri ArcGIS Add-In Installation Utility dialog box, the signer, date stamp, and validity of the selected signature display with information on whether the associated certificate is from a trusted source. Detailed information on the selected signature’s certificate can be displayed by clicking the Show Certificate button.

A secure add-in file must have at least one digital signature that is valid and trusted. An invalid signature indicates that the contents of the add-in file have been modified in some way since the signature was applied.

####Applying digital signatures to an add-in

The utility, ArcGISSignAddIn.exe, is provided in the ArcGIS Pro installation folder and can be used to sign add-ins. To use this utility, you must have a Personal Information Exchange (.pfx) file that stores the ITU X.509 certificate containing both public and private encryption keys. To learn more about how to create a .pfx file, read <a href="http://msdn.microsoft.com/en-us/library/windows/hardware/ff549703(v=vs.85).aspx" target="_blank">Personal Information Exchange (.pfx) Files</a> on MSDN. The utility can also be used to view or remove existing digital signatures. The steps below explain how ArcGISSignAddIn.exe can be integrated into your Visual Studio ArcGIS Pro add-in project to sign your add-in during the post build process.

**Step 1** <br>
Open your ArcGIS Pro add-in project in Visual Studio. Right-click the project context menu and choose Unload Project. <br>
![unload-project.png](images/Digitally signed add ins/unload-project.png)

**Step 2**<br>
Right-click the project context menu again and choose Edit Project. The .csproj or .vbproj file opens in the Visual Studio editor.
![edit-project.png](images/Digitally signed add ins/edit-project.png)

**Step 3**<br>
Add the following lines of code to the bottom of the project file before the closing project tag `</Project>`. 

* Replace the path in the sample command below to your .pfx file on disk and use the password that has been assigned to the .pfx file.
* Confirm that the path to ArcGIS Pro's bin folder matches your installation location.
 
```xml
  <Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />
  <UsingTask AssemblyFile="C:\Program Files\ArcGIS\Pro\bin\proapp-sdk-MSBuild.dll" TaskName="proapp_sdk_MSBuild.PackageAddIn" />
  <UsingTask AssemblyFile="C:\Program Files\ArcGIS\Pro\bin\proapp-sdk-MSBuild.dll" TaskName="proapp_sdk_MSBuild.CleanAddIn" />
  <UsingTask AssemblyFile="C:\Program Files\ArcGIS\Pro\bin\proapp-sdk-MSBuild.dll" TaskName="proapp_sdk_MSBuild.ConvertToRelativePath" />
  <Import Project="C:\Program Files\ArcGIS\Pro\bin\Esri.ProApp.SDK.Desktop.targets" Condition="Exists('C:\Program Files\ArcGIS\Pro\bin\Esri.ProApp.SDK.Desktop.targets')" />
  <!-- To modify your build process, add your task inside one of the targets below and uncomment it. 
       Other similar extension points exist, see Microsoft.Common.targets.
  <Target Name="BeforeBuild">
  </Target>  -->
  <Target Name="AfterBuild" DependsOnTargets="PackageArcGISAddInContents">
    <!--Replace the path in the sample command below to your pfx file on disk and use the password that has been assigned to the pfx file.
        Confirm that the path to ArcGIS Pro bin folder matches your installation location.-->
    <Exec Command="&quot;C:\Program Files\ArcGIS\Pro\bin\ArcGISSignAddIn.exe&quot; $(TargetDir)$(TargetName).esriAddInX /c:D:\ArcGISPro-SDK\ESRI.pfx /p:pizzapie408 /s"  />
  </Target>
</Project>

```

**Step 4**<br>
Click the Save button to save the project file. Right-click the project in the Solution Explorer and select Reload Project. Your add-in will now be digitally signed whenever you build your project.