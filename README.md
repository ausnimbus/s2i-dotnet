# AusNimbus Builder for .NET

[![Build Status](https://travis-ci.org/ausnimbus/s2i-dotnet.svg?branch=master)](https://travis-ci.org/ausnimbus/s2i-dotnet)
[![Docker Repository on Quay](https://quay.io/repository/ausnimbus/s2i-dotnet/status "Docker Repository on Quay")](https://quay.io/repository/ausnimbus/s2i-dotnet)

[AusNimbus](https://www.ausnimbus.com.au/) builder for .NET provides a fast, secure and reliable [.NET hosting](https://www.ausnimbus.com.au/languages/dotnet-hosting/) environment.

Bower and NPM are included and used if the appropriate `bower.json` or `package.json` file are found.

## Environment variables

* **DOTNET_PROJECT**

    This must be the path to the `.csproj` or `.dll`

    By default it will search for the first `.csproj` file
    If you set `DOTNET_PROJECT` to a published `.dll` file, the build process will be skipped.

* **DOTNET_RESTORE_SOURCES**

    Used to specify the list of NuGet package sources used during the restore operation. This overrides
    all of the sources specified in the NuGet.config file.

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
