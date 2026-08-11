AI-Assistant Development
========================

.. contents::
   :local:
   :depth: 2

Overview
--------

`MCP (Model Context Protocol) <https://modelcontextprotocol.io/>`_ is an open standard protocol introduced by
Anthropic that lets AI Agents securely connect to external tools, data sources, and workflows.
Through MCP, AI Agents move beyond suggestions — they can directly perform actions.

Pro2 SDK Development MCP is a revise version of `RealMCU SDK Development MCP <https://aiot.realmcu.com/en/latest/tools/ai_assistant/mcp.html#sdk-development-mcp/>`_,
design direclty for AmebaPro2


Pro2 SDK Development MCP
------------------------

The SDK Development MCP launches its MCP server as a local subprocess and communicates with the AI
client over standard input/output, requiring no network connection or additional authentication. The AI
can directly modify project configuration, build, flash, and read/write the serial port — driving an
automated, closed-loop development workflow.

.. important::
   **Prerequisites — Building Environment must be set up first.**

   Before registering or using the SDK Development MCP, complete the following steps in order
   based on your OS environment (refer to :doc:`01_BUILD` for full details):

   1. Install the ARM toolchain (``asdk-10.3.0``) and cmake, and add them to PATH.

      - **Windows**: install msys2 first, then set up the toolchain and cmake inside msys2.
      - **Linux / macOS**: install the toolchain and cmake directly via your system package
        manager or by extracting the provided toolchain archive.

   2. Run the environment initialization script from the SDK root to create the Python
      virtual environment (``.venv``) and install MCP server dependencies:

      - **Windows**: ``env.bat``
      - **Linux / macOS / WSL2**: ``source env.sh``

Environment Setup and Registration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Step 1: Initialize the environment and install dependencies**

Run the environment initialization script from the SDK root. This creates the Python virtual
environment (``.venv``) and installs the MCP server dependencies. Choose the script matching
your OS:

**Windows**

.. code-block:: bat

   cd <SDK_ROOT>
   env.bat

**Linux / macOS / WSL2**

.. code-block:: bash

   cd <SDK_ROOT>
   source env.sh

.. note::
   If ``env.sh`` is not executable, grant execute permission first:

   .. code-block:: bash

      chmod +x env.sh

.. note::
   ``env.bat`` / ``env.sh`` only needs to be run once per SDK checkout. It creates the
   ``.venv`` Python environment required by the MCP server. If ``.venv`` is missing, the
   AI client will report a connection failure when trying to start the server.

**Step 2: Register the MCP**

**Claude Code**

Run the registration command from the SDK root (replace ``<SDK_ROOT>`` with the actual absolute path):

.. code-block:: bat

   :: Windows
   claude mcp add ameba-dev-pro2 -- <SDK_ROOT>\tools\ameba\ameba_dev_mcp\launcher.bat

.. code-block:: bash

   # Linux / macOS / WSL2
   claude mcp add ameba-dev-pro2 -- <SDK_ROOT>/tools/ameba/ameba_dev_mcp/launcher.sh

.. note::
   On Linux / macOS, if ``launcher.sh`` is not executable, grant permission first:

   .. code-block:: bash

      chmod +x <SDK_ROOT>/tools/ameba/ameba_dev_mcp/launcher.sh

Verify the registration:

.. code-block:: bash

   claude mcp list
   # Expected output: ameba-dev-pro2: ✓ Connected

To uninstall:

.. code-block:: bash

   claude mcp remove ameba-dev-pro2

**Codex**

Add via the Codex IDE interface:

1. Enter **Settings** -> **Extensions** -> **MCP servers**, click **Add server**.
2. Choose ``STDIO`` mode and enter the following:

   .. code-block:: text

      Name:              ameba-dev-pro2
      Command to launch: <SDK_ROOT>/tools/ameba/ameba_dev_mcp/launcher.sh

   On Windows, change the command to ``<SDK_ROOT>\\tools\\ameba\\ameba_dev_mcp\\launcher.bat``
   (backslashes must be escaped as ``\\`` in JSON).

