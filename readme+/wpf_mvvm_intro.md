# MVVM and WPF: Intro and guidelines
Notably successfull and stable Visual Studio 2010 was written from scratch in WPF relying on **MVVM** (Model-View-ViewModel). However this pattern was known as [MVP](https://martinfowler.com/eaaDev/uiArchs.html) well before Microsoft(r) released its WPF.
 
WinForms application can be an exemplary MVVM&nbsp;<sup>**_bind**</sup> but it was WPF that made a symbiosis with MVVM.

&nbsp;&nbsp;<sup>**_bind**</sup><sub>&nbsp;&nbsp;*Binding* "glue" in WPF controls comes from there.</sub>

## Model
Model (*M*) is NOT data. It's abstraction of data **and business**. Data may be 
+ a database, cloud, pipe, service, queue,
+ hardcoded, stubbed, mocked or randomized,
+ persistent or transient,
+ plain or decorated with biz logic

There're could be no Model at all.
 
## ViewModel
+ A rather good definition of ViewModel - *converter on steroids* - was done in [The Philosophies of MVVM](https://joshsmithonwpf.wordpress.com/2008/12/01/the-philosophies-of-mvvm/) 

+ ViewModel (*VM*) shall not know about View at all - any UI declarations here, like colors, sizes, layout, sound are principally wrong.  

Back to the *View*. When dealing with something complexer than literal values and objects, converters must render no UX artifacts, like color, size, font, *skin*.

+ It outght to be 'info', 'alarm', 'warning', 'off-time', 'off-line', 'low bandwidth', "untrusted". And the *View* shall care about turning them into proper UX elements.


&nbsp;<sup>**_mvw**</sup>
 
&nbsp;&nbsp;<sup>**_mvw**</sup><sub>&nbsp;&nbsp;A Google team suggested even a better concept (name) for its Angular - MVW. *Model-View-Whatever*.</sub>

## View
+ A *View* can be even not a view - but, say, a voice assistant. Thinking on how disabled people may use your application, isn't only sane, but may boost one's design comprehension. 
+ Write code-behind for views, provided no other technical solution possible in VieModel&nbsp;<sup>**_cs**</sup>.

+ XAML&nbsp;<sup>**_xaml**</sup> with every dev step gets quite bulky (like any markup language).
The rule of thumb - *divide et impera*. Divide into user controls, resources, elements. Even if these won't be re-used.

&nbsp;&nbsp;<sup>**_cs**</sup><sub>&nbsp;&nbsp;somehow in manifold of WPF projects i never touched these files</sub>

&nbsp;&nbsp;<sup>**_xaml**</sup><sub>&nbsp;&nbsp;apropos, pronounced *zammel*

## WPF Converters
 
### Parametrization
Along with other built-in converters WPF brings [BooleanToVisibilityConverter](https://docs.microsoft.com/de-de/dotnet/api/system.windows.controls.booleantovisibilityconverter). 
 
What if i'd like that not `true` but `false` visibility. What if i need something out of eyes as `Collapsed` instead of `Hidden`?

Shall i add converters (e.g. prefixed *Inverted*) or/and variables in ViewModel? Or there're could be a better solution:
 

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
and then ...
 ```
<local:BoolToVisibilityConverter x:Key="BoolToVisibility" />
<local:BoolToVisibilityConverter x:Key="InvertedBoolToVisibility" Invert="True" />
<local:BoolToVisibilityConverter x:Key="BoolToCollapsed" CollapseNotHide="True"/>
 ```
With more logically justified parameters in one converter even more class declarations can be spared with one hit.  

## Proposals
### Defaults for Null and unavailable values
Make use of `TargetNullValue` and `FallbackValue`, and this will greatly help to find binding bugs in development time, or clearly inform users.

 ```
<Label Content="{Binding UserName, 
        NullValue='!null value!', FallbackValue='!binding error!' }"/>
```
NOTE: TargetNull and Fallback are also important for enabling of actions/commands.  
Challenge: no idea how to workaround acync loading. When both setting will mislead users. 

# Tools to use
[Snoop WPF](https://github.com/snoopwpf) is a freeware, mature and effective inspector.

## Frameworks
There're more than experienced providers of suites, frameworks, controls for RAD&nbsp;<sup>**
 **</sup>. Telerik, Devexpress, Infragistics - to name a few. &nbsp;<sup>**_dx**</sup>

With annual licences for WPF components starting from $900 - it's a good buy for even the smallest commercial product, unless it's pure "Hello world".

&nbsp;&nbsp;<sup>**_rad**</sup><sub>&nbsp;&nbsp;Rapid Application Development
 
&nbsp;&nbsp;<sup>**_dx**</sup><sub>&nbsp;&nbsp;Devexpress's eloquent "We are your team" sounds realistic.

### Frontend
Not in question. When application needs one (from scratch, to replace obsolete), we shan't invent the wheel (unless it's rounder than others).

It's where you get the best out-of-the-box: design, professional look&feel.
Their team focuses on WPF questions, and you - on business logic, the application and UX. 

As well, there you get right in time support and motivated community. 

### Backend
Simple applications need no elaborated structure. The easiest MVVM starts from the declaration of a single `class` and setting it as `datacontext` for the window.

For any big one application either time for a steep lurning curve of the adopted framework must be planned or one own developed.

Quite many backend frameworks (not only for .NET) are good at themselves but may deprive your application of the [KISS](https://en.wikipedia.org/wiki/KISS_principle) principle and add up too much unnecessary "ballast" code.
