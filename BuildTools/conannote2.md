1. conan info ..    check the dependences of packages

2. conan info .. --graph=file.html  check the dependences of packages

3. The line set(CONAN_DEFINES -DSOME_LIBDEFINE) sets a CMake variable named CONAN_DEFINES to the value -DSOME_LIBDEFINE.

    In Conan, CONAN_DEFINES is a predefined variable that contains a list of preprocessor definitions that should be passed to the compiler when building a Conan package. By setting CONAN_DEFINES in your CMake project, you are telling CMake to pass these preprocessor definitions to the compiler when building your project's targets that depend on Conan packages.

    In this case, the -DSOME_LIBDEFINE preprocessor definition will be defined when building targets that depend on Conan packages. This can be useful, for example, if a library you are using requires a certain preprocessor definition to be set in order to work correctly. By setting CONAN_DEFINES in your CMake project, you can ensure that the required preprocessor definition is set when building your project.
    
    4.conanfile.txt and conanfile.py are both files used by Conan to define and manage dependencies for a project, but they serve different purposes.

    conanfile.txt is a declarative file format that allows you to list the dependencies your project needs and their versions. It is meant for simple projects that do not require complex build logic or customization. In conanfile.txt, you can specify the packages that your project depends on, as well as any options or settings that need to be passed to the package's build process.

    conanfile.py, on the other hand, is a Python script that allows you to define more complex build logic and customization for your project's dependencies. It is meant for larger or more complex projects that require custom build settings, complex dependency graphs, or other advanced functionality. In conanfile.py, you can define functions that implement custom build steps or configure dependencies based on specific build options or conditions.
