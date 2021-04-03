.. _getting_started_quickstart:

Quick Start
===========
This page describes the absolute minimum to get your project up and running


Setup
-----
To setup the engine, please refer to :ref:`Installation<getting_started_installation>`

Initialization
--------------
The ``zel_initialization`` function is the place to create your level.

Create entities and add components to those entities.
Then register a system to process those components.

Logic
-----
The function ``zel_logic`` is the place where systems get executed.
They process components and may change their data.

It takes ``delta_time`` as a parameter.
This is the passed time between the previous and current frame.

Render
------
Of course ``zel_render`` is the place where everything gets rendered.
Right now you can only clear the screen with a color.
Soon the Rendering API will get re-implemented and then this function should process all render data.

Logic and Rendering is seperated here because of the plans to multithread those functions in the future.
It also forces you to think modulair and can help in certain situations to maintain a pleasant gaming experience.

Termination
-----------
When the user wants to close the game ``zel_termination`` gets called.
If you need to free any memory, you would be able to do so here.

What's Next?
------------
Now that you have a working project and a basic understanding of the engine's structure, you can check out the :ref:`Examples <examples>`.

