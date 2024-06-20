GCC Makefile
============

Adding files in CMake project
-----------------------------

Adding sources and headers
~~~~~~~~~~~~~~~~~~~~~~~~~~

In the section, we will introduce how to add files to AmebaPro2 project,
including adding source, header files, creating and linking the library
files.

(1) Adding Source files

   Open the application.cmake at
   “project/realtek_amebapro2_v0_example/GCC-RELEASE/application/”.

   Add the source code by append to app_sources:

.. code-block:: cmake

   list(
     APPEND app_sources
     ...
     ${PATH_TO_YOUR_SOURCE_FILES}
     ...
   )

(2) Adding header files

   Open the includepath.cmake at
   “project/realtek_amebapro2_v0_example/GCC-RELEASE/”.

   Add the header files by append to inc_path_re:

.. code-block:: cmake

   list(
     APPEND inc_path_re
     ...
     ${PATH_TO_YOUR_HEADER_FILES}
     ...
   )

Also add directory to be included by include_directories (<path to header folder>).

.. code-block:: cmake

   include_directories (${PATH_TO_YOUR_INCLUDE_DIR})


Adding library files
~~~~~~~~~~~~~~~~~~~~

Method 1
^^^^^^^^

You can place the library under
“project/realtek_amebapro2_v0_example/GCC-RELEASE/application/output”.

Assume your library file is libABC.a, you can modify application.cmake
like:

.. code-block:: cmake

   target_link_libraries(
     ${app}
     ...
     ABC
     ...
   }

Method 2
^^^^^^^^

(1) Declare your library

.. code-block:: cmake

   add_library (<LIBRARY_NAME> STATIC IMPORTED)


(2) Setup location of your library by:

.. code-block:: cmake

  set_property (TARGET <LIBRARY_NAME> PROPERTY IMPORTED_LOCATION <PATH_TO_YOUR_LIBRARY>)

or

.. code-block:: cmake

  set_target_properties (<LIBRARY_NAME> PROPERTY IMPORTED_LOCATION <PATH_TO_YOUR_LIBRARY>)

(3) Link to your library

.. code-block:: cmake

   target_link_libraries(
     ${app}
     ...
     <LIBRARY_NAME>
     ...
   }

Building a library
~~~~~~~~~~~~~~~~~~

Create a cmake file for the library
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

(1) Set up minimum required cmake version and the project name. Here the
    output library file name will be libtest.a or libtest.so.

.. code-block:: cmake

   cmake_minimum_required(VERSION 3.6)
   project(test)
   set(test test)

(2) Append source files to project source list

.. code-block:: bash

   list(APPEND test_sources
      $(PROJ_ROOT)/example/test01.c
      $(PROJ_ROOT)/example/test02.c
   )

(3) Assign the library type, STATIC means that the library will be built
    as static-link library (*.a), while SHARED means that the library
    will be built as dynamic-link library (*.so).

.. code-block:: cmake

   add_library(
     ${test} STATIC ${test_sources}
   )

(4) Add the compile flag for the library

.. code-block:: cmake

   list(APPEND test_flags
     CONFIG_BUILD_ALL=1
     CONFIG_BUILD_LIB=1
     ${YOUR_COMPILE_FLAGS}
   )

(5) Add the header files need to be included in the library

.. code-block:: cmake

   include(../includepath.cmake)
   target_include_directories(${test} PUBLIC
     ${YOUR_INCLUDE_DIRS}
   }

Add the cmake and link the library to the project
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can include and link to your library in application.cmake files by

.. code-block:: bash

   include(./libtest.cmake)
     ...
     target_link_libraries(
       ${app}
       ...
       test
       ...
     }
   ...


Turn off the dependency of the library
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If users do not want to rebuild their own library each time when modification is not related to their library, users can open the
DependInfo.cmake under GCC-RELEASE/build/application/CMakeFiles/<YOUR_LIBRARY.dir> and turn on the CMAKE_DEPENDS_IN_PROJECT_ONLY.

.. code-block:: cmake

   # Consider dependencies only in project.
   set(CMAKE_DEPENDS_IN_PRJECT_ONLY ON)


Creating a new application example
----------------------------------

The application example folder of AmebaPro2 needs to have app_example.c
and <EXAMPLE_FOLDER.cmake>. The app_example.c is the entry of the
example and the cmake file is for project build. Here are the steps for
building up a new application example.

