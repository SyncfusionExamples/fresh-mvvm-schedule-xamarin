# How to bind appointments in Xamarin.Forms Schedule (SfSchedule) using FreshMVVM framework

The [SfSchedule](https://help.syncfusion.com/xamarin/scheduler/overview?) allows you to work with FreshMvvm framework. To achieve this, follow the below steps:

The following article explains about working with FreshMVVM framework in Schedule
https://www.syncfusion.com/kb/11227/how-to-bind-appointments-in-xamarin-forms-schedule-sfschedule-using-freshmvvm-framework

**Step 1:** Install the [FreshMvvm](https://www.nuget.org/packages/FreshMvvm/) NuGet package in your shared code project.

**Step 2:** Create your XAML page (view) with name ending with “Page”

``` c#
namespace ScheduleXamarin
{
    public partial class SchedulerPage : ContentPage
    {
        public SchedulerPage()
        {
            InitializeComponent();
        }
    }
}
```
**Step 3:** Create a page model with the name ending with PageModel and inherit FreshBasePageModel. If your Page name is MainPage, then the PageModel name should be MainPageModel and the namespace of Page and PageModel should be the same. In this PageModel, you can keep the ViewModel related properties.

``` c#
namespace ScheduleXamarin
{
    public class SchedulerPageModel : FreshBasePageModel
    {
        
    }
}
```
**Step 4:** To raise property changed notifier, use RaisePropertyChanged method of base class in your PageModel.
``` c#
public class SchedulerPageModel : FreshBasePageModel
{
        private ObservableCollection<Meeting> meetings;
 
        public SchedulerPageModel()
        {
            this.Meetings = new ObservableCollection<Meeting>();
        }
 
        public ObservableCollection<Meeting> Meetings
        {
            get
            {
                return this.meetings;
            }
 
            set
            {
                this.meetings = value;
                this.RaisePropertyChanged("Meetings");
            }
        }
}
```
**Step 5:** Set MainPage using PageModel in your App.xaml.cs file.
``` c#
public partial class App : Application
{
        public App()
        {
            InitializeComponent();
 
            var page = FreshPageModelResolver.ResolvePageModel<SchedulerPageModel>();
            var basicNavContainer = new FreshNavigationContainer(page);
            MainPage = basicNavContainer;
        }
}
```
**Step 6:** Bind the appointment collection to the Schedule DataSource without mentioning the binding context.
``` xml
<schedule:SfSchedule x:Name="schedule" ScheduleView="WeekView" DataSource="{Binding Meetings}">
          <schedule:SfSchedule.AppointmentMapping>
                <schedule:ScheduleAppointmentMapping
               SubjectMapping="EventName" 
               ColorMapping="Color"
               StartTimeMapping="From"
               EndTimeMapping="To">
                </schedule:ScheduleAppointmentMapping>
         </schedule:SfSchedule.AppointmentMapping>
</schedule:SfSchedule>
```
