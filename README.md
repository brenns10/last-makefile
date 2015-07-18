# The Last Makefile You'll Ever Need

Welcome to the home of `last-makefile`, my self-proclaimed "last Makefile you'll
ever need."  Really, it's just the last Makefile *I'll* ever need, but it sounds
a lot cooler the first way.

## What Is It?

`last-makefile` is a generic Makefile for C programming projects.  The idea is,
you just fill in some basic variables that describe your project, plop it into
the root directory, and start running `make`.

## Why?

Makefiles are really "arcane".  They have a very unusual and difficult to use
syntax that is built on top of your shell.  This means that, to someone who has
a *lot* of experience with a broad range of compilers, shells, Unix tools,
programming tools, etc., Makefiles are *ridiculously* powerful.  But to most of
us, they're these obnoxious files that we just tweak until they can barely
compile our code.

I don't claim to be an expert, but I have written a lot of Makefiles.  My first
large-ish C project, [libstephen][], was the first time I had the motivation to
come up with a high-quality, feature-rich Makefile that would continue to work
well as I added to my project.  That Makefile served as the basis of the [cky][]
Makefile, which diverged as I experimented with new features.  Each new C
project, small or large, brought opportunities for improvement.  Now, I've taken
the time to merge all the features of my Makefiles into a single "master"
Makefile!

## What Can It Do?

*   **Obviously, it compiles your project!**  Just run `make`.
*   **Create libraries or executables.** Works for static libraries, dynamic
    libraries, and executables.  (set the `PROJECT_TYPE` variable)
*   **It supports build configurations.** Typically, you want to have at least
    two types of builds - debug and release.  Debug builds can contain debugging
    symbols, assertions, and higher logging levels, while release builds should
    have higher optimization levels.  My makefile has both debug and release, as
    well as a third configuration explained a bit later on.  The build is
    release by default, but you can set it to debug by adding `CFG=debug` to the
    command.
*   **Testing is important, too!.** This makefile has two targets, one for your
    main compilation target, and one for your tests.  Your tests live in a
    separate directory and are always run with Valgrind to check for memory
    leaks.  `make` builds your project, and `make test` builds and runs your
    tests.
*   **Code coverage helps you write tests.** Once you have test code, the last
    build configuration, "coverage", will automatically run your tests and
    generate a code coverage report, which helps you figure out what you need to
    test next.  Run `make CFG=coverage cov` to generate an HTML coverage report
    in the `cov/` directory.
*   **Automatic documentation is a good thing™.** Writing thorough documentation
    for public functions is important, and the result should be a usable API
    reference.  If you have Doxygen, just run the wizard, and from then on you
    can run `make doc` to generate up-to-date documentation.
*   **Writing out code dependencies should be a computer's job.** One of the
    worst parts of creating makefiles is maintaining the list of dependencies
    for each `.o` file.  If you forget dependencies, you'll get annoying link
    errors and have to start over with a `make clean`.  But who remembers to
    update the makefile each time they add or remove a `#include`?  This
    makefile completely removes that responsibility.  Under normal development
    (adding and removing dependencies and files), you should never need to `make
    clean`, since this makefile will automatically update its lists of
    dependencies and recompile whatever files it needs.


## How To Use It?

Simply fill in the configuration variables in the first section.  You should use
a project structure like this:

*   `src` - contains source files, internal header files, as well as folders
    that contain "modules" within your code.
*   `test` - contains tests for your main code.  This could also be structured
    into modules if you'd like.
*   `inc` - contains any public header files.
*   `obj/CONFIGURATION/` - contains object files
*   `bin/CONFIGURATION/` - contains final targets
*   `doc` - contains automatically generated documentation
*   `cov` - contains automatically generated coverage reports
*   `dep` - contains automatically generated dependencies

[libstephen]: https://github.com/brenns10/libstephen
[cky]: https://github.com/brenns10/cky
