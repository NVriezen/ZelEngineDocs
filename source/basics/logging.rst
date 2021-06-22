.. _logging:

Logging
=======
When developing your game, you may need to check if something is working.
One of the easy ways to do this is to just print something to the console.
You can do this with ``zel_print``.

Printing
---------
The ``zel_print`` function is actually very simple.
It's just a wrapper for ``printf``.
So we can print a message in the same fashion as with ``printf``.

.. code-block:: cpp

	zel_print("Hello World!");		    //A simple string
	zel_print("A float: %0.2f", 3.14f);	    //A string with a float
	zel_print("An integer: %d", 6);		    //A string with an integer
	zel_print("Multiple: %d %0.1f", 6, 3.14f);  //Multiple variables in the string

Maybe you want to print which entity you are accessing.
An entity is just a 32-bit integer, so we can print it like a decimal.

.. code-block:: cpp

	zel_print("Accessing Entity: %d", player_entity);

Another thing you may want to check is the position of an enemy.
A position is a vector3, so we can print it like a float.

.. code-block:: cpp

	zel_transform_t enemy_transform;
	zel_print("Enemy position: %0.4f, %0.4f, %0.4f", enemy_transform.position.x, enemy_transform.position.y, enemy_transform.position.z);

If you are wondering what different options there are to format your message.
Just check the reference for ``printf`` on `cplusplus.com <https://www.cplusplus.com/reference/cstdio/printf/>`_.