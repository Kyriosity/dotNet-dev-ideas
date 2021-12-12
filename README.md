Everything here is the subject of IMHO (in my humble/honest opinion). I could have misunderstood, missed some points. Errors and mistakes are also possible.

With this said, any rational critic (review) is very welcome.

General ideas you may find in the repository convert-smth (dev_concerns.md).

## Foreword
I personally keep C#\.NET for the most expressive and powerfull language/platform for business applications. It's a flagship of Microsoft&#174; (enough said), where the ship is .NET and the flag is C#. 

It evolves nonstop, giving us up-to-date concise and readable syntax. Its team learns from drawbacks and advantages of other languages and finds trade off between readability and brevity.

In my comprehension, the mission of C# is: you write clean code and .NET cares that it becomes effective executible. Though in C# one gets destructors, finalizers, garbage collecting, memory allocation, "front door" to unsafe code  - programming them doesn't look sound (unless it's very specific task or not conforming 3d party).

## Frameworks and libraries
### Commercial
I've discussed some already [here](WPF-MVVM/Guidelines.md).
### Freeware (MIT and similar licences)
Some projects on GitHub launched by a single person have grown to industry standards and got adopted in mainstream languages. Remember Newton.json.

However when using any think if somebody other or you will be able to support and fix bugs in non-commercial 3d party, if its contributor(s) are not responding.

### .NET and Microsoft
#### Newest language features
Microsoft quire often releases newer versions of the platform and language.
It's worth to spend a day evaluating and remembering them.

#### LINQKit and PredicateBuilder
These are often neglected or remain unknown.

## Practical hints

<details>
<summary><b>Negate with XOR</b></summary>

```diff csharp
var isLoading = false;
// ...
-     isLoading = !isLoading
+     isLoading ^= true;
// XOR with true explicitly tells that this is inversion

-    legacyObject.SomeLongModuleName.PoorlyNamedVariable1 = !legacyObject.SomeLongModuleName.PoorlyNamedVariable1;
+    legacyObject.SomeLongModuleName.PoorlyNamedVariable1 ^= true;
// terser and prevents typing errors (when one applies a var/prop with similar name)
```
   
</details>

<details>
<summary><b><i>out</i> for readability</b></summary>

```csharp
if (!pauseComplete(out var msRemaining))
   module.Sleep(msRemaining);
```
</details>

<details>
<summary><b>Null-coalescing</b></summary>
 
Matter of taste but some love tricks like that:
```csharp
_order = order?? throw new ArgumentNullException(nameof(order));
```
</details>

<details>
<summary><b>Benchmark with <i>using</i></b></summary>
For dead-simple logging, profiling put the benchmarking on <code>ctor</code> and <code>Dispose()</code> of the being *used*.
   
<code>[CallerMemberName]</code> in the constructor will prevent mistaken names of the being *benchmarked*.

```csharp
using (var benchmark = new Benchmark())
{
    // benchmarked stuff here
}

class Benchmark : IDisposable
{
   string _caller;
   public Benchmark([CallerMemberName] string caller = "<undefined>") {
      _caller = caller;
      // start log here id-d with _caller
   }

   public void Dispose() {
       // finish log here id-d with _caller
   }
}
```
</details>
   
<details>
<summary><b>Name "magic" values</b></summary>
   
```diff csharp
-     legacySystem.ModuleD1.Abracadabra = true; // specifies that text input is treated culture-insensitive
+     bool IsInputCultureInvariant = ...
+     legacySystem.ModuleD1.Abracadabra = IsInputCultureInvariant;
   
-     popup(shortMessage).ShowFor(3200);
+     popup(shortMessage).ShowFor(Ux.MinimumMsToReadPrompt);
```
</details>
   
<details>
<summary><b>Clean your temp files</b></summary>

The naming of *temporary* folder (and files) is deceptive. It grows, unless you time up to time clean this folder on your own. Even prominent software put tons of waste there. &nbsp;&nbsp;<sup>**_win**</sup>

&nbsp;&nbsp;<sub><sup>**_win**</sup>&nbsp;&nbsp;And Windows&trade; predictably won't care about these files, say, on restart.</sub>

If your application exchanges/stores big volumes of data through %tmp%, it make sense to remove them too.
Keeping track of created temp files and deleting them on exit (e.g. flushing specific subfolder) isn't the award-winning idea:
+ application may crash
+ you shoud distinguish between instances of the same application
+ the logic for temp files could be compicated (e.g. biz process on the end of application)

<code>FileOptions.DeleteOnClose</code> may be suitable. In the snippet below a file won't be deleted if only power supply abruptly goes off:
```csharp
using (var fs = new FileStream(Path.GetTempFileName(), FileMode.Open, 
          FileAccess.ReadWrite, FileShare.None, 4096, FileOptions.DeleteOnClose)) {
               // payload here: reading or storing data into/from shared prop
}
```
Nice to develop a kind of wrapper, through which data is sent/read to/from temp storage.
</details>
