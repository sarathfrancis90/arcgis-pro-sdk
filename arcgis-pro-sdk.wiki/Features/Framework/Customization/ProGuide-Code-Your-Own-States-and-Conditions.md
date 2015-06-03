This guide demonstrates how to code your own custom states and conditions. 

```
Language:      C# and Visual Basic
Subject:       Framework
Contributor:   ArcGIS Pro SDK Team <arcgisprosdk@esri.com>
Organization:  Esri, http://www.esri.com
Date:          04\15\2015
ArcGIS Pro:    1.1 (Beta)
Visual Studio: 2013
```

**Background**

If you want a refresher or need help on how to build an add-in, read [ProGuide: Build Your 1st Add-in](ProGuide-Build-Your-First-Add-in).<br/>
**Overview**

There are three basic steps to creating and using states and conditions. 

1. Define the condition (based on a state or combination of states)
2. Create a method or property to toggle the state
3. Link the condition to a UI element 

Use the method or property created in step 2 to modify the state value in response to some application change. This forces the condition to be re-evaluated and the UI to update.

Assume you have an add-in that contains two buttons on the UI. The first is a toggle button that controls the status of the state. The second button responds to the state through a condition.

<br/>
**Step 1**: Define the condition

Define the condition in the DAML. Remember that conditions live outside the module tags.

```xml
  <conditions>
    <!-- our custom condition -->
    <insertCondition id="example_state_condition" caption="Example state">
      <!-- our condition is set true or false based on this underlying state -->
      <state id="example_state" />
    </insertCondition>
  </conditions>
```

**Step 2**: Create a method to toggle the state

```C#

#region Toggle State
/// <summary>
/// Activate or Deactivate the specified state. State is identified via
/// its name. Listen for state changes via the DAML <b>condition</b> attribute
/// </summary>
/// <param name="stateID"></param>
public static void ToggleState(string stateID) 
{
   if (FrameworkApplication.State.Contain(stateID)) 
   {
      FrameworkApplication.State.Deactivate(stateID);
   } 
   else 
   {
      FrameworkApplication.State.Activate(stateID);
   }
}

#endregion Toggle State
```

**Step 3**: Link the condition to a UI element

In this instance, the condition is linked to a button on the ribbon.

```xml
<!-- associate our condition with the enabled state of the button via the "condition" attribute -->
<button id="esri_sdk_RespondToAppStateBtn" caption="Respond to state" className="RespondToStateButton" 
           loadOnClick="true" smallImage="Images\GenericButtonBlack16.png"
           largeImage="Images\GenericButtonBlack32.png" condition="example_state_condition">
          <tooltip heading="Respond To State Button">
            Listens for the condition named <b>example_state_condition</b><disabledText>The <b>example_state_condition</b> is False</disabledText>
          </tooltip>
</button>
```

In this example, modifying the state value is achieved through an additional button whose OnClick event calls the ToggleState method with the _example_state_ parameter.

<br/>