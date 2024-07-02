Building Environment
====================

.. contents::
  :local:
  :depth: 2

Setting up GCC Building Environment
-----------------------------------

Building the project in GCC Building Environment (Windows)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Installing mingw with ASDK and setting up the CMake
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

(1) Download and extract msys64_v10_3.7z from tools folder

(2) Check the windows Environment Variable HOME by Command Prompt

.. code-block:: bash

   echo %HOME%

.. note :: If %HOME% is not exist, the command will simply print %HOME%

(3) If your windows already have Environment Variable named HOME, open the file "msys64/etc/post-install/05-home-dir.post"

   Add "HOME=<PATH_TO_YOUR_MSYS64>/home/<USER_FOLDER>".
   
.. code-block:: bash

   # If the home directory doesn't exist, create it.
   HOME=C:/msys64_v10_3/msys64/home/${USER}
   if [ ! -d "${HOME}" ]; then
     if mkdir -p "${HOME}"; then
        echo "Copying skeleton files."

.. note :: By default <USER_FOLDER> is ${USER}. To prevent some errors, do not include space characters in <USER_FOLDER>

(4) Double click "msys2_shell.cmd" from mysys64 folder

(5) After setting up mingw, you need to install cmake. Download cmake in
    https://github.com/Kitware/CMake/releases/download/v3.20.0-rc1/cmake-3.20.0-rc1-windows-x86_64.msi
    and install it

(6) Add location of cmake.exe to PATH of msys2_shell by using vim
    ~/.bashrc and appending path of cmake.exe to environment variable
    PATH or using editor to directly append the path to file
    "msys64/home/<USER_FOLDER>/.bashrc"

.. code-block:: bash

   export PATH=/c/Program\ Files/CMake/bin:$PATH


.. Caution :: If your PATH contains space characters, remember to use "\\" to escape

.. note :: For the first time adding the CMake PATH, after adding the PATH, you need to re-open the msys2_shell and check by:

.. code-block:: bash

   $ cmake --version
   cmake version 3.20.0-rc1
   
   CMake suite maintained and supported by Kitware (kitware.com/cmake).

|

Adding toolchain to msys2
^^^^^^^^^^^^^^^^^^^^^^^^^

(1) Like adding PATH for cmake, user can add or change the toolchain in
    "msys64/home/<USER_FOLDER>/.bashrc".

(2) Add toolchain PATH by "export PATH=<path to toolchain>:$PATH".

.. code-block:: bash

   if [ -d "../../asdk-10.3.0" ]; then
       echo "asdk-10.3.0 exist"
       export PATH=/asdk-10.3.0/mingw32/newlib/bin:$PATH

.. note :: The recommended toolchain version is 10.3.0

Building the project
^^^^^^^^^^^^^^^^^^^^

(1) Open mingw by double clicking "msys2_shell.cmd".

(2) Enter the project location:
    project/realtek_amebapro2_v0_example/GCC-RELEASE.

(3) Create folder "build" and enter "build" folder.

(4) Run "cmake .. -G"Unix Makefiles"
    -DCMAKE_TOOLCHAIN_FILE=../toolchain.cmake" to create the makefile.

(5) Run "cmake --build . --target flash" to build and generate flash
    binary.

.. note :: If building successfully, you can see flash_ntz.bin in the build folder


|

Building the project in GCC Building Environment (LINUX)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Add toolchain to the linux PATH
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

(1) Extract the toolchain file (the toolchain file may provide in tools
    folder):

.. code-block:: bash

   tar -jxvf <PATH_TO_YOUR_TOOLCHAIN.tar.bz2> -C <DIR_TO_EXTRACT>

(2) Add toolchain to PATH:

.. code-block:: bash

   export PATH=<PATH_TO_YOUR_TOOLCHAIN>/asdk-10.3.0/linux/newlib/bin:$PATH

.. note :: You can add PATH to ~/.bash_profile

Installing cmake for linux
^^^^^^^^^^^^^^^^^^^^^^^^^^

(1) Install cmake using terminal (like "sudo apt-get -y install cmake"),
    if the installation is successful, you can get the version by "cmake
    --version".

Building the project
^^^^^^^^^^^^^^^^^^^^

(1) Open linux terminal and enter the project location:
    project/realtek_amebapro2_v0_example/GCC-RELEASE/.

(2) Create folder "build" and enter "build" folder.

(3) Run "cmake .. -G"Unix Makefiles"
    -DCMAKE_TOOLCHAIN_FILE=../toolchain.cmake" to create the makefile.

(4) Run "cmake --build . --target flash" to build and generate flash
    binary.

.. note :: 
	- If building successfully, you can see flash_ntz.bin in the build folder
	- If the ‘build’ folder has been used by others, you can remove ‘build’ folder first to have clean build
	- If there’s some permission issues, you can do "chmod -R 777 <PATH_TO_YOUR_SDK>"


|

Log UART Settings
-----------------

(1) To use AmebaPro2 log UART, the user needs to connect jumpers to
    **J21** for **FT232 (CON8)**.

(2) After using CON8 to connect to PC, you can use console tools (like
    tera term, MoBaxterm) to get log from EVB by setting baud rate as
    **115200**.

	.. image:: ../_static/01_BUILD/pro2_EVB.png
