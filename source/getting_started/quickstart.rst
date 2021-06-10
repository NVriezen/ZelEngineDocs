.. _quickstart:

Quick Start
===========
This page describes the absolute minimum to get your project up and running

Setup
-----
To install the engine, please refer to :ref:`Installation<getting_started_installation>`

Migrating from a different engine
---------------------------------
You may have used a game engine before.
This engine works different from most engines.
Therefore this engine has its own `Migration Guide <https://nvriezen.github.io/ZelMigrationGuide/>`_.
It is the place to go when you've used other engines before.

This guide will help you get familiar with the Zel Game Engine.
It explains the differences between popular game engines you might have used before. Helping you change your way of thinking while using this engine.

I can not tell you enough how much this guide can help you, so please check it out.

User Code
---------
There are two projects, the ``ZelGameEngine`` which is the engine itself and ``Application`` which is your game.
The engine will run certain functions from ``zel_entry.cpp`` included in the ``Application`` project.
This is where you can plug your own code in.

.. tip::

	In the Solution Explorer in Visual Studio you can click on "Show All Files" to see the folders with the files. Do this for all the projects in the solution. It keeps things more organized.

Initialization
--------------
The ``zel_initialization`` function is the place to create your level.

Create entities and add components to those entities.
Then register a system to process those components.

Logic
-----
The function ``zel_logic`` is the place where systems get executed.
The systems process components and may change their data.
It takes ``delta_time`` as a parameter.
This is the passed time between the previous and current frame.

The for-loop inside ``zel_logic`` iterates over all registered systems and executes them.

.. note:: 

	The engine expect you to write code in systems. This means you don't write code directly in ``zel_logic``.

.. warning::

	``zel_logic`` may not be registered as a system through the ``zel_level_register_system`` function. This would create an infinite loop.

Render
------
Of course ``zel_render`` is the place where everything gets rendered.
This function should process all render data.
If you are unsure how the function does this, you may want to leave the code as is.

Logic and Rendering is seperated here because of the plans to multithread those functions in the future.
It also forces you to think modulair and can help in certain situations to maintain a pleasant gaming experience.

Termination
-----------
When the user wants to close the game ``zel_termination`` gets called.
If you need to free any memory, you would be able to do so here.

Including
---------
All functions and data containers that can be used in your application are exposed through ``zel_api.h`` which is always included in ``zel_entry.cpp``.
When you want to use engine specific functions inside your own files you only need to include ``zel_api.h``.
To see what exactly is available in the Zel API, please look at the ``zel_api.h`` file itself in the ``ZelGameEngine`` project.

What's Next?
------------
Now that you have a working project and a basic understanding of the engine's structure, you can check out the :ref:`Examples <examples>`.

If you've used a game engine before, you may want to check out the :ref:`Migration Guide <migrationguide>`.
It will help you better understand what this engine does differently from the engines you are used to.

