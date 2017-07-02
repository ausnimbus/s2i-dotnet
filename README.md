# AusNimbus Builder for .NET [![Build Status](https://travis-ci.org/ausnimbus/s2i-dotnet.svg?branch=master)](https://travis-ci.org/ausnimbus/s2i-dotnet) [![Docker Repository on Quay](https://quay.io/repository/ausnimbus/s2i-dotnet/status "Docker Repository on Quay")](https://quay.io/repository/ausnimbus/s2i-dotnet)

[![.NET](https://user-images.githubusercontent.com/2239920/27292957-5a162312-5558-11e7-999e-a6abb05e2c90.jpg)](https://www.ausnimbus.com.au/)

The [AusNimbus](https://www.ausnimbus.com.au/) builder for .NET provides a fast, secure and reliable [ASP.NET](https://www.ausnimbus.com.au/apps/aspnet-hosting/) and [.NET hosting](https://www.ausnimbus.com.au/languages/dotnet-hosting/) environment.

It supports projects that use `.csproj` or pre-compiled `.dll` packages.

Bower and NPM are included and used if the appropriate `bower.json` or `package.json` file are found.

Web processes must bind to port `8080`, and only the HTTP protocol is permitted for incoming connections.

## Environment variables

* **DOTNET_APP**

    * This must be the path to the `.csproj` or published `.dll`
    * By default it will search for the first `.csproj` file (alphabetically)

    If you set `DOTNET_APP` to a `.dll` file, the build process is be skipped.

* **DOTNET_RESTORE_SOURCES**

    * Used to specify the list of NuGet package sources used during the restore operation.
    * This overrides all of the sources specified in your NuGet.config file.

* **DOTNET_TEST_PROJECTS**

    * Used to specify the list of test projects to run. `dotnet test` is invoked for each folder.
    * Default: ``

* **DOTNET_ENV**

    * Used to run the application in Debug or Release mode. This should be either `Release` or `Debug`.
    * This is passed to the `dotnet publish` invocation.
    * Default : `Release`.

* **NPM_MIRROR**

    * Use a custom NPM registry mirror to download packages during the build process.

## Versions

The versions currently supported are:

- 1.1.2
