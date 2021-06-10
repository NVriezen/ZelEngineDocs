.. _entities:

Entities
========
Entities represent the concept of game objects.
A player can be an entity, an enemy can be an entity and a camera can also be represented as an entity.

You may look at an entity like a container of components.
You combine certain components together by adding it to an entity.
This way you can construct, for example, the player.

.. tip::

	Entities and Components work differently compared to most popular engines. Please read through the `Migration Guide <https://nvriezen.github.io/ZelMigrationGuide/>`_ to understand better how to use Entities and Components in the Zel Game Engine.

Entity IDs
^^^^^^^^^^
Entities in this engine's implementation are nothing more than 32-bit integers.

Index
"""""
The bottom 24 bits, 0 to 23, represent an index. With this index an entity can be identified as it represents its position in an array, from here on called entity spot.
Components can use this entity index to map components to that particular entity.

Generational Index
""""""""""""""""""
The top 8 bits, 24 to 31, are the generational index.
This is a special value which gets increased everytime an entity spot changes.
So when an entity gets created or destroyed in that spot.

When trying to get a component through an entity id, the generational index is compared.
If the generational index in that entity spot is different than the generational index in the entity id, it indicates the entity does not actually have that component or has been destroyed.

.. admonition:: Click me to learn more
	:class: toggle

	Components get stored in a class specific for that component type.
	Systems are just functions which get tracked by the level.
	Therefore Entities only have to be a unique number for them to work.
	With that unique ID we can identify groups of components.

	We use the generational index for some security and robustness.
	There are macros in the ``zel_entities.h`` file to help make an ID from an index and a generational index.

	.. code-block:: cpp

		#define ZEL_CAPACITY_GENERATION 0xFF
		#define ZEL_CAPACITY_ENTITIES 0xFFFFFF
		#define ZEL_MAX_ENTITIES 0xFFFF
		#define ZEL_GENERATION_BIT 24

		#define ZEL_GET_GENERATION(id) ((id & 0xFF000000) >> ZEL_GENERATION_BIT)
		#define ZEL_GET_INDEX(id) (id & ZEL_CAPACITY_ENTITIES)
		#define ZEL_CREATE_ID(generation, index) (generation << ZEL_GENERATION_BIT) | index


	Entities are simply stored inside ``zel_level_t``.
	The position inside the ``std::vector``, the index, decides part of their ID. The index in that vector is also the index used in the Component class for the mapping of components. 
	So the data in ``entity_to_component`` points to the component that is attached to a particular entity.
	If the entity has index 2 in the level struct, then its components can be found in ``entity_to_component[2]``.

	.. code-block:: cpp

		T* get_component(zel_entity_id entity)
		{
			zel_index entity_index = ZEL_GET_INDEX(entity);
			zel_generation entity_generation = ZEL_GET_GENERATION(entity);
			zel_generational_ptr component_pointer = entity_to_component[entity_index];
			if (component_pointer.generation != entity_generation)
			{
				zel_print("[zel_component class] get component return nullptr | Entity: %d | Type: %s\n", entity, typeid(T).name());
				return nullptr;
			}

			zel_index component_index = ZEL_GET_INDEX(component_pointer.id);
			return &components[component_index];
		}
	

Creating an entity
^^^^^^^^^^^^^^^^^^
Entities need to be created through a level.

.. code-block:: cpp

	zel_entity_id entity = zel_level_create_entity(example_level);

As you can see the function ``zel_level_create_entity`` returns a ``zel_entity_id``.
This is nothing more than a type definition.

.. code-block:: cpp

	typedef uint32_t zel_entity_id;

A 32-bit unsigned integer is used to represent an entity id, as was mentioned above. To learn more about how an entity id gets constructed, please see `zel_ecs.h <https://github.com/NVriezen/ZelGameEngine/blob/master/ZelGameEngine/include/zel_ecs.h#L11>`_.
