# MVVM and WPF: Getting on and guidelines
Model-View-ViewModel was known well before Microsoft(r) released its WPF ([MVP](https://martinfowler.com/eaaDev/uiArchs.html) is another name). 
Successfull and stable Visual Studio 2010 was written from scratch in WPF on MVVM pattern.

Also a WinForms application can be an exemplary MVVM - *Binding* "glue" in WPF controls comes from there -   
however it was WPF that made a symbiosis with MVVM.

 
## Model
Model (*M*) is NOT data. It's abstraction of data **and business**. Data may be 
+ a database, cloud, pipe, service
+ hardcoded, stubbed, mocked or random
+ persistent or/and not

There're could be no Model at all.&nbsp;&nbsp;<sup>**_mvw**</sup>
 
&nbsp;&nbsp;<sup>**_mvw**</sup><sub>&nbsp;&nbsp;A Google team suggested even a better concept (name) for its Angular - MVW. *Model-View-Whatever*.</sub>
 
## ViewModel
+ A rather good definition of ViewModel - *converter on steroids* - was done in [The Philosophies of MVVM](https://joshsmithonwpf.wordpress.com/2008/12/01/the-philosophies-of-mvvm/) 

+ ViewModel (*VM*) shall not know about View at all - any UI declarations there (colors/themes, sizes, layout, acoustic output/input) are principally wrong.

ViewModel must render no UX decorations, but something like 'info', 'error', 'warning', 'off-time', 'off-line', 'low bandwidth', "untrusted". And it's the job of *View* to care about turning them into proper UX elements (e.g. icons, or red color for error label).

## View
+ A *View* can be even not a view but, say, a voice assistant. Keeping in mind how disabled people may use your application is not only sane, but will boost design comprehension. 
+ Write code-behind for views, provided no other technical solution possible in VieModel&nbsp;&nbsp;<sup>**_cs**</sup>. Datacontext, when set in code, is hardly good practice.

+ On one following commit XAML&nbsp;&nbsp;<sup>**_xaml**</sup> can become very bulky (like any markup language).
The rule of thumb - *divide et impera*. Divide into user controls, resources, elements. Even if these won't be re-used - otherwise it will be always a challenge to read and navigate in your XAML.

&nbsp;&nbsp;<sup>**_cs**</sup><sub>&nbsp;&nbsp;Somehow in no WPF project i ever touched these files.</sub>

&nbsp;&nbsp;<sup>**_xaml**</sup><sub>&nbsp;&nbsp;Apropos, pronounced *zammel*

## WPF Converters
 
### Parametrization
WPF brings the built-in [BooleanToVisibilityConverter](https://docs.microsoft.com/de-de/dotnet/api/system.windows.controls.booleantovisibilityconverter) in WPF. 
What if i'd like that not *true* but *false* visibility - shall i add a converter (usually prefixed *Inverted*) or a variable in ViewModel? 
What if i'd like not to see as Collapsed instead of Hidden.

```csharp
public class BoolToVisibilityConverter : IValueConverter
{
    public bool Invert { get; set; }
    public bool CollapseNotHide { get; set; }

    public object Convert(object value, Type targetType, object parameter, CultureInfo culture) {
        if (!(value is bool show))
             return DependencyProperty.UnsetValue;

        var hideOption = CollapseNotHide ? Visibility.Collapsed : Visibility.Hidden;
        return show ^ Invert ? Visibility.Visible : hideOption;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture) {
       return Binding.DoNothing;
    }
}
``` 
And now ...
 ```
<local:BoolToVisibilityConverter x:Key="BoolToVisibility" />
<local:BoolToVisibilityConverter x:Key="InvertedBoolToVisibility" Invert="True" />
<local:BoolToVisibilityConverter x:Key="BoolToCollapsed" CollapseNotHide="True"/>
 ```
With more logically justified parameters in one converter even more class declarations can be spared with one hit.  

## Proposals
### Defaults for Null and unavailable values
Make use of `TargetNullValue` and `FallbackValue`, and this will greatly help in finding binding bugs by development, or clearly inform users.
 

 ```
< Text="{Binding UserName}
 tNullValue='!null value!' FallbackValue='!binding error!' />
```
`NOTE: TargetNull and Fallback are also important for enabling of *actions*.  
Challenge: I have no idea how to workaround acync loading. When both setting will mislead users. 

# Tools to use
[Snoop WPF](https://github.com/snoopwpf) is a freeware, and looks to be a mature, effective inspector.

## Frameworks
There're more than experienced providers of suites, frameworks, controls for RAD&nbsp;&nbsp;<sup>**_rad**</sup>. Telerik, Devexpress, Infragistics - to name a few. &nbsp;&nbsp;<sup>**_dx**</sup>

With annual licences e.g. for WPF components starting at $900 - it's a bargain for even the smallest commercial product, unless it's mere "Hello world".

&nbsp;&nbsp;<sup>**_rad**</sup><sub>&nbsp;&nbsp;Rapid Application Development

&nbsp;&nbsp;<sup>**_dx**</sup><sub>&nbsp;&nbsp;Devexpress's eloquent "We are your team" sounds realistic.

### Frontend
Not in question. We shall not invent the wheel (unless it's rounder than others).

It's where you get the best out-of-the-box: design, professional look&feel.
Their team focuses on WPF questions, and you - on business logic, the application and UX. 

As well, there you get right in time support and motivated community. 
 

### Backend
Simple applications need no elaborated structure at all.

For any big one either time for a steep lurning curve of the adopted framework must be planned or one own developed.

Quite many backend frameworks (not only for .NET) are good at themselves but may deprive your application of the [KISS](https://en.wikipedia.org/wiki/KISS_principle) principle and add up too much unnecessary "ballast" code.
