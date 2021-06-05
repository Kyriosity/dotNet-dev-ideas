# MVVM and WPF: Getting on and guidelines
Model-View-ViewModel was known well before Microsoft(r) released its WPF ([MVP](https://martinfowler.com/eaaDev/uiArchs.html) is another name). Also WinForms can be on MVVM, since Binding technics in WPF controls are inhereted from there.  
However it was WPF that made a symbiosis with MVVM.
Very successfull ans stable Visual Studio 2010 was written from scratch in WPF on MVVM pattern.
 
## Model
Model (*M*) is NOT data. It's abstraction of data **and business**. Data may be 
+ a database, cloud, pipe, service
+ hardcoded, stubbed, mocked or randomized
+ persistent and not

There're could be no Model at all.&nbsp;&nbsp;<sup>**_mvw**</sup>
 
&nbsp;&nbsp;<sup>**_mvw**</sup><sub>&nbsp;&nbsp;A Google team suggested even a better concept (name) for its Angular - MVW. *Model-View-Whatever*.</sub>
 
## ViewModel
+ A rather good definition of ViewModel - *converter on steroids* - was done in [The Philosophies of MVVM](https://joshsmithonwpf.wordpress.com/2008/12/01/the-philosophies-of-mvvm/) 

+ ViewModel (*VM*) shall not know about View at all - any UI declarations here, like colors, sizes, layout, sound are principally wrong.  

Back to the *View*. When dealing with something complexer than literal values and objects, converters must render no UX artifacts, like color, size, font, *skin*.

+ It outght to be 'info', 'alarm', 'warning', 'off-time', 'off-line', 'low bandwidth', "untrusted". And the *View* shall care about turning them into proper UX elements.

## View
+ A *View* can be even not a view - but, say, a voice assistant. Thinking on how disabled people may use your application, isn't only sane, but may boost one's design comprehension. 
+ Write code-behind for views, provided no other technical solution possible in VieModel&nbsp;&nbsp;<sup>**_cs**</sup>.

+ XAML&nbsp;&nbsp;<sup>**_xaml**</sup> with every dev step gets quite bulky (like any markup language).
The rule of thumb - *divide et impera*. Divide into user controls, resources, elements. Even if these won't be re-used.

&nbsp;&nbsp;<sup>**_cs**</sup><sub>&nbsp;&nbsp;Though somehow in no one WPF projects i haven't touched these files.</sub>

&nbsp;&nbsp;<sup>**_xaml**</sup><sub>&nbsp;&nbsp;Apropos, pronounced *zammel*

## Converters
 
### Naming and parametrization
Consider usually met paired converters, like `RunStatusToBoolean` and `InvertedRunStatusToBoolean`.
What's repellent? 

A) It must be a single converter class where XAML can set a property like `Negate=true`  

B) Readability: the name should be better `IsRunning` or `IsRunningOrPending`, or smth else what even not developers will understand.
 
With more logically justified parameters in one converter even more class declarations can be spared with one hit.  
   
### Defaults for Null and unavailalbe values
Make use of `TargetNullValue` and `FallbackValue`. This will greatly help in finding binding bugs within development time, or clearly inform users.
 
```
<TextBox Text="{Binding UserName}
    TargetNullValue='!null value!' FallbackValue='!binding error!' />
```
 
NOTE: TargetNull and Fallback are also important settings for enabling of *actions*.  
Challenge: no idea how to workaround acync loading. Then both setting will mislead users. 

# Tools to use
[Snoop WPF](https://github.com/snoopwpf) is a freeware, and looks to be a mature, effective inspector.

## Frameworks
There're very expreinced players on the market like Telerik, Devexpress, Infragistics (you may suggest others). &nbsp;&nbsp;<sup>**_dx**</sup>

With annual licences e.g. for WPF components starting at $900 - it's a bargain for even the smallest commercial product, unless it's pure "Hello world".

### Frontend
Here not a question. Just use - do not invent the wheel.

It's where you get the best out-of-the-box: design, professional look&feel.
Their good developers focus on WPF questions, and you - on business logic, the application and UX. 

As well, there you get right in time support and motivated community. 
 
&nbsp;&nbsp;<sup>**_dx**</sup><sub>&nbsp;&nbsp;Devexpress has a very attractive motto "We are your team". Looks to be true.

### Backend
Here the compexity of the application must be considered. Simple applications need no complex structure at all.

For any big application either a steep lurning curve of the framework must be planned or one own developed.

Many backend frameworks (not only for .NET) miss the [KISS](https://en.wikipedia.org/wiki/KISS_principle) principle and could be an overkill.
