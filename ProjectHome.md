# New version 0.1.2 #

There is a new version after quite a long period of inactivity (almost two years actually) - grab it here: **http://runsharp.googlecode.com/files/runsharp-0.1.2-pre-alpha-src.zip**.

Don't be misled by the small number change! This is what's going to be released as 0.2 after documentation comments are finally added, and a few bugs are fixed.

# Overview #

RunSharp is a layer above the standard .NET Reflection.Emit API, allowing to generate/compile dynamic code at runtime very quickly and efficiently (unlike using CodeDOM and invoking the C# compiler). To the best of my knowledge, there is no such library available at the moment.

# Example #

A simple hello world example in C#

```
public class Test
{
   public static void Main(string[] args)
   {
      Console.WriteLine("Hello " + args[0]);
   }
}
```

can be dynamically generated using RunSharp as follows:

```
AssemblyGen ag = new AssemblyGen("hello.exe");
TypeGen Test = ag.Public.Class("Test");
{
   CodeGen g = Test.Public.Static.Method(typeof(void), "Main", typeof(string[]));
   {
      Operand args = g.Param(0, "args");
      g.Invoke(typeof(Console), "WriteLine", "Hello " + args[0] + "!");
   }
}
ag.Save();
```

The above code should generate roughly the same assembly as if the first example was compiled using `csc`.