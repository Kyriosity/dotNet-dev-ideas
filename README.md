# General ideas for .NET development
Everything in this repository is the subject of IMHO (in my humble/honest opinion). I could have misunderstood or misssed some points as well. Errors and mistakes are possible.

With this said, any rational critic (review) is very welcome.

## Intro
I personally keep C#\.NET for the most expressive and powerfull language/platform for business applications. It's a flagship of Microsoft&#174; (enough said), where the ship is .NET and the flag is C#. 

It evolves nonstop, giving us up-to-date concise and readable syntax. Its team learns from drawbacks and advantages of other languages.

In my comprehension, the mission of C# is: you write clean code and .NET cares that it becomes effective executible. Though in C# one gets destructors, finalizers, garbage collecting, memory allocation, "door" to unsafe coding  - programming them doesn't look sound (unless it's very specific task or not conforming 3d party).

## Commercial frameworks
I've discussed them already [here](WPF-MVVM/Guidelines.md).

## Practical hints
That's what i've learned, like and follow.
### Negate with XOR
Following snippets may look attractive to negate (inverse) a boolean:
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

### *Out* can add up to readability 
```csharp
if (!pauseComplete(out var msRemaining))
   DoSomething();
```
### Benchmark with *using*
For simple logging, profiling put the benchmarking on <code>ctor</code> and <code>Dispose()</code> of the being *used*.
[CallerMemberName] in the constructor will prevent mistaken names of the *benchmarked*.
```csharp
    using (var benchmark = new Benchmark())
    {
        // benchmarked calls here
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
### Delete your temp data
The naming of *temporary* path and files is deceptive. It grows, unless you time up to time clean this folder on your own. Even prominent software put tons of waste there. &nbsp;&nbsp;<sup>**_win**</sup>

&nbsp;&nbsp;<sub><sup>**_win**</sup>&nbsp;&nbsp;Windows&trade; predictably won't care about these files, say, on restart.</sub>

If your application exchanges/stores big volumes of data through %tmp%, it make sense to think of the cleaning.
Deleting own files on exit isn't the best idea:
+ application may crash
+ you shoud distinguish between instances of the application
+ the logic for temp files could be compicated (e.g. a file shall remain by restart)

The solution is dead-simple: <code>FileOptions.DeleteOnClose</code>. In the snippet below a file won't be deleted if only power supply abruptly goes off:
```csharp
using (var fs = new FileStream(Path.GetTempFileName(), FileMode.Open,  FileAccess.ReadWrite, FileShare.None, 4096, FileOptions.DeleteOnClose)) {
           // payload here: reading or storing data into/from shared prop
}
// once the stream is closed the file will be deleted
```

### Naming
#### Props
I'd prefer to name a prop <code>Unid</code>, <code>UniqueId</code> when its value is unique at least within the domain. Otherwise <code>id</code>.

#### Null-coalescing 
Matter of taste but some would like tricks like that:
```csharp
_order = order?? throw new ArgumentNullException(nameof(order));
```
