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
		zel_component_collection<zel_transform_t> transform_collection = zel_component_collection_create<zel_transform_t>(level);
		for (size_t component_index = 1; component_index < transform_collection.length; component_index++)
		{
			zel_transform_t* transform = &(*transform_collection.first)[component_index];
			transform->position.x += 1;
			//zel_print("New transform X position | entity [%d]: %0.1f\n", transform_collection.entities[component_index], transform->position.x);
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
To access entities in systems there are actual three ways to do it.
The key is to think about what data you need, how you access the data and profiling your code is important.

So there is something called **entity collection**, which is simply a ``std::vector`` that contains entities.
Then we have the **component collection**, which is a struct containing pointers to the components.
Finally there is the **duo collection**, which, as the name may suggest, contains entities as well as components.

The easiest collection to use would be the entity collection.
It will return a list of entities that have the components you want to access.

.. code-block:: cpp

	std::vector<zel_entity_id> entity_collection = zel_entity_collection_create<zel_transform_t>(level);

You can simply iterate over the entities because it's a ``std::vector``.
Then to access a component you can simply call ``zel_level_get_component`` with the entity as parameter.

.. code-block:: cpp

	std::vector<zel_entity_id> entity_collection = zel_entity_collection_create<zel_transform_t, zel_material_t, zel_mesh_t>(level);
	for (size_t collection_index = 0; collection_index < entity_collection.size(); collection_index++)
	{
		zel_entity_id entity = entity_collection[collection_index];
		zel_transform_t* transform_of_entity = zel_level_get_component<zel_transform_t>(level, entity);
	}

As you can see in the code snippet above it is also possible to insert multiple component types. This will return all entities which have exactly that combination of components attached to them.

Accessing Entities in a system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
You may want to access a particular entity in a system.
For example you only want to process the enemy's components.
If all enemies have an ``enemy_ai`` component, you could use that component to identify all enemies.
Iterating over those enemies in a system would look like the following.

.. code-block:: cpp

	std::vector<zel_entity_id> entity_collection = zel_entity_collection_create<enemy_ai, zel_transform_t>(level);
	for (size_t collection_index = 0; collection_index < entity_collection.size(); collection_index++)
	{
		//Only enemies with an enemy_ai and transform component will be processed here
	}

Otherwise you could differentiate entities by adding a tag to them.
This would just be a component, but we call it a tag because it's a component without any data.

.. code-block:: cpp

	//Put this at the top of a file
	struct enemy_tag { };

	//This will go alongside other initialization code
	zel_level_register_component<enemy_tag>(example_level);
	zel_level_add_component(example_level, entity, enemy_tag);

	//This will basically be your system
	std::vector<zel_entity_id> entity_collection = zel_entity_collection_create<zel_transform_t, enemy_tag>(level);
	for (size_t collection_index = 0; collection_index < entity_collection.size(); collection_index++)
	{
		//Only entities with the enemy tag and transform component will be processed here
	}


