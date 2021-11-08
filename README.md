# General ideas for .NET development
Everything in this repository is the subject of IMHO (in my humble opinion). I could have misunderstood or misssed some points as well.  It may also contain errors and mistakes. 

With this said, any rational critic (review) is very welcome.

## Intro
I personally keep C#\.NET for the most expressive and powerfull language/platform for business applications. It's a flagship of Microsoft&#174; (enough said), where the ship is .NET and the flag is C#. 

It evolves nonstop, giving us up-to-date concise and readable syntax. Its team learns from drawbacks and advantages of other languages.

In my comprehension, the mission of C# is: you write clean code and .NET cares that it becomes effective executible. Though in C# one gets destructors, finalizers, garbage collecting, memory allocation, "door" to unsafe coding  - programming them doesn't look sound (unless it's very specific task or not conforming 3d party).

## Practical hints
That's that i learned, like and follow.
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
### Use *using* for benchmarks
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
      // log start here
   }

   public void Dispose() {
       // log finishe here with _caller id
   }
}
```
