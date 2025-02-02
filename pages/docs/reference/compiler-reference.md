---
type: doc
layout: reference
title: "Kotlin Compiler Options"
---

# Kotlin Compiler Options

Each release of Kotlin includes compilers for the supported targets: 
JVM, JavaScript, and native binaries for [supported platforms](native-overview.html#target-platforms).

These compilers are used by the IDE when you push the __Compile__ or __Run__ button for your Kotlin project.  

You can also run Kotlin compilers manually from the command line as described 
in the [Working with command-line compiler](/docs/tutorials/command-line.html) tutorial. For example: 

<div class="sample" markdown="1" mode="shell" theme="idea">

```bash
$ kotlinc hello.kt -include-runtime -d hello.jar
```

</div>
 
## Compiler options

Kotlin compilers have a number of options for tailoring the compiling process.
Compiler options for different targets are listed on this page together with a description of each one.

There are several ways to set the compiler options:
- In IntelliJ IDEA, write in the options in the __Additional command-line parameters__ text box in 
__Settings | Build, Execution, Deployment | Compilers | Kotlin Compiler__
- If you're using Gradle, specify the options in the `kotlinOptions` property of the Kotlin compilation task.
For details, see [Using Gradle](using-gradle.html#compiler-options).
- If you're using Maven, specify the options in the `<configuration>` element of the Maven plugin node. 
For details, see [Using Maven](using-maven.html#specifying-compiler-options).
- If you run a command-line compiler, add the options directly to the utility call or write them into an [argfile](#argfile).

## Common options

The following options are common for all Kotlin compilers.

### `-version` 
* Display the compiler version.

### `-nowarn`
* Suppress the compiler from displaying warnings during compilation.

### `-Werror`
* Turn any warnings into a compilation error. 

### `-verbose`
* Enable verbose logging output which includes details of the compilation process.

### `-help` (`-h`)
* Display usage information and exit. Only standard options are shown.
To show advanced options, use `-X`.

### `-X`
* Display information about the advanced options and exit. These options are currently unstable: 
their names and behavior may be changed without notice.

### `-kotlin-home <path>`
* Specify a custom path to the Kotlin compiler used for the discovery of runtime libraries.
  
### `-P plugin:<pluginId>:<optionName>=<value>`
* Pass an option to a Kotlin compiler plugin.
Available plugins and their options are listed in [Compiler plugins](compiler-plugins.html).
  
### `-language-version <version>`
* Provide source compatibility with the specified version of Kotlin.

### `-api-version <version>`
* Allow using declarations only from the specified version of Kotlin bundled libraries.

### `-progressive`
* Enable the [progressive mode](whatsnew13.html#progressive-mode) for the compiler.
    
    In the progressive mode, deprecations and bug fixes for unstable code take effect immediately,
    instead of going through a graceful migration cycle.
    Code written in the progressive mode is backwards compatible; however, code written in
    a non-progressive mode may cause compilation errors in the progressive mode.
    
{:#argfile}

### `@<argfile>`
* Read the compiler options from the given file. A file passed in this option can contain compiler options with values 
and paths to the source files. For example:

    <div class="sample" markdown="1" mode="shell" theme="idea">
    
    ```
    -d hello.jar -include-runtime
    hello.kt
    ```
    
    </div>

    You can also separate compiler options from source file names and pass multiple argument files.

    <div class="sample" markdown="1" mode="shell" theme="idea">
    
    ```bash
    $ kotlinc @compiler.options @classes
    ```
    
    </div>
    
    If the files reside in locations different from the current directory, use relative paths. 
    
    <div class="sample" markdown="1" mode="shell" theme="idea">
    
    ```bash
    $ kotlinc @options/compiler.options hello.kt
    ```
    
    </div>

    
## JVM compiler options

The Kotlin compiler for JVM compiles Kotlin source files into Java class files. 
The command-line tools for Kotlin to JVM compilation are `kotlinc` and `kotlinc-jvm`.
You can also use them for executing Kotlin script files.

In addition to the [common options](#common-options), Kotlin/JVM compiler has the options listed below.

### `-classpath <path>` (`-cp <path>`)
* Search for class files in the specified paths. Separate elements of the classpath with system path separators (**;** on Windows, **:** on macOS/Linux).
The classpath can contain file and directory paths, ZIP, or JAR files.

### `-d <path>`
* Place the generated class files into the specified location. The location can be a directory, a ZIP, or a JAR file. 

### `-include-runtime`
* Include the Kotlin runtime into the resulting JAR file. Makes the resulting archive runnable on any Java-enabled 
environment.

### `-jdk-home <path>`
* Use a custom JDK home directory to include into the classpath if it differs from the default `JAVA_HOME`.

### `-jvm-target <version>`
* Specify the target version of the generated JVM bytecode. Possible values are `1.6`, `1.8`, `9`, `10`, `11`, and `12`.
 The default value is `1.6`.

### `-java-parameters`
* Generate metadata for Java 1.8 reflection on method parameters.

### `-module-name <name>`
* Set a custom name for the generated `.kotlin_module` file.
  
### `-no-jdk`
* Don't automatically include the Java runtime into the classpath.

### `-no-reflect`
* Don't automatically include the Kotlin reflection (`kotlin-reflect.jar`) into the classpath.
  
### `-no-stdlib`
* Don't automatically include the Kotlin/JVM stdlib (`kotlin-stdlib.jar`) and Kotlin reflection (`kotlin-reflect.jar`)
 into the classpath. 
  
### `-script`
* Evaluate a Kotlin script file. When called with this option, the compiler executes the first Kotlin script (`*.kts`) 
file among the given arguments.
  
### `-script-templates <classnames[,]>`
* Script definition template classes. Use fully qualified class names and separate them with commas (**,**).


## Kotlin/JS compiler options

The Kotlin compiler for JS compiles Kotlin source files into JavaScript code. 
The command-line tool for Kotlin to JS compilation is `kotlinc-js`.

In addition to the [common options](#common-options), Kotlin/JS compiler has the options listed below.

### `-libraries <path>`

- Paths to Kotlin libraries with `.meta.js` and `.kjsm` files, separated by the system path separator.

### `-main {call|noCall}`

- Define whether the `main` function should be called upon execution.

### `-meta-info`

- Generate `.meta.js` and `.kjsm` files with metadata. Use this option when creating a JS library.

### `-module-kind {plain|amd|commonjs|umd}`

- The kind of JS module generated by the compiler:
    - `plain` - a plain JS module;
    - `commonjs` - a [CommonJS](http://www.commonjs.org/) module;
    - `amd` - an [Asynchronous Module Definition](https://en.wikipedia.org/wiki/Asynchronous_module_definition) module;
    - `umd` - a [Universal Module Definition](https://github.com/umdjs/umd) module.
    
    To learn more about the different kinds of JS module and the distinctions between them,
    see [this](https://www.davidbcalhoun.com/2014/what-is-amd-commonjs-and-umd/) article.
    
### `-no-stdlib`

- Don't automatically include the default Kotlin/JS stdlib into the compilation dependencies.

### `-output <filepath>`

- Set the destination file for the compilation result. The value must be a path to a `.js` file including its name.

### `-output-postfix <filepath>`

- Add the content of the specified file to the end of the output file.

### `-output-prefix <filepath>`

- Add the content of the specified file to the beginning of the output file.

### `-source-map`

- Generate the source map.

### `-source-map-base-dirs <path>`

- Use the specified paths as base directories. Base directories are used for calculating relative paths in the source map.

### `-source-map-embed-sources {always|never|inlining}`

- Embed source files into the source map.

### `-source-map-prefix`

- Add the specified prefix to paths in the source map.


## Kotlin/Native compiler options

Kotlin/Native compiler compiles Kotlin source files into native binaries for the [supported platforms](native-overview.html#target-platforms). 
The command-line tool for Kotlin/Native compilation is `kotlinc-native`.

In addition to the [common options](#common-options), Kotlin/Native compiler has the options listed below.


### `-enable-assertions` (`-ea`)

- Enable runtime assertions in the generated code.
    
### `-g`

- Enable emitting debug information.
    
### `-generate-test-runner` (`-tr`)

- Produce an application for running unit tests from the project.
    
### `-generate-worker-test-runner` (`-trw`)

- Produce an application for running unit tests in a [worker thread](native/concurrency.html#workers).
    
### `-generate-no-exit-test-runner` (`-trn`)

- Produce an application for running unit tests without an explicit process exit.
    
### `-include-binary <path>` (`-ib <path>`)

- Pack external binary within the generated klib file.
    
### `-library <path>` (`-l <path>`)

- Link with the library. To learn about using libraries in Kotlin/native projects, see 
[Kotlin/Native libraries](native/libraries.html).

### `-library-version <version>` (`-lv`)

- Set library version.
    
### `-list-targets`

- List the available hardware targets.

### `-manifest <path>`

- Provide a manifest addend file.

### `-module-name <name>`

- Specify a name for the compilation module.
This option can also be used to specify a name prefix for the declarations exported to Objective-C:
[How do I specify a custom Objective-C prefix/name for my Kotlin framework?](native/faq.html#q-how-do-i-specify-a-custom-objective-c-prefixname-for-my-kotlin-framework)

### `-native-library <path>`(`-nl <path>`)

- Include the native bitcode library.

### `-no-default-libs`

- Disable linking user code with the [default platform libraries](native/platform_libs.html) distributed with the compiler.
    
### `-nomain`

- Assume the `main` entry point to be provided by external libraries.

### `-nopack`

- Don't pack the library into a klib file.

### `-linker-option`

- Pass an argument to the linker. To learn more, see 
[C compiler and linker options](native/c_interop.html#c-compiler-and-linker-options).

### `-linker-options <args>`

- Pass arguments to the linker. To learn more, see 
[C compiler and linker options](native/c_interop.html#c-compiler-and-linker-options).
 Separate different arguments with whitespaces.

### `-nostdlib`

- Don't link with stdlib.

### `-opt`

- Enable compilation optimizations.

### `-output <name>` (`-o <name>`)

- Set the name for the output file.

### `-entry <name>` (`-e <name>`)

- Specify the qualified entry point name.

### `-produce <output>` (`-p`)

- Specify output file kind:
    - `program`
    - `static`
    - `dynamic`
    - `framework`
    - `library`
    - `bitcode`

### `-repo <path>` (`-r <path>`)

- Library search path. For more information, see [Library search sequence](native/libraries.html#library-search-sequence).

### `-target <target>`

- Set hardware target. To see the list of available targets, use the [`list-targets`](#-list-targets) option.

  
