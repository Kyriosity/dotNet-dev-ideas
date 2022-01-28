# .NET Development - Practical hints
## Coding
<details>
<summary><b>negate with XOR</b></summary>

 

```diff csharp

var isLoading = false;

// ...

-     isLoading = !isLoading

+     isLoading ^= true; // this explicitly tells of inversion

 

-    legacyObject.SomeLongModuleName.PoorlyNamedVariableA1c = !legacyObject.SomeLongModuleName.PoorlyNamedVariableAlc;

+    legacyObject.SomeLongModuleName.PoorlyNamedVariable1 ^= true;

// terser and prevents typing errors (when one applies a var/prop with similar name)

```

</details>

 

<details>

<summary><code>out</code> <b>for readability</b></summary>

 

```csharp

if (!pauseComplete(out var msRemaining))

   module.Sleep(msRemaining);

```

</details>

 

<details>

<summary><b>condense with null-coalescing</b></summary>

This will spare at least a line.

   

```csharp

_order = order?? throw new ArgumentNullException(nameof(order));

```

</details>

 

<details>

<summary><b>benchmark easy with</b> <code>using</code></summary>

For straightforward logging/profiling use <code>ctor</code> and <code>Dispose()</code> of a being *used* benchmark.

   

<code>[CallerMemberName]</code> in the constructor will prevent mistaken names of the being *benchmarked*.

 

```csharp

using (var benchmark = new Benchmark()) {
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

<summary><b>named tuples for return</b></summary>

Even in the stricktest OOD does not manadate to declare objects for every trifle.

```csharp

 

```

</details>

   

<details>

<summary><code>nameof</code><b> is good, <code></b>CallerMemberName</code> <b>may be better</b></summary>

 

```diff csharp

+   public

-   private

```  

</details>

 

<details>

<summary><b>name "magic" values</b></summary>

   

```diff csharp

-     legacySystem.ModuleD1.Abracadabra = true; // specifies that text input is treated culture-insensitive

+     bool IsInputCultureInvariant = ...

+     legacySystem.ModuleD1.Abracadabra = IsInputCultureInvariant;

   

-     popup(shortMessage).ShowFor(3200);

+     popup(shortMessage).ShowFor(Ux.Milliseconds.MinToReadPrompt);

```

</details>
## Good Parts
<details>

<summary><b>сlean temp files</b></summary>

 

The naming of *temporary* folder (and files) is deceptive. It grows, unless you time up to time clean this folder on your own. Even prominent applications put tons of waste there. &nbsp;&nbsp;<sup>**_win**</sup>

 

&nbsp;&nbsp;<sub><sup>**_win**</sup>&nbsp;&nbsp;And Windows&trade; predictably won't care about these files, say, on restart.</sub>

 

If your application exchanges/stores big volumes of data through %tmp%, it's nice .

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
 
## Styling
  ### Curly braces
Avoid braces for one line statement - indent does the job
```csharp
if (a == b)
    i++; // do we really need braces here?

apply(i);
```

Use in-line opening brace as in JS/Typescript 
```csharp
if (a == b) { // one line less but readability not hurt
    i++;
    j++;
}
```
### Strings
Avoid comparing with empty, whitespace or null. Just use `IsEmpty` or `IsNullOrWhitespace`


Do not compare with <code>=</code>
  
```csharp

```

### Enums
If a value from enum shall be explicitly set, reserve undefined value. This will 
```csharp
enum Start {
  Undefined,
  Ready,
  Steady,
  Go,
  Finished
}
```

