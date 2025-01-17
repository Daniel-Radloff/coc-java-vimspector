# coc-java-vimspector

Fork of [vscode-java-debug](https://github.com/microsoft/vscode-java-debug) to
works with [coc.vim](https://github.com/neoclide/coc.nvim)
and [coc-java](https://github.com/neoclide/coc-java)



https://user-images.githubusercontent.com/14270445/164477764-04025fec-2af7-4251-b511-98209958e001.mov


## Overview
A lightweight Java Debugger based on [Java Debug Server](https://github.com/Microsoft/java-debug) which extends the [Language Support for Java by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.java). It allows users to debug Java code using Visual Studio Code (VS Code). Here's a list of features:

- Launch/Attach
- Breakpoints/Conditional Breakpoints/Logpoints
- Exceptions
- Pause & Continue
- Step In/Out/Over
- Variables
- Callstacks
- Threads
- Debug console
- Evaluation
- Hot Code Replace

## Requirements
- JDK (version 1.8.0 or later)
- coc.nvim (version 0.0.80 or later)
- coc-java
- vimspector
- [Language Support for Java by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.java) (version 0.14.0 or later)

## Install

1. Install this extension by run command:

```
:CocInstall coc-java-vimspector
```

## Use

- Launch Vim or Nvim
- Open a Java project (Maven/Gradle/Eclipse)
- Open a Java file to activate the extensions
- :CocList mainClassListRun and select class to start

Please also check the documentation of [Language Support for Java by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.java) if you have trouble setting up your project.

## Options

### Launch

- `mainClass` (required) - The fully qualified class name (e.g. [java module name/]com.xyz.MainApp) or the java file path of the program entry.
- `args` - The command line arguments passed to the program. Use `"${command:SpecifyProgramArgs}"` to prompt for program arguments. It accepts a string or an array of string.
- `sourcePaths` - The extra source directories of the program. The debugger looks for source code from project settings by default. This option allows the debugger to look for source code in extra directories.
- `modulePaths` - The modulepaths for launching the JVM. If not specified, the debugger will automatically resolve from current project.
- `classPaths` - The classpaths for launching the JVM. If not specified, the debugger will automatically resolve from current project.
- `encoding` - The `file.encoding` setting for the JVM. If not specified, 'UTF-8' will be used. Possible values can be found in https://docs.oracle.com/javase/8/docs/technotes/guides/intl/encoding.doc.html.
- `vmArgs` - The extra options and system properties for the JVM (e.g. -Xms\<size\> -Xmx\<size\> -D\<name\>=\<value\>), it accepts a string or an array of string.
- `projectName` - The preferred project in which the debugger searches for classes. There could be duplicated class names in different projects. This setting also works when the debugger looks for the specified main class when launching a program. It is required when the workspace has multiple java projects, otherwise the expression evaluation and conditional breakpoint may not work.
- `cwd` - The working directory of the program. Defaults to `${workspaceFolder}`.
- `env` - The extra environment variables for the program.
- `envFile` - Absolute path to a file containing environment variable definitions.
- `stopOnEntry` - Automatically pause the program after launching.
- `console` - The specified console to launch the program. If not specified, use the console specified by the `java.debug.settings.console` user setting.
  - `internalConsole` - VS Code debug console (input stream not supported).
  - `integratedTerminal` - VS Code integrated terminal.
  - `externalTerminal` - External terminal that can be configured in user settings.
- `shortenCommandLine` - When the project has long classpath or big VM arguments, the command line to launch the program may exceed the maximum command line string limitation allowed by the OS. This configuration item provides multiple approaches to shorten the command line. Defaults to `auto`.
  - `none` - Launch the program with the standard command line 'java [options] classname [args]'.
  - `jarmanifest` - Generate the classpath parameters to a temporary classpath.jar file, and launch the program with the command line 'java -cp classpath.jar classname [args]'.
  - `argfile` - Generate the classpath parameters to a temporary argument file, and launch the program with the command line 'java @argfile [args]'. This value only applies to Java 9 and higher.
  - `auto` - Automatically detect the command line length and determine whether to shorten the command line via an appropriate approach.
- `stepFilters` - Skip specified classes or methods when stepping.
  - `classNameFilters` - [**Deprecated** - replaced by `skipClasses`] Skip the specified classes when stepping. Class names should be fully qualified. Wildcard is supported.
  - `skipClasses` - Skip the specified classes when stepping. You could use the built-in variables such as '$JDK' and '$Libraries' to skip a group of classes, or add a specific class name expression, e.g. java.*, *.Foo
  - `skipSynthetics` - Skip synthetic methods when stepping.
  - `skipStaticInitializers` - Skip static initializer methods when stepping.
  - `skipConstructors` - Skip constructor methods when stepping.

### Attach

- `hostName` (required, unless using `processId`) - The host name or IP address of remote debuggee.
- `port` (required, unless using `processId`) - The debug port of remote debuggee.
- `processId` - Use process picker to select a process to attach, or Process ID as integer.
  - `${command:PickJavaProcess}` - Use process picker to select a process to attach.
  - an integer pid - Attach to the specified local process.
- `timeout` - Timeout value before reconnecting, in milliseconds (default to 30000ms).
- `sourcePaths` - The extra source directories of the program. The debugger looks for source code from project settings by default. This option allows the debugger to look for source code in extra directories.
- `projectName` - The preferred project in which the debugger searches for classes. There could be duplicated class names in different projects. It is required when the workspace has multiple java projects, otherwise the expression evaluation and conditional breakpoint may not work.
- `stepFilters` - Skip specified classes or methods when stepping.
  - `classNameFilters` - [**Deprecated** - replaced by `skipClasses`] Skip the specified classes when stepping. Class names should be fully qualified. Wildcard is supported.
  - `skipClasses` - Skip the specified classes when stepping. You could use the built-in variables such as '$JDK' and '$Libraries' to skip a group of classes, or add a specific class name expression, e.g. java.*, *.Foo
  - `skipSynthetics` - Skip synthetic methods when stepping.
  - `skipStaticInitializers` - Skip static initializer methods when stepping.
  - `skipConstructors` - Skip constructor methods when stepping.

### User Settings

- `java.debug.logLevel`: minimum level of debugger logs that are sent to VS Code, defaults to `warn`.
- `java.debug.settings.showHex`: show numbers in hex format in "Variables" viewlet, defaults to `false`.
- `java.debug.settings.showStaticVariables`: show static variables in "Variables" viewlet, defaults to `false`.
- `java.debug.settings.showQualifiedNames`: show fully qualified class names in "Variables" viewlet, defaults to `false`.
- `java.debug.settings.showLogicalStructure`: show the logical structure for the Collection and Map classes in "Variables" viewlet, defaults to `true`.
- `java.debug.settings.showToString`: show 'toString()' value for all classes that override 'toString' method in "Variables" viewlet, defaults to `true`.
- `java.debug.settings.maxStringLength`: the maximum length of string displayed in "Variables" or "Debug Console" viewlet, the string longer than this length will be trimmed, defaults to `0` which means no trim is performed.
- `java.debug.settings.numericPrecision`: the precision when formatting doubles in "Variables" or "Debug Console" viewlet.
- `java.debug.settings.hotCodeReplace`: Reload the changed Java classes during debugging, defaults to `manual`. Make sure `java.autobuild.enabled` is not disabled for [VSCode Java](https://github.com/redhat-developer/vscode-java). See the [wiki page](https://github.com/Microsoft/vscode-java-debug/wiki/Hot-Code-Replace) for more information about usages and limitations.
  - manual - Click the toolbar to apply the changes.
  - auto - Automatically apply the changes after compilation.
  - never - Never apply the changes.
- `java.debug.settings.enableRunDebugCodeLens`: enable the code lens provider for the run and debug buttons over main entry points, defaults to `true`.
- `java.debug.settings.forceBuildBeforeLaunch`: force building the workspace before launching java program, defaults to `true`.
- `java.debug.settings.onBuildFailureProceed`: Force to proceed when build fails, defaults to false.
- `java.debug.settings.console`: The specified console to launch Java program, defaults to `integratedTerminal`. If you want to customize the console for a specific debug session, please modify the 'console' config in launch.json.
  - `internalConsole` - VS Code debug console (input stream not supported).
  - `integratedTerminal` - VS Code integrated terminal.
  - `externalTerminal` - External terminal that can be configured in user settings.
- `java.debug.settings.exceptionBreakpoint.skipClasses`: Skip the specified classes when breaking on exception. You could use the built-in variables such as '$JDK' and '$Libraries' to skip a group of classes, or add a specific class name expression, e.g. java.*, *.Foo
- `java.debug.settings.stepping.skipClasses`: Skip the specified classes when stepping. You could use the built-in variables such as '$JDK' and '$Libraries' to skip a group of classes, or add a specific class name expression, e.g. java.*, *.Foo
- `java.debug.settings.stepping.skipSynthetics`: Skip synthetic methods when stepping.
- `java.debug.settings.stepping.skipStaticInitializers`: Skip static initializer methods when stepping.
- `java.debug.settings.stepping.skipConstructors`: Skip constructor methods when stepping.
- `java.debug.settings.jdwp.limitOfVariablesPerJdwpRequest`: The maximum number of variables or fields that can be requested in one JDWP request. The higher the value, the less frequently debuggee will be requested when expanding the variable view. Also a large number can cause JDWP request timeout. Defaults to 100.
- `java.debug.settings.jdwp.requestTimeout`: The timeout (ms) of JDWP request when the debugger communicates with the target JVM. Defaults to 3000.
- `java.debug.settings.vmArgs`: The default VM arguments to launch the Java program. Eg. Use '-Xmx1G -ea' to increase the heap size to 1GB and enable assertions. If you want to customize the VM arguments for a specific debug session, please modify the 'vmArgs' config in launch.json.
- `java.silentNotification`: Controls whether notifications can be used to report progress. If true, use status bar to report progress instead. Defaults to `false`.


# Troubleshooting

- See ~/.vimspector.log.
- Run `:messages` to get echoed messages in vim.

## License

EPL 1.0, See [LICENSE](LICENSE) for more information.
