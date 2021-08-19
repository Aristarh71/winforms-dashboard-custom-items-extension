# Custom Items

This example contains the source code of the most requested custom items. Custom items allow you to embed any WinForms control into a [Dashboard](https://www.devexpress.com/products/net/dashboard/). You can use the complete custom items from this example as they are, or modify them according to your needs. 
In a test dashboard of this example, you can add custom items from the Ribbon and switch between tabs in the [ribbon UI main menu](https://docs.devexpress.com/WindowsForms/5482/controls-and-libraries/ribbon-bars-and-menu/common-features/look-and-feel) at the left edge to display each custom item on the [dashboard surface](https://docs.devexpress.com/Dashboard/18205/winforms-dashboard/winforms-designer/ui-elements/dashboard-surface?p=netframework).

![custom-item-overview](images/custom-items-overview.png)

## Prerequisites  

A custom [Web Page](#custom-web-page) item requires [WebView2 Runtime](https://developer.microsoft.com/en-us/microsoft-edge/webview2/) to be installed on your machine.

## Example Overview

The example consists of two projects: 
* [CustomItemExtension](CustomItemExtension/CustomItemExtension)

    Contains the source code of custom items and can be used in customer solutions. 

* [CustomItemTest](CustomItemExtension/CustomItemTestQ)

    Contains a dashboard with custom items and can be used as a test project.


Refer to the following list for detailed item descriptions:

- [Custom Sankey item](#custom-sankey-item)

- [Custom Sunburst item](#custom-sunburst-item)

- [Custom Tree List item](#custom-tree-list-item)

- [Custom Waypoint Map item](#custom-waypoint-map-item)

- [Custom Funnel item](#custom-funnel-item)

- [Custom Gantt item](#custom-gantt-item)

- [Custom Web Page](#custom-web-page)

### Custom Sankey Item
[View file](CustomItemExtension/CustomItems/SankeyChart)

The Sankey diagram visualizes data as weighted flows or relationships between nodes.

![Sankey](images/Sankey.png)

### Custom Sunburst Item
[View file](CustomItemExtension/CustomItems/SunburstChart)

The Sunburst chart combines a Treemap and Pie chart to visualize hierarchical data in a circular layout.

![Sunburst](images/SunBurst.png)

### Custom Tree List Item
[View file](CustomItemExtension/CustomItems/HierarhyTree)

The hybrid item combines a Tree List and Grid. The Tree List uses the parent-child relationships to generate the hierarchical data structure.

![TreeList](images/Treelist.png)

### Custom Waypoint Map Item

[View file](CustomItemExtension/CustomItems/WaypointMap)

The Waypoint map visualizes data as linked points.

![WayPointMap](images/WayPointMap.png)

### Custom Funnel Item

[View file](CustomItemExtension/CustomItems/Funnel)

The Funnel chart visualizes the progressive reduction of data as it passes from one stage to another in a process or procedure.

![Funnel](images/Funnel.png)

### Custom Gantt Item

[View file](CustomItemExtension/CustomItems/GanttItem)

The Gantt chart visualizes project schedule data as bars.

![Gantt](images/Gantt.png)

### Custom Web Page

[View file](CustomItemExtension/CustomItems/WebPageItem)

The Web Page displays the web content. You can specify the URI pattern to create the page address from a data column at run-time.

![Web Page](images/webpage.png)
## Example Structure

Custom item files are stored in the _CustomItems_ folder. Each custom item has the following classes:

### CustomItemExtensionModule

Implements the `IExtensionModule` interface with the following methods:

* `AttachViewer` / `AttachDesigner`

	Execute the binding code. Subscribe the `CustomDashboardItemControlCreating` event, create custom item bars, and remove buttons from the Ribbon if necessary. For example, the custom Sunburst item does not support Drill-Down. The `RemoveDrillDownBarItem` method removes this option from the Ribbon. 
* `DetachViewer` / `DetachDesigner`

	Execute the unbinding code. Unsubscribe from the `CustomDashboardItemControlCreating` event.
	
### CustomItemMetaData 

Contains metadata for a custom item that describes options and settings available to a user in the UI. 

### CustomItemControlProvider 

Contains configuration settings for a custom control. A custom control displays a custom item in a dashboard.

This class contains the following methods:

* `CustomItemControlProvider ()` - [provider](https://docs.devexpress.com/Dashboard/DevExpress.DashboardWin.CustomControlProviderBase) constructor. This is where the default control settings and event subscriptions are performed.

* [UpdateControl](https://docs.devexpress.com/Dashboard/DevExpress.DashboardWin.CustomControlProviderBase.UpdateControl(DevExpress.DashboardCommon.CustomItemData))

	 - Creates and updates a custom control.

	 - Creates a custom item data source.

	 - Validates data members in metadata.

	 - Binds custom item to data.

	 - Sets Master Filter mode and Drill-Down.

* [GetPrintableControl](https://docs.devexpress.com/Dashboard/DevExpress.DashboardWin.CustomControlProviderBase.GetPrintableControl(DevExpress.DashboardCommon.CustomItemData-DevExpress.DashboardCommon.CustomItemExportInfo)) 

	 Obtains the printable control to export a custom item. You need to create a `PrintableComponentContainer` from the current custom control.  

* [SetSelection](https://docs.devexpress.com/Dashboard/DevExpress.DashboardWin.CustomControlProviderBase.SetSelection(DevExpress.DashboardWin.CustomItemSelection))

	Updates a custom control according to the current master filter selection. 


The _Images_ folder contains icons for custom items and their options. 

The file that is used for an icon must be embedded in the file assembly. The file’s [BuildAction](https://docs.microsoft.com/en-us/visualstudio/ide/build-actions?view=vs-2019) property is set to `Embedded Resource`.
 
 
## Integrate a Custom Item to Your Project

1. Add the [CustomItemExtension](CustomItemExtension/CustomItemExtension) project to your solution.

1. Add a reference to this project to "References" in your project with a dashboard control.

1. Register the [CustomItemMetadata](https://docs.devexpress.com/Dashboard/DevExpress.DashboardCommon.CustomItemMetadata) type for a custom item and attach its module in your application:

For example, call the following code to register the `SankeyItemMetadata` type in your application:

**C# code**:
```csharp
namespace CustomItemsSample {
    static class Program {
        /// <summary>
        /// The main entry point for the application.
        /// </summary>
        [STAThread]
        static void Main() {
            //...
            Dashboard.CustomItemMetadataTypes.Register<SankeyItemMetadata>();
            Application.Run(new Form1());
        }
    }
}
```

**VB code**: 
```vb
Namespace CustomItemsSample
    Friend NotInheritable Class Program
        ''' <summary>
        ''' The main entry point for the application.
        ''' </summary>
        Private Sub New()
        End Sub
        <STAThread> _
        Shared Sub Main()
          '...
          Dashboard.CustomItemMetadataTypes.Register(Of SankeyItemMetadata)()
          Application.Run(New Form1())
        End Sub
    End Class
End Namespace
```
* Call the following code to attach the extension to the `DashboardDesigner` control:

**C# code**:
```csharp
public Form1() {
        InitializeComponent();
        SankeyItemModule.AttachDesigner(dashboardDesigner1);
}
``` 

**VB code**: 
```vb
Public Sub New()
		InitializeComponent()
		SankeyItemModule.AttachDesigner(dashboardDesigner1)
End Sub 
```
## Documentation

- [Custom Item Tutorials](http://docs.devexpress.com/Dashboard/403031/winforms-dashboard/winforms-designer/create-a-custom-item#tutorials)

- [Custom Item Troubleshooting](https://docs.devexpress.com/Dashboard/403250/winforms-dashboard/winforms-designer/create-a-custom-item/custom-item-troubleshooting)



