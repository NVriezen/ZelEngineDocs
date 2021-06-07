.. _levels:

Levels
======
In the Zel Game Engine we use the word **level** to indicate a collection of entities. You can use these levels to create different menus or stages for your game.

Creating a level
^^^^^^^^^^^^^^^^
To create a level you can use the function ``zel_level_create``.

.. code-block:: cpp

	zel_level_t* example_level = zel_level_create("Example Level");

As you can see it returns a pointer to a struct called ``zel_level_t``.
For more information on ``zel_level_t`` see :ref:`here<zel_level_t>`.

.. warning::

	A level manager has not been implemented yet. So currently you need to set your level as the `active_level` in order to make the engine work.

Registering components
^^^^^^^^^^^^^^^^^^^^^^
Before you can use any components you need to register them.
This is to keep the data footprint as low as possible.
So to be able to use the transform component in the level, you can do the following.

.. code-block:: cpp

	zel_level_register_component<zel_transform_t>(example_level);

The recommended way is to register all components immediately after you created the level, but it's possible to register components at any time.

Some components need to clean up some stuff. For example; materials may need to destroy a shader or textures they've used. There is another function enabling that kind of behaviour.

.. code-block:: cpp

	zel_level_register_component_with_destroy<zel_material_t>(example_level, zel_material_destroy);

.. admonition:: Click me to learn more
	:class: toggle

	The reason that you need to register your components lies in the way the engine stores them. Components get stored in an instance of the templated class ``ZelComponent`` that inherits from ``ZelComponentBase``.
	
	As you can see in ``zel_level.h`` when you call ``zel_level_register_component`` a new instance of the class is created on the heap especially for that type of component.
	
	.. code-block:: cpp

		template <typename T>
		void zel_level_register_component(zel_level_t* level)
		{
			std::string type_name = typeid(T).name();

			if (level->components.find(type_name) != level->components.end())
			{
				//The component type is already registered
				return;
			}

			ZelComponent<T>* new_component_type = new ZelComponent<T>();
			ZelComponentBase* base_component_type = new_component_type;
			level->components.insert({ type_name, base_component_type });
		}

	If you forget to register a component type, then it can't be stored and the engine will crash.

	But why store the components like this?
	The goal is to reduce the amount of times the CPU has to load data from RAM, in some cases called a cache miss.
	We can do this by packing data closely together and only the data we access often together.
	So storing all the same type of data in a big array inside the ``ZelComponent`` class can help reduce those cache misses.

	For more insight in how this all works you can look at `zel_level.h`_, `zel_component_base.h`_ and `zel_component_class.h`_.

.. _zel_level.h: https://github.com/NVriezen/ZelGameEngine/blob/master/ZelGameEngine/include/zel_level.h

.. _zel_component_base.h: https://github.com/NVriezen/ZelGameEngine/blob/master/ZelGameEngine/include/zel_component_base.h

.. _zel_component_class.h: https://github.com/NVriezen/ZelGameEngine/blob/master/ZelGameEngine/include/zel_component_class.h

.. note::

	For more information about which components are built into the engine, please take a look at the :ref:`Components<components>` section.


Registering Systems
^^^^^^^^^^^^^^^^^^^
Systems also need to be registered to a level.
This way you can determine which systems actually get used in a level.

.. code-block:: cpp

	zel_level_register_system(example_level, example_system_update, example_system_name);

The function for registering needs the level of course, but also the system's update function and name. The name can be used to unregister the system again.

Creating entities
^^^^^^^^^^^^^^^^^
Components can't exist on their own.
They need to be attached to an entity.
You can create an entity with ``zel_level_create_entity``.

.. code-block:: cpp

	zel_entity_id entity = zel_level_create_entity(example_level);

This returns a ``zel_entity_id`` which identifies the entity inside the level.

See the sections :ref:`Entities<entities>`, :ref:`Components<components>` and :ref:`Systems<systems>` for more information.