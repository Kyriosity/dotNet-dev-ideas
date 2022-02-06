
Everything here is the subject of IMHO (in my humble/honest opinion). I could have misunderstood, missed some points. Errors and mistakes are also possible.

With this said, any rational critic (review) is very welcome.

General ideas you may find in the repository convert-smth (dev_concerns.md).

## Foreword
C#\.NET deserves to be noted as one of the most expressive and powerfull language/platforms for business applications. It's a flagship of Microsoft&#174; (enough said), where the ship is .NET and the flag is C#.

It evolves nonstop, giving us up-to-date concise and comprehensible syntax. Its team learns from drawbacks and advantages of other languages and finds trade off between readability and brevity.

In my comprehension, the mission of C# is: you write clean code and .NET cares that it gets translated into effective and safe executible. Though in C# one gets destructors, finalizers, garbage collecting, memory allocation, "front door" to unmanaged code  - programming them doesn't look sound (unless it's very specific task, workaround of bizzare bugs  or non-conforming 3d party).

## Frameworks and libraries
### Commercial
I've discussed some already [here](WPF-MVVM/Guidelines.md).
### Freeware (MIT and similar licences)
Some projects on GitHub launched by a single person have grown to industry standards and got adopted in mainstream languages. Remember Newton.json.

However when using any think if somebody other or you will be able to support and fix bugs in non-commercial 3d party, if its contributor(s) are not responding.

## .NET and Microsoft specific
What's often neglected, forgotten or remains unknown.
### Newest platform features
Microsoft quite often releases newer versions of .NET.
It's worth to spend a day evaluating and remembering <i>what's new</i>.

### Third party tools
#### LINQKit and PredicateBuilder
From the author of C# in a Nutshell. Are worth of review.
### Fluent assertions and SpecFlow

### Tuples and records
C# devs mostly think in interfaces and objects, sometimes structs.
Nevertheless records and tuples gain focus in recent C# versions.

#### Named tuples
A nice shortcut to return a complex values.

### Multithreading
Quite often treated as unsafe, hard to learn and debug = in fact it's out-of=the box, easy and really advantage.
Just test how reliable and effective is `Parallel.ForEach()`.

#### Record classes

## Further reading
Big enough to list them separately 
[Practical hints for development](readme+/practical_hints.md)
