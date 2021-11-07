# General ideas and guidelines for creating .NET applications
Everything in this repository is the subject of IMHO (in my humble opinion). I could have misunderstood or misssed some points as well. 

It may also contain errors and mistakes. Any rational critic (review) much welcome.

## Intro
I personally keep C#\.NET for the most expressive and powerfull language/platform for business applications. It's a flagman of Microsoft&#174; (enough said). It evolves nonstopr, giving us up-to-date concise and readable syntax. Its team learns from drawbacks and advantages of other languages.

In my comprehension, the mission of C# is: you write clean code and .NET cares that it becomes effective executible. Though this language has destructors, finalizers, garbage collecting, memory allocation - programming them doesn't look sound (unless it's very specific app), 

## Practical hints
### Negate with XOR
Following snippets may look attractive to negate (inverse) a boolean:
```csharp
var isLoading = true;
isLoading ^= true;
legacyObject.SomeLongModuleName.PoorlyNamedVariable1 ^= true;
// XOR with true explicitly tells us that this is inversion, and
// prevents typing errors (when one applies a var/prop with similar name)
```

### *Out* can look attractive
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
```
