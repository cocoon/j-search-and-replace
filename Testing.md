# Introduction #

This pages goes over the testing and debugging of the solution.
Normally this isn't required, but there's a section of the code that runs under UAC and testing that requires a couple of extra steps.

# Regular testing #

Just open the solution and either run the app in Debug mode.
There are also Unit Tests you can run to make sure everything is ok. At the time of this writing, there are no unit tests for the part of the code that runs under UAC.

# Testing under UAC #

UAC (User Account Control) is a feature introduced in Windows Vista to prevent applications from tinkering with the OS where it can have devastating effects.

In this app, there's a piece of the code that updates the registry under `HKEY_CLASSES_ROOT`, where the user can register for a right click option to Search and Replace a file.
This part of the code is all in the `SetRegistry` class.

Debugging under UAC is a bit more tricky and here are the steps:

### Update `app.manifest` ###
Update `app.manifest` in the section `requestedExecutionLevel`
Needs to be updated to the values below (there's a commented line for you to toggle to make it easier).
```
<requestedExecutionLevel level="requireAdministrator" uiAccess="false" />
```
### Run Visual Studio as admin ###
In start, right click and choose "Run as administrator" to start Visual Studio.

### Add project properties ###
In the _Debug > Project Properties_ page, add the following command line arguments:

`-ConfigRegistryRightClickHandler "<FILE_EXTENSIONS>"`

Where `<FILE_EXTENSIONS>` are the file extensions that you want to target the right click to, ex, if you want to target `*.srt` and `*.smi` files (both are usually text files for movie subtitles), add the following entry:

`-ConfigRegistryRightClickHandler ".srt, .smi"`

# Back to testing normally #

Just follow the steps in the reverse direction.
If you think you'll be toggling back and forth with UAC testing, you can just add or remove the command line parameters in the project properties, but where you're done with that, make sure you tidy up and update the `app.manifest` file and restart Visual Studio in normal user mode as well.