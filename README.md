# AusNimbus Builder for .NET [![Build Status](https://travis-ci.org/ausnimbus/s2i-dotnet.svg?branch=master)](https://travis-ci.org/ausnimbus/s2i-dotnet) [![Docker Repository on Quay](https://quay.io/repository/ausnimbus/s2i-dotnet/status "Docker Repository on Quay")](https://quay.io/repository/ausnimbus/s2i-dotnet)

[![.NET](https://user-images.githubusercontent.com/2239920/27292957-5a162312-5558-11e7-999e-a6abb05e2c90.jpg)](https://www.ausnimbus.com.au/)

The [AusNimbus](https://www.ausnimbus.com.au/) builder for .NET provides a fast, secure and reliable [ASP.NET](https://www.ausnimbus.com.au/apps/aspnet-hosting/) and [.NET hosting](https://www.ausnimbus.com.au/languages/dotnet-hosting/) environment.

This document describes the behaviour and environment configuration when running your .NET Core apps on AusNimbus.

- [Runtime Environments](#runtime-environments)
- [Web Process](#web-process)
- [Dependency Management](#dependency-management)
- [Environment Configuration](#environment-configuration)
- [Advanced](#advanced)
  - [Build Customization](#build-customization)
    - [Configuring npm](#configuring-npm)
  - [Running Tests](#running-tests)
- [Extending](#extending)
  - [Build Stage (assemble)](#build-stage-assemble)
  - [Runtime Stage (run)](#runtime-stage-run)
  - [Persistent Environment Variables](#persistent-environment-variables)
- [Debug Mode](#debug-mode)

## Runtime Environments

AusNimbus supports the latest stable release.

The currently supported versions are `1.1`

Only projects that use `.csproj` or pre-compiled `.dll` packages are supported.

## Web Process

Your application's web processes must bind to port `8080`.

AusNimbus handles SSL termination at the load balancer.

By default the builder will look for the first `.csproj` file available. However you may also explicitly specify your `csproj` file or a pre-compiled `.dll` file.

NAME       | Description
-----------|-------------
DOTNET_APP | Set to the path of your `.csproj` or published `.dll`

If you set `DOTNET_APP` to a `.dll` file, the build stage is skipped.

## Dependency Management

The `dotnet` utility is used to `restore`, `test` (optional) and `publish` your application.

Bower and NPM are bundled and used if the appropriate `bower.json` or `package.json` file are found.

## Environment Configuration

The following environment variables are available for you to configure your .NET environment:

NAME        | Description
------------|-------------
DOTNET_ENV  | Used to run the application in `Debug` or `Release` mode.

## Advanced

### Build Customization

You may overwrite the sources specified in your `NuGet.config` file by specifying the following environment variable:

NAME                   | Description
-----------------------|-------------
DOTNET_RESTORE_SOURCES | Specify the list of NuGet package sources used during the restore operation.

#### Configuring npm

If you would like to use a custom npm mirror you may use the following environment variable:

NAME        | Description
------------|-------------
NPM_MIRROR  | Define a custom npm registry mirror for downloading dependencies

You can also customize npm further by including a [.npmrc](https://docs.npmjs.com/files/npmrc) file in your project’s root.

### Running Tests

You may specify a list of test projects to run as part of the build stage using the following environment variable:

NAME                  | Description   
----------------------|-------------------
DOTNET_TEST_PROJECTS  | `dotnet test` is invoked for each folder

## Extending

AusNimbus builders are split into two stages:

- Build
- Runtime

Both stages are completely extensible, allowing you to customize or completely overwrite each stage.

### Build Stage (assemble)

If you want to customize the build stage, you need to add the executable `.s2i/bin/assemble` file in your repository.

This file should contain the logic required to build and install any dependencies your application requires.

If you only want to extend the build stage, you may use this example:

```sh
#!/bin/bash

echo "Logic to include before"

# Run the default builder logic
. /usr/libexec/s2i/assemble

echo "Logic to include after"
```

### Runtime Stage (run)

If you only want to change the executed command for the run stage you may the following environment variable.

NAME        | Description
------------|-------------
APP_RUN     | Define a custom command to start your application.

**NOTE:** `APP_RUN` will overwrite any builder's runtime configuration (including the [Debug Mode](#debug-mode) section)

Alternatively you may customize or overwrite the entire runtime stage by including the executable file `.s2i/bin/run`

This file should contain the logic required to execute your application.

If you only want to extend the run stage, you may use this example:

```sh
#!/bin/bash

echo "Logic to include before"

# Run the default builder logic
. /usr/libexec/s2i/run
```

As the run script executes every time your application is deployed, scaled or restarted it's recommended to keep avoid including complex logic which may delay the start-up process of your application.

### Persistent Environment Variables

The recommend approach is to set your environment variables in the AusNimbus dashboard.

However it is possible to store environment variables in code using the `.s2i/environment` file.

The file expects a key=value format eg.

```
KEY=VALUE
FOO=BAR
```

## Debug Mode

The AusNimbus builder provides a convenient environment variable to help you debug your application.

NAME        | Description
------------|-------------
DEBUG       | Set to "TRUE" to enable Debug Mode
