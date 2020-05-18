# aar-r8-interface-obfuscation

This example illustates a problem that we have not been able to solve, when using R8 obfuscation for an Android AAR.

If creating a public interface at com.example.obfuscationexample.MyInterface:

```
interface MyInterface{
    fun myFunction1()
}
```

and using the obfuscation rule in proguard-rules.pro:
```
-keep class com.example.obfuscationexample.MyInterface { *; }
```

this is nicely preserved from obfuscation and kept in the result AAR. This is needed when for example constructing a public API for the AAR.

If adding a private interface to com.example.obfuscationexample.internal.MyInterfaceInternal with one of its methods named identical:

```
interface MyInterfaceInternal{
    fun myFunction1()
    fun myObfuscatedFunction()
}
```

and using rule:
```
-keep, allowobfuscation class com.example.obfuscationexample.internal.MyInterfaceInternal { *; }
```

only the interface name and method, which is not identically named as the one in the public interface, are obfuscated.

If however setting in gradle.properties the flag
```
android.enableR8=false
```

making the build using Proguard instead of R8, both of the indented private methods are indeed obfuscated.

Is there another set of rules for R8 to behave similar to Proguard in this case?