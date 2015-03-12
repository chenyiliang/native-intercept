# Usage: #

In order to intercept native methods, the `NativeInterceptorAgent` must be attached prior to loading the classes with native methods. This can be accomplished in two ways...

### Attaching via VM arguments ###

`> java -javaagent:path/to/native-interceptor-`_version_`.jar -classpath ...:path/to/native-interceptor-`_version_`.jar com.example.YourClass`

(note: the native-interceptor jar is both a classpath entry and the -javaagent VM arg...this may not be strictly necessary, but this is what has been tested)

### Attaching to a running VM ###

  * Add `native-interceptor-`_version_`.jar` to the classpath.
  * Call `NativeInterceptorAgent.enable();` prior to loading any classes that need to be intercepted.

# FAQ: #

  1. What can I do with `NativeInterceptor`?
> > Intercept native methods prior to invoking native code. This is useful in situations where you do not have the native code available (`*`cough`*`GWT`*`cough`*`) or in tests for code that depends on native code when the test covers only the Java portion of the code.
  1. I get an exception that tells me to check the documentation, what's wrong?
> > That means the agent was not applied prior to the loading of the class that needs to be intercepted. The documentation is not very thorough, but the above usage instructions cover this subject briefly.
  1. I get the error: `java.lang.NoClassDefFoundError: com/sun/tools/attach/...`
> > This library uses APIs included in the `tools.jar` jar file included with the Windows and Linux JDKs (`JDK_HOME/lib/tools.jar`) and that jar is likely missing from the classpath. This does not apply to OS X JDKs as the APIs in the `tools.jar` are packaged in the main `classes.jar` file.
  1. Does `NativeInterceptor` require Java 1.6?
> > Not entirely. While the Java agent functionality was added in Java 1.5, the ability to add a native prefix was added in 1.6. `NativeInterceptor` now wraps Java 1.6 calls inside of a check on the Java version. This means it should be possible to use with Java 1.5. In this case, however, native methods in candidate classes that are not intercepted will not resolve correctly to their native implementations.