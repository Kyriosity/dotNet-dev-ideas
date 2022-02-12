Everything here is the subject of IMHO (in my humble/honest opinion). I could have misunderstood, missed some points. Errors and mistakes are also possible.
 
With this said, any rational critic (review) is very welcome.
 
This doc intercepts with design notes in some other repositories.
 
## Foreword
C#\.NET deserves to be noted as one of the most expressive and powerfull language/platforms for business applications. It's a flagship of Microsoft&#174; (enough said), where the ship is .NET and the flag is C#.
 
It evolves nonstop, providing us with up-to-date, concise, comprehensible syntax. Its team learns from drawbacks and advantages of other languages and picks trade off between readability and brevity.
 
The mission of C# is: you write clean code and .NET cares that it gets translated into effective and safe executible. Though in C# one sees destructors, finalizers, garbage collecting, memory allocation, "front door" to unmanaged code  - programming them doesn't look sound (unless it's a very specific task, workaround of bizzare bugs or non-conforming 3d party).
 
## Frameworks and libraries
### Commercial
Some have been discussed already [here](WPF-MVVM/Guidelines.md).
### Freeware (MIT and similar licences)
Some projects on GitHub launched privately or for limited use have grown to industry standards and got adopted in mainstream languages. Remember [Newton.json](https://www.newtonsoft.com/json)
 
However when applying any 3d-party think if there's a community (at least you) - able to support and fix bugs, if contributor(s) aren't responding.
 
## .NET and Microsoft specific
What's often neglected, forgotten or remains unknown.
### Newest platform features
Microsoft quite often releases newer versions of .NET, worth to spend a day evaluating and remembering <i>what's new</i>.
If Microsoft notes aren't enough, there're plenty of smart guys, revieweing them in a detail.
 
### Tuples and records
C# devs mostly think in interfaces and objects, sometimes structs.
Nevertheless records and tuples gain more focus and power in recent C# versions.
 
#### Named tuples
A nice shortcut to return a complex values. Problem: can't cast them to interface or object.

#### Records
TBD
 
### Multitasking
Concerns over complexity and opacity divert some from taking adavantage of parallelism.
But look, are there semaphores, threads/pools in the next snippet?
```dif csharp
...
   var nats = Enumerable.Range(1, 28 * 1000 * 1000).ToArray();
   - foreach (var item in nats) 
   -   CalcHard(item);
   + Parallel.ForEach(nats, CalcHard); // worked twice faster on my notebook
...
   
static void CalcHard(int nat) {
   using var sha = SHA512.Create();
   _ = sha.ComputeHash(Encoding.UTF8.GetBytes(((int)Math.Sqrt(nat) / Math.Atan2(nat, nat)).ToString()));
 }
```
Going further with parallelism will require more know-hows but C# cares that you think in abstract terms.
The main rule is: test if multitasking spares time or provides responsible UI.

## Further manuscripts
[Practical hints for development](readme+/practical_hints.md)\
[Intro and guidelines for WPF](readme+/wpf_mvvm_intro.md)
