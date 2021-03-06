# Sharpmake


## Introduction
Sharpmake is a generator for Visual Studio projects and solutions. It is
similar to CMake and Premake, but it is designed for **speed** and
**scale**. Sharpmake has been used at Ubisoft to generate several thousands
of *vcxproj*, *csproj* and *sln* files in a matter of seconds, and each of
these projects can support a large number of Visual Studio configurations as
well.

That makes Sharpmake ideal for the development of multi-platform games, where
the number of platforms, the different levels of optimization, the multiple
rendering APIs on PC and the level editor can quickly multiply the number of
configurations a given code base must support. Sharpmake generates all those
configurations at once, very quickly. Thus, it becomes trivial to generate
and regenerate the entire project.

Sharpmake uses C# for scripting, hence the name. That means that you can edit
your scripts in Visual Studio (or Visual Studio Code) and have a complete
IntelliSense programming experience.

Sharpmake can also generate makefiles and Xcode projects, but it is currently
only available for Windows. With .NET Core and .NET Standard though, it is
our hope that it will eventually cross the platform barrier. In the
meanwhile, you may have luck using it with Mono.

Sharpmake was developed internally at Ubisoft for Assassin's Creed 3 in 2011.
After experimenting with the other existing tools, it became clear that none
of these solutions were performant enough to generate the number of
configurations needed (at least not in a trivial way) and that a custom
generator was needed.


## Documentation
The best place for the Sharpmake documentation is the
[wiki on GitHub](https://github.com/ubisoftinc/Sharpmake/wiki). The Sharpmake
source code also comes with samples that you can study.


## Building Sharpmake
Building Sharpmake is quite straightforward. Clone the repo on GitHub, open the
solution in Visual Studio and build the solution in *Release*. The binaries
will be found in the *Sharpmake.Application/bin/Release*. You can run the
*deploy_binaries.py* script to automatically fetch the binaries and copy them
in a *Binaries* folder.


## More Platforms
Sharpmake originally had support for game consoles, but Ubisoft pulled it out
because those could not be open sourced. Sharpmake now has an extension system
that allows support for these consoles to be added back at runtime.

If you need support for these platforms and are an authorized developer, you
can contact the SDK provider to get platform extension for Sharpmake.


## Contributing

### Tests
We will only accept merge requests that pass every tests. The unit tests are
written with NUnit and the regression tests are ran by comparing the samples'
output with a reference output. You can run the *regression_tests.py* script
after having built the solution in Visual Studio to run the regression tests.

Because the regression tests just do a direct comparison with the output, it is
possible to get a false negative after having done a good change. In that case,
please update the tests so they match the output after your change. You can run
the *UpdateSamplesOutput.bat* and *UpdateSharpmakeProjects.bat* batch files to
automatically overwrite the reference output files.

Naturally, we also recommend that you put your own tests after fixing a bug or
adding a feature to help us avoid regressions.

### Additional Platforms
If you want to add support for an additional platform, please make sure that
the platform is open and that you are not breaking your NDA. Ubisoft has not
published platform support for most video game consoles for that exact reason.
We will not accept merge requests for new platforms that are not completely
open for development.


## Some important sharpmake attributes

### Project - located at Sharpmake/Project.cs
- project.Name
- project.SharpmakeCsFileName - File name of the c# project configuration, ex: "MyProject.sharpmake"
- project.SharpmakeCsPath - Path of the CsFileName, ex: "c:\dev\MyProject"
- project.SourceRootPath - Root path of source file for this project, ex: "c:\dev\MyProject\src"
- project.RootPath - RootPath used as key to generate ProjectGuid and as a path helper for finding source files
- project.AdditionalSourceRootPaths - More source directories to parse for files in addition to SourceRootPath
- project.SourceFiles - Files in the project, may be full path of partial path from SourceRootPath
- project.SourceFilesCompileExtensions - File that match this regex compile
- project.SourceFilesFilters - if !=  null, include only file in this filter
- project.SourceFilesExclude - Excluded files from the project, removed from SourceFiles
- project.SourceFilesIncludeRegex - files that match SourceFilesIncludeRegex and SourceFilesExtension from source directory will make SourceFiles
- project.SourceFilesFiltersRegex - Filters SourceFiles list
- project.SourceFilesExcludeRegex - Sources file that match this regex will be excluded from build
- project.SourceFilesBuildExclude - Sources file to exclude from build from SourceFiles


### Solution - located at Sharpmake/Solution.cs
- solution.Name
- solution.ClassName - Solution Class Name, ex: "MySolution"
- solution.SharpmakeCsFileName - File name of the c# project configuration, ex: "MyProject.cs"
- solution.SharpmakeCsPath - Path of the CsFileName, ex: "c:\dev\MyProject"

### Configuration - located at Sharpmake/Project.Configuration.cs
- string TargetFileFullName = "[conf.TargetFilePrefix][conf.TargetFileName][conf.TargetFileSuffix]" - the name of the output of the project
