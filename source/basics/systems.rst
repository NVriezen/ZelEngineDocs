.. _systems:

Systems
=======
Systems contain most of the logic. They process and manipulate the components.
That means you only have input through a combination of components. These components are obtained through entities.
Systems are in essence nothing more than a function.

Template
^^^^^^^^
Systems always need to have the same parameters and a name.
A template is included to make it easier to create systems.
In the ``Application`` project's ``include`` folder you will find ``example_system.h`` with the following code.
This file can be used for your own systems.

.. code-block:: cpp

	#pragma once
	#include <zel_api.h>
	#include <zel_math.h>

	const char* example_system_name = "example_system";

	void example_system_update(zel_level_t* level, float delta_time)
	{
		for (zel_entity_id entity : zel_entities_list<zel_transform_t>(level))
		{
			zel_level_get_component<zel_transform_t>(level, entity)->position.x += 1;
			//zel_print("New transform X position | entity [%d]: %0.1f\n", entity, zel_level_get_component<zel_transform_t>(level, entity)->position.x);
		}
	}

Registering a system
^^^^^^^^^^^^^^^^^^^^
Systems needs to be registered just like components.
This way you can turn systems on and off when you like.
To register a system you also need to input a name, which is used to unregister a system.

.. code-block:: cpp

	zel_level_register_system(example_level, example_system_update, "example_system");

Unregistering a system is done in somewhat the same way.

.. code-block:: cpp

	zel_level_unregister_system(example_level, "example_system");

.. admonition:: Click me to learn more
	:class: toggle

	Systems are literally functions with a name attached to them.

	.. code-block:: cpp

		typedef struct zel_level_t _zel_level_t;
		typedef void(*zel_system_t)(_zel_level_t* level, float delta_time);

	``zel_system_t`` is just a function pointer.
	It gets associated to a string, the system name, when you register a system in a level.

	.. code-block:: cpp

		void zel_level_register_system(zel_level_t* level, zel_system_t system_update, const char* system_name)
		{
			level->systems.insert({ system_name, system_update });
		}

For more information, see the :ref:`zel_level.h<zel_level_h>` section.

Entities list iterator
^^^^^^^^^^^^^^^^^^^^^^
You can get a list of entities through the ``zel_entities_list`` iterator.
This iterator goes through all entities and only returns the ones which have the components you need.

.. code-block:: cpp

	zel_entities_list<zel_transform_t> transform_list = zel_entities_list<zel_transform_t>(level);

Then to get the beginning or end of the list.

.. code-block:: cpp

	zel_entities_list<zel_transform_t>::iterator beginning_of_list = transform_list.begin();
	zel_entities_list<zel_transform_t>::iterator end_of_list = transform_list.end();

There is also an easy way to iterate over the entities in the list.
By using a for loop like so:

.. code-block:: cpp

	for (zel_entity_id entity : zel_entities_list<zel_transform_t>(level))
	{
		//use the transform component here
	}

This is the recommended way to use the ``zel_entities_list``.
It's also not restricted to only one type. You can get a list of entities which have a combination of multiple components.

.. code-block:: cpp

	for (zel_entity_id entity : zel_entities_list<zel_transform_t, zel_material_t, zel_mesh_t>(level))
	{
		//use the transform component here
	}

Accessing Entities in a system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
You may want to access a particular entity in a system.
For example you only want to process the enemy's components.
If all enemies have an ``enemy_ai`` component, you could use that component to identify all enemies.
Iterating over those enemies in a system would look like the following.

.. code-block:: cpp

	for (zel_entity_id entity : zel_entities_list<enemy_ai, zel_transform_t>(level))
	{
		//Only enemies with a transform component will be processed here
	}

Otherwise you could differentiate entities by adding a tag to them.
This would just be a component, but we call it a tag because it's a component without any data.

.. code-block:: cpp

	struct enemy_tag { };
	zel_level_register_component<enemy_tag>(example_level);
	zel_level_add_component(example_level, entity, enemy_tag);

	for (zel_entity_id entity : zel_entities_list<enemy_tag, zel_transform_t>(example_level))
	{
		//Only entities with the enemy tag and transform component will be processed here
	}


