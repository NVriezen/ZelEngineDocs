.. _getting_started_installation:

Installation
============
This page describes the process to install the Zel Game Engine on your computer.

Download
--------
First you need to download the required files.
There are two ways to receive these files.

The first one is through cloning the **Github** repository.
The repository can be found here: **link to repository**

Another way is to download the files.

Both can be done by going to the following repository.
[Link to Zel Game Engine repository](https://github.com/NVriezen/ZelGameEngine)


Github
------
Cloning a repository won't be explained in this documentation.
Luckily there are plenty of resources out there including [Github's own documentation](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository)

When you have cloned the repository, you can skip to "Setting Up Visual Studio".

Setting Up Visual Studio
------------------------
Right now the engine has only been used with Visual Studio 2015.
Other build environments won't be supported anytime soon.
Any Visual Studio version from VS2015 onwards should be good.

.. todo:: add visual studio instructions

Configurations
--------------
The engine works with two configurations; ``Debug`` and ``Release``.
Some features will be disabled in the Release configuration in the future.
Right now you can change some settings in 'zel_engine_settings.h' in the engine's include folder.

Testing the setup
-----------------
At this point you should be able to run the project.
Be sure to set the configuration on 'Debug'.

If everything went well, you won't be seeing any build errors.
A somewhat dark (blue) window should appear with a command prompt window.
In the command prompt window you can see the engine's version as well as "Hello World".

.. note: 
    If you do get errors or any other issue, please check if you followed the instructions correctly.
    If the problems persist, please open an issue on the documentation's [Github page]() describing the problem you have.

What's Next?
------------
Now that you have everything setup, it's time to test it out.
Let's write a Hello World program. //TODO: link to hello world