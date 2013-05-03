The Acqu Ecosystem
==================

This repository shall be the common code basis of acqu users within
the A2 collaboration. It will (at least) cover the calibration and a
simple physics analysis (TO BE DONE).

However, this master branch is still under heavy development and many
changes could break your workflow or personal code. But this will
improve!

So be warned, but don't hesitate to fork this repo here and contribute
your personal changes if there useful by providing pull requests.


Prerequisites
-------------

* CERN ROOT > 5.30 with MySQL support (enabled by default)
* CMake > 2.8 (most Linux distributions provide packages) 
* Zlib and Ncurses development packages (see also troubleshooting)
* git 
* Doxygen for documentation support (nicely integrated into QtCreator)

You probably want to create a GitHub account and fork from this
repository. Then clone this personal "acqu" distribution (it is still
very easy to get updates later).


Installation
------------

With CMake, it is required to build "outside" of the source
directory (e.g. in an empty subfolder). Thus, we create an extra
`build` sub-directory including all Makefiles (execute in "acqu"
basedir containing acqu\_core, acqu\_user, ...):

`mkdir build && cd build && cmake ..`

Note that the two dots in the end really mean the parent directory
(where the central CMakeLists.txt is located). Furthermore, it
defaults to Debug mode and tells you exactly where the to-be-built
libraries and binaries are located. You might want to add the binary
path to your PATH environment (but if you use QtCreator, not even that
is required).

Then start the parallel compilation for example on a QuadCore machine as 

`make -j5`

You can also create some documentation with 

`make doc` 

If you already opened QtCreator before, it should tell you that it
registered automatically the generated help file.

If you want to build only the DAQ part (no analysis), change the above
cmake call to 

`mkdir build && cd build && cmake -DACQU_DAQ=On ..`

which also automatically detects and includes EPICS, if available.


Usage with QtCreator
--------------------

With the new CMake system, it is very easy to use QtCreator as an
Integrated Development Environment. Just ensure that your version is
recent enough (something like 2.5). Then do the following to set it up
in QtCreator: 

1. Import the `acqu` project as a CMake project by opening the
CMakeLists.txt in the basedir. A wizard dialog should pop up.

2. You should get asked for a build directory. Create a new subdir
`build` and choose that. You can also choose an already existing one.

3. Run cmake from the GUI as required from the Wizard and check if it
found the correct ROOT installation. If it did not find the correct
one, either remove your ROOTSYS environment variable or add one by
starting QtCreator from the console like this: `ROOTSYS=/path/to/root
qtcreator &`.

4. Then set the working directory in "Project (left tab) --> Run
 Settings" to your acqu_user directory and provide the correct *.dat
 file to AcquRoot as an argument. At this point, you can also set
 `-j5` as an argument to make in the build settings tab.

5. Hit F5 and you should be able to build, run and debug AcquRoot.
That means you can set breakpoints, have sophisticated code completion
and a Doxygen-driven help system. You can still use in parallel the
command line, if you find that more useful in some cases.


Migration and Troubleshooting
-----------------------------

* If the detection of some libraries still fails despite installing
  the necessary *-dev packages from your distribution, delete
  CMakeCache.txt in your build directory and run cmake again. In
  general, cmake caches already found packages and also settings, so
  don't be confused by this "feature".
* Note that with CMake no environment variables are really needed
  anymore. This is why all setup files have been deleted. However, you
  might want to add the binary directory to your PATH environment.
* Since the .gitignore's have changed, you can get rid of old build
  files by `git clean -fdn`, which lists all files, and if that is
  okay, delete them with `git clean -fd`.  
* If the compilation fails, check if you have some previous ROOT
  Dictionary source files. You can find them by `find | grep Dict` (in
  basedir) and if that lists only unneeded files delete them by `find
  | grep Dict | xargs rm`. Also git should report some of them as
  untracked (see above item how to get rid of them).
* If you just want to run AcquRoot, then the Debug mode (=no
  optimizations) can be unnecessarily slow. Enable the Release mode by
  executing `cmake -DCMAKE_BUILD_TYPE=Release ..` inside the build
  directory (can also be a new build directory).
* If you're changing header files and compiling breaks, a `make clean`
  could help since not all dependencies are correctly identified at
  the moment (but most should be!).
* If the help in QtCreator does not work (i.e. F1 does not show you
  something when the pointer in the editor is located at some acqu
  class), then try registering manually the help file
  "build/doc/qthelp.qch" via "Tools --> Options... --> Help". Restart
  QtCreator.