Verify: After saving the configuration and returning, the **MCP Servers** section should display
an entry for ``ameba-dev-pro2`` with a connected indicator.

To uninstall: Select **Uninstall** in the corresponding MCP Servers settings.

Board Configuration
~~~~~~~~~~~~~~~~~~~~

Before first use, run the following slash command in the Claude Code chat:

.. code-block:: text

   /ameba-setup-boards

The AI will guide you through entering the board model, serial port, and other information, and
automatically perform a read-only environment check — no manual JSON editing required.

After setup, two configuration files are generated in the SDK root:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - File
     - Description
   * - ``board_info.json5``
     - Board configuration: alias to SoC model + serial port + connection type.
   * - ``project_info.json5``
     - Flash layout: firmware files and address table per SoC, auto-populated after the first
       ``build_firmware``.

.. note::
   To add or remove boards or change ports later, re-run ``/ameba-setup-boards`` or edit
   ``board_info.json5`` directly.

.. note::
   AmebaPro2 currently has **no auto-download circuit**. The board must be put into download mode manually
   before each flash operation:

   - **AMB82 (standard)**: set the **J27 jumper**, then press **RESET**.
   - **AMB82-MINI**: press and hold **UART_DOWNLOAD**, then press **RESET** simultaneously, then
     release both buttons.

Typical Agent Workflow
~~~~~~~~~~~~~~~~~~~~~~~

Tell the AI your development goal in Claude Code, and it will sequentially call
``build_firmware`` -> ``flash_firmware_tool`` -> ``serial_connect_tool`` -> ``serial_expect_tool`` ->
``serial_disconnect_tool`` to complete one full verification cycle:

.. code-block:: text

   # Tell the AI in Claude Code:
   Update the log print in main() to include the project version number.
   Build and flash to RTL8735B_COM5, then show me the boot log once it comes up.

Example AI tool call sequence:

.. code-block:: python

   build_firmware(alias="RTL8735B_COM5", video_example=True, nn=False)
   flash_firmware_tool(alias="RTL8735B_COM5")
   serial_connect_tool(alias="RTL8735B_COM5", reset=False)
   serial_expect_tool(alias="RTL8735B_COM5",
                      patterns=["START SCHEDULER", "Crash Dump"],
                      timeout=20.0)
   serial_disconnect_tool(alias="RTL8735B_COM5")

The same workflow applies to video examples. Use ``/ameba-video-example`` to interactively
select and build a video or NN example, or drive the tools directly:

.. code-block:: text

   # Tell the AI in Claude Code:
   Switch to the vipnn_facedet example, build and flash to RTL8735B_COM5,
   then show me the serial output.

Example AI tool call sequence:

.. code-block:: python

   set_video_example_tool(example_id="vipnn_facedet")
   build_firmware(alias="RTL8735B_COM5", video_example=True, nn=True)
   flash_firmware_tool(alias="RTL8735B_COM5")
   serial_connect_tool(alias="RTL8735B_COM5", reset=False)
   serial_expect_tool(alias="RTL8735B_COM5",
                      patterns=["SCRFD FPS", "Crash Dump"],
                      timeout=30.0)
   serial_disconnect_tool(alias="RTL8735B_COM5")

MCP Features — SDK Development
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The SDK Development MCP exposes three categories of functionality to the AI Agent.

