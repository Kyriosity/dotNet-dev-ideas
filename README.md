# General ideas for .NET development
Everything in this repository is the subject of **IMHO** and may look **banal**. I could have misunderstood or missed some points as well. Errors and mistakes are possible. 

With this said, any rational critic (review) is very welcome.

&nbsp;&nbsp;<sub><sup>**_imho**</sup>&nbsp;&nbsp;**I**n **M**y **H**umble **O**pinion</sub>

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

## Traps
It could be trivia for many but i met following flows im some projects, and keep them as a reminder (or checklist)
### Recursion
"Deep" recursive calls may lead to memory leaks. It's easy to forget about that.

### LINQ limitations
LINQ is a nice tool but on large data volumes it's a performance curse.

### Improper multithreading in UI
When a thread is the choice to auto-save a doc, prove something on-the-fly, it will raise access exceptions (sometimes silent).

The solution is to use Idle slots in UI Dispatcher (this way spellcheck works in Microsoft&reg; Word&trade;).

### Overall error handling
..
Handle only what you know, i.e. application specific exceptions. "Eating" Out-of-memory or disk exceptions may make a big fat system bug.

### Replacing NullException with check
Null pointer exceptions are known as *billion dollar mistake* (and the same value prize for guys with criminal energy). But next workarounds are even worse.
```csharp
if (person is null)
      return;
      
if (person.HasWonJackpot && person.EMail is not null)
       NotifyImmediately(person);
```
Ideas ?
+ Constraints
+ 
