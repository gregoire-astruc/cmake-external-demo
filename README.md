Demonstration of CMake ExternalProject capabilities
===================================================

Using the `SuperBuild` pattern, the root `CMakeLists.txt` treats everything as an external project,
even the local one.

However, the local one (called `demo`) is fetched from directly from the source directory, while
the other external dependencies (`libpng` and `zlib`) are downloaded, built and installed.

With this strategy, the `demo/CMakeLists.txt` file is kept rather clean using only standard CMake
`find_package`. But since the `CMAKE_PREFIX_PATH` is set to point to the downloaded and compiled
libraries, it will prefer those over the system ones (if any).
