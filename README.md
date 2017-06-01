# .NET S2I Builder

[![Build Status](https://travis-ci.org/ausnimbus/s2i-dotnet.svg?branch=master)](https://travis-ci.org/ausnimbus/s2i-dotnet)
[![Docker Repository on Quay](https://quay.io/repository/ausnimbus/s2i-dotnet/status "Docker Repository on Quay")](https://quay.io/repository/ausnimbus/s2i-dotnet)

This repository contains the source for the [source-to-image](https://github.com/openshift/source-to-image)
builders used to deploy [.NET applications](https://www.ausnimbus.com.au/languages/dotnet/)
on [AusNimbus](https://www.ausnimbus.com.au/).


## Environment variables

* **DOTNET_STARTUP_PROJECT**

    Used to select the project to run. This must be the folder containing
    `project.json`. Defaults to `.`.

* **DOTNET_PUBLISH**

    Used to control whether the application should be built by executing
    `dotnet build` or `dotnet publish`. To publish the application set the
    value to `true`. It is recommended to publish your application. For
    backwards compatibility, the default is `false`. In the next major release,
    this variable will be removed and the builder will always publish the
    application.

* **DOTNET_ASSEMBLY_NAME**

    Used to select the assembly to run. This must NOT include the `.dll` extension.
    Set this to the output assembly name specified in `project.json` (`name`, `buildOptions/outputName`).
    For `project.json`, the assembly name defaults to the `project.json` parent folder. The name of the
    parent folder is used as the default value for `DOTNET_ASSEMBLY_NAME`.

    When `project.json` is at the `context-dir`, the parent folder name will be 'src'. So, by
    default, this generates a 'src.dll' assembly. Setting `DOTNET_ASSEMBLY_NAME` will cause:
    - the assembly to be <DOTNET_ASSEMBLY_NAME>.dll
    - the application sources to be in subfolder `DOTNET_ASSEMBLY_NAME` in the deployed
    container.

* **DOTNET_RESTORE_SOURCES**

    Used to specify the list of NuGet package sources used during the restore operation. This overrides
    all of the sources specified in the NuGet.config file.

* **DOTNET_NPM_TOOLS**

    Used to specify a list of npm packages to install before building the app.
    Defaults to ``.

* **DOTNET_TEST_PROJECTS**

    Used to specify the list of test projects to run. This must be folders containing
    `project.json`. `dotnet test` is invoked for each folder. Defaults to ``.

* **DOTNET_CONFIGURATION**

    Used to run the application in Debug or Release mode. This should be either
    `Release` or `Debug`.  This is passed to the `dotnet publish` invocation.
    Defaults to `Release`.

* **NPM_MIRROR**

    Use a custom NPM registry mirror to download packages during the build process.

## Versions

The versions currently supported are:

- 1.1.2
- 2.0.0-preview
