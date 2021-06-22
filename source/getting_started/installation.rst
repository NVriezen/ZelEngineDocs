.. _getting_started_installation:

Installation
============
This page describes the process to install the Zel Game Engine on your machine.

.. attention:: 

	Make sure to not have any spaces in the file path! So instead of having ``C:/My Projects/Zel Game Engine/ZelGameEngine.sln`` you need to remove the spaces and do it like this ``C:/My_Projects/Zel_Game_Engine/ZelGameEngine.sln``

Downloading The Engine
----------------------
First you need to download the required files.
This can be done by cloning the Github repository or just downloading it directly.

If you've never used Github before the recommended option is to go to :ref:`downloading_rep`

Cloning The Repository
^^^^^^^^^^^^^^^^^^^^^^
Cloning a repository won't be explained in this documentation.
Luckily there are plenty of resources out there including `Github's own documentation`_.

.. _Github's own documentation: https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository

The engine's repository can be found `here <https://github.com/NVriezen/ZelGameEngine>`_.
It has most of the files needed for the engine.

When you have cloned the repository, you can skip to :ref:`setting_up_vs`.

.. _downloading_rep:

Downloading The Repository
^^^^^^^^^^^^^^^^^^^^^^^^^^
Downloading the engine is rather simple.
Go to `the engine's repository <https://github.com/NVriezen/ZelGameEngine>`_.
Then click on the green **code** button and click on **Download Zip** in the dropdown menu.
This way you'll download the latest master version of the engine.

.. _setting_up_vs:

Setting Up Visual Studio
------------------------
Right now the engine has been used with Visual Studio 2015, 2017 and 2019.
Any Visual Studio version from VS2015 onwards should therefore be good.
Other build environments won't be supported anytime soon.

Open the solution through ``ZelGameEngine.sln``
You may see an upgrade prompt when opening the solution.
Make sure to click "OK", otherwise the engine probably will not run.

.. note::

	If you are already past this point, you need to upgrade the solution manually.
	This can be done by going into the Properties of each project.
	Then set the Platform Toolset and SDK to the installed versions on your system.

After Visual Studio has loaded the solution, you'll see two projects.
One named ``Application`` and another named ``ZelGameEngine``.

Configurations
^^^^^^^^^^^^^^
The engine works with two configurations: ``Debug`` and ``Release``.
Some features may be disabled in the Release configuration.
The output console, where all messages from ``zel_print`` will appear, is one of them.

Testing the setup
^^^^^^^^^^^^^^^^^
At this point you should be able to run the project.
Be sure to set the configuration on ``Debug``.

If everything went well, you won't be seeing any build errors.
A somewhat dark (blue) window should appear with a command prompt window.
In the command prompt window you can see the engine's version as well as "Hello World".

.. note:: 
    If you do get errors or any other issue, please **check again** if you followed the instructions correctly. If the problems persist, please open an issue on the documentation's `Github issues page <https://github.com/NVriezen/ZelEngineDocs/issues>`_ describing the problem you have.

What's Next?
------------
Now that you have everything setup, you can start creating your game.
It is **highly recommended** to read the :ref:`Migration Guide<migrationguide>` first. It will be the best way to get you familiar with the Zel Game Engine. It will explain the differences between this engine and the ones you are used to.

After that you can take a look at the :ref:`Quick Start<quickstart>` section to start writing your first code. Don't forget to keep the :ref:`Migration Guide<migrationguide>` at hand for when you don't understand something. It will definitely help you out.

If you prefer to dive straight in, there are some :ref:`Examples<examples>` available and you can dig through the :ref:`API Reference<api>`.
Some pages have *"Click Me"* sections which give you an in-depth explanation about the code.