(1) Create a folder under “sdk/component/example”, move the source code
    to the folder and add app_example.c and <EXAMPLE_FOLDER.cmake> in
    the folder.

(2) Open app_example.c and call to the entries of example under the
    function app_example

.. code-block:: c

   void app_example(void)
   {
     example_audio_helix_aac();
   }

(3) Append the source code, header file, compile flag and library needed
    under the lists in <EXAMPLE_FOLDER.cmake>.

(4) After done the previous steps, users can build up the new example
    project by:

.. code-block:: bash

   cmake .. -G"Unix Makefiles" -DCMAKE_TOOLCHAIN_FILE=../toolchain.cmake -DEXAMPLE=<EXAMPLE_FOLDER>
   cmake --build . --target flash

How to use example source code
------------------------------

In this section, we will describe how to use the example source code for
AmebaPro2

Application example source
~~~~~~~~~~~~~~~~~~~~~~~~~~

AmebaPro2 application’s example source codes which can be separate into function examples and integrated examples. For function examples, it is
typically shows how to use the normal function provided in AmebaPro2 like SD, file, audio, file system, etc. For integrated examples, they
provide some integrated application for AmebaPro2 functions, like video and audio streaming and simple doorbell-chime application.
Each example subfolder contains only one example, the entry function is app_example(void).

The example entry function is defined as app_example and only one
example is exist in the same project.

Function examples
^^^^^^^^^^^^^^^^^

The function examples can be found under folder “sdk/component/example”.

Here are steps to build up the example:

(1) Create example build folder in
    “project/realtek_amebapro2_v0_example/GCC-RELEASE” and enter it

.. code-block:: bash

   cd project/realtek_amebapro2_v0_example/GCC-RELEASE
   mkdir build_example && cd build_example

(2) Use cmake to create makefile for example. The <EXAMPLE_FOLDER_NAME>
    can refer to the folders under sdk/component/example.

.. code-block:: bash

   cmake .. -G"Unix Makefiles" -DCMAKE_TOOLCHAIN_FILE=../toolchain.cmake -DEXAMPLE=<EXAMPLE_FOLDER_NAME>

.. note :: If the example folder not exist, the “<EXAMPLE_FOLDER_NAME> Not Found” message will show, please check the example folder name

(3) If example configured successfully, run build command to generate
    flash image

.. code-block:: bash

   cmake --build . --target flash

.. note :: In AmebaPro2 project, when -DEXAMPLE=<EXAMPLE_FOLDER_NAME> is used, the integrated function -DVIDEO_EXAMPLE=on and -DDOORBELL_CHIME=on will be set to off after the needed source imported.

Integrated examples
^^^^^^^^^^^^^^^^^^^
The function examples can be found under folder “sdk/project/realtek_amebapro2_v0_example/src”.

-  Video examples

   These examples could be found under “sdk/project/realtek_amebapro2_v0_example/src/mmfv2_video_example” and opened by using compiling flag –DVIDEO_EXAMPLE=on. The detail of these examples can refer to Multimedia Framework Architecture.

-  Doorbell and chime example

   The example could be found under “sdk/project/realtek_amebapro2_v0_example/src/doorbell-chime”, which provide users to construct a simple doorbell system which could push video and audio and get audio streaming though the Skynet. This example can be opened through -DDOORBELL_CHIME=on.

.. note :: In AmebaPro2 project, the flag of integrated could not be used in the same time.

MMF example source
~~~~~~~~~~~~~~~~~~

In sdk/component/example/media_framework, it provides audio-only MMF examples. The examples are based on the Multimedia Framework Architecture and the detail can refer to **Multimedia Framework Architecture**.

Peripheral example source
~~~~~~~~~~~~~~~~~~~~~~~~~

The peripheral example sources are located at the folder sdk/project/realtek_amebapro2_v0_example/example_sources and basically provide main.c and readme file. The main.c file contains the usage of peripheral function and user should replace it with the original main.c (in SDK/project/realtek_amebapro2_v0_example/src). On the other hand, like application example source, the method to compile example and
adjust the important parameters is described in the readme file. After the setting, user can rebuild the project with peripheral example.

WiFi example source
~~~~~~~~~~~~~~~~~~~

For user to test and development, we provide AT command in AmebaPro2. Users can key in AT command to connect WLAN by the console in PC. AT command can refer to “AN0025 Realtek at command.pdf”.