**Tools**

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Tool
     - Description
   * - ``set_target``
     - Set the build target SoC; must be called before ``build_firmware`` when switching targets.
   * - ``build_firmware``
     - Build firmware using the CMake build system; supports normal example, video, and NN video
       builds. Automatically syncs ``project_info.json5`` on success.
   * - ``flash_firmware_tool``
     - Flash firmware to the AmebaPro2 board via ``uartfwburn``. Requires the board to be
       manually put into download mode first (see Board Configuration section).
   * - ``list_serial_ports_tool``
     - List available serial ports on the local machine; when an alias is given, confirms
       that the board's configured port is currently visible.
   * - ``apply_board_config_tool``
     - Atomically write or merge board entries into ``board_info.json5``.
   * - ``env_pre_check_tool``
     - Read-only environment readiness check: validates config files, serial port visibility,
       ``uartfwburn`` presence, and cmake/toolchain setup.
   * - ``list_video_examples_tool``
     - List all available video examples grouped by category, with the currently active
       example marked.
   * - ``set_video_example_tool``
     - Activate a specific video example by updating
       ``video_example_media_framework.c`` and ``amebapro2_fwfs_nn_models.json``.
   * - ``serial_connect_tool``
     - Open the serial connection; pulses DTR/RTS and clears the buffer by default.
       Idempotent — reuses an existing session if already open.
   * - ``serial_disconnect_tool``
     - Close the serial connection and release the port.
   * - ``serial_command_tool``
     - Send a command string and wait for a response; combines buffer drain + write +
       pattern match in one call.
   * - ``serial_expect_tool``
     - Block until one of the specified regex patterns matches on the serial buffer,
       or an idle/hard timeout fires.
   * - ``serial_read_tool``
     - Non-blocking peek; returns whatever is currently in the receive buffer
       (up to ``size`` bytes).

**Prompts**

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Prompt
     - Description
   * - ``/ameba-setup-boards``
     - Guided board configuration setup; collects board model and serial port, writes
       ``board_info.json5``, and automatically calls ``env_pre_check_tool`` to verify.
   * - ``/ameba-video-example``
     - Interactively select a video example, then build and flash it to the board.
       Guides through example selection, build flags (pristine / nn), flash, and optional
       serial monitor in one session.

**Resources**

.. list-table::
   :header-rows: 1
   :widths: 42 58

   * - Resource URI
     - Description
   * - ``board://list``
     - List all configured board aliases with SoC and transport; read this first to discover
       which alias to pass to flash/serial tools.
   * - ``board://{alias}``
     - Resolved configuration for a single board (defaults applied, password masked).
   * - ``device://profiles``
     - List AmebaPro2 device hardware information (RTL8735B flash tool details and
       PG tool availability).
   * - ``device://{device_name}/info``
     - Hardware info for a specific device (e.g. ``RTL8735B``): flash tool, baudrates,
       memory types, and download mode instructions.
   * - ``device://{device_name}/{memory_type}/info``
     - Hardware info filtered by memory type (``nor`` or ``nand``).
   * - ``config://project_info``
     - Full ``project_info.json5`` contents (flash layout, validated with defaults merged).
   * - ``config://board_info``
     - Full ``board_info.json5`` contents (passwords masked as ``***``).
   * - ``debug://hardware``
     - Hardware-side troubleshooting reference: serial driver versions, download mode
       circuit, RTS/DTR timing, failure patterns, and error-code index.

Troubleshooting
~~~~~~~~~~~~~~~

Press **Ctrl+O** in Claude Code to expand the detailed parameters and return values of MCP
tool calls and inspect specific error codes.

.. list-table::
   :header-rows: 1
   :widths: 35 65

   * - Symptom / Error code
     - Resolution
   * - ``BOARD_CONFIG_MISSING`` / ``PROJECT_CONFIG_MISSING``
     - Re-run ``/ameba-setup-boards`` to generate the configuration files.
   * - ``PORT_NOT_VISIBLE``
     - Make sure the dev board is plugged in.
   * - Flash stalls / cannot enter download mode
     - AMB82: set J27 jumper then press RESET.
       AMB82-MINI: hold UART_DOWNLOAD + press RESET simultaneously.
   * - ``ALIAS_REQUIRED`` / ``ALIAS_NOT_FOUND``
     - The error message lists available aliases; retry with the correct one.
   * - ``ameba-dev-pro2`` shows as disconnected
     - Confirm the first build has been completed and ``.venv`` exists, then restart Claude Code.
       Make sure ``<SDK_ROOT>`` in the registration command is a valid absolute path.
   * - Server fails to start (import error)
     - Run ``launcher.bat`` (Windows) or ``launcher.sh`` (Linux/macOS) manually in a terminal
       to view the full stderr output.

