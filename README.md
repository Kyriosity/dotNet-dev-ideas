# General ideas and guidelines for creating .NET applications
Everything in this repository is the subject of IMHO (in my humble opinion). I could have misunderstood or misssed some points as well. 

It may also contain errors and mistakes. Any rational critic (review) much welcome.

## Intro
I personally keep C#\.NET for the most expressive and powerfull language/platform for business applications. It's a flagman of Microsoft&#174; (enough said). It evolves, constantly bringing us up-to-date concise and readable syntax. Its team learns from drawbacks and advantages of other languages.

In my comprehension, the mission of C# is: you write clean code and .NET cares that it becomes effective. 

## Practical hints
### Negate
Following snippets may look attractive to negate (inverse) a boolean:
```csharp
var isLoading = true;
isLoading ^= true;

legacyObject.SomeLongModuleName.PoorlyNamedVariable1 ^= true;
```

