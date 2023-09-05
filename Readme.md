<!-- default badges list -->
![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/128634326/18.1.3%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/E2554)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)
<!-- default badges end -->
# WinForms Scheduler - Customize the popup menu

This example handles the [PopupMenuShowing](https://docs.devexpress.com/WindowsForms/DevExpress.XtraScheduler.SchedulerControl.PopupMenuShowing) event to customize the Scheduler's popup menu (add new items, modify and remove existing items):

```csharp
private void schedulerControl1_PopupMenuShowing(object sender, PopupMenuShowingEventArgs e) {
    if (e.Menu.Id == DevExpress.XtraScheduler.SchedulerMenuItemId.DefaultMenu) {
        // Hide the "Change View To" menu item.
        SchedulerPopupMenu itemChangeViewTo = e.Menu.GetPopupMenuById(SchedulerMenuItemId.SwitchViewMenu);
        itemChangeViewTo.Visible = false;
        // Remove unnecessary items.
        e.Menu.RemoveMenuItem(SchedulerMenuItemId.NewAllDayEvent);
        // Disable the "New Recurring Appointment" menu item.
        e.Menu.DisableMenuItem(SchedulerMenuItemId.NewRecurringAppointment);
        // Find the "New Appointment" menu item and rename it.
        SchedulerMenuItem item = e.Menu.GetMenuItemById(SchedulerMenuItemId.NewAppointment);
                if (item != null) {
                    item.Caption = "&New Meeting";
                    item.ImageOptions.SvgImage = DevExpress.Utils.Svg.SvgImage.FromFile("NewItem.svg");
                }
        // Create a menu item for the Scheduler command.
        ISchedulerCommandFactoryService service = schedulerControl1.GetService<ISchedulerCommandFactoryService>();
        SchedulerCommand cmd = service.CreateCommand(SchedulerCommandId.SwitchToGroupByResource);
        SchedulerMenuItemCommandWinAdapter menuItemCommandAdapter =
            new SchedulerMenuItemCommandWinAdapter(cmd);
        DXMenuItem menuItem = (DXMenuItem)menuItemCommandAdapter.CreateMenuItem(DXMenuItemPriority.Normal);
        menuItem.BeginGroup = true;
        e.Menu.Items.Add(menuItem);
        // Insert a new item into the Scheduler popup menu and handle its click event.
        e.Menu.Items.Add(new SchedulerMenuItem("Click me!", MyClickHandler));
    }
}
```

![WinForms Scheduler - Customize the popup menu](https://raw.githubusercontent.com/DevExpress-Examples/how-to-customize-the-scheduler-popup-menu-e2554/17.2.3+/media/f796b593-631a-11e7-80c0-00155d624807.png)


## See Also

* [Scheduler Pop-up Menus](https://docs.devexpress.com/WindowsForms/119049/controls-and-libraries/scheduler/visual-elements/pop-up-menus)
