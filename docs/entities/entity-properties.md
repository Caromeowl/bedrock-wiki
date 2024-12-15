---
title: Entity Properties
category: General
tags:
    - experimental
mentions:
    - SirLich
    - sermah
    - MedicalJewel105
    - Luthorius
    - stirante
    - TheItsNameless
    - SmokeyStack
description: Entity Properties were implemented to save data or store values on entities efficiently without needing the use of components or attributes in server-side of the entity, similar to Block Properties.
---

Documentation on the new Entity Properties, also known as Actor Properties, introduced in the 1.16.230.52 Minecraft: Bedrock Edition beta version.
Entity Properties were implemented to save data or store values on entities efficiently without needing the use of components or attributes (For example, "minecraft:variant") in server-side of the entity (Behavior Pack), similar to Block Properties.

## Entity Properties Definition

### Defining Properties on Entities

Entity Properties Definition:

<CodeHeader></CodeHeader>

```json
{
    "minecraft:entity": {
        "description": {
            "identifier": "wiki:properties_example",
            "properties": {
                "property:number_range_example": {
                    "values": {
                        "min": 0,
                        "max": 100
                    }
                },
                "property:number_enum_example": {
                    "values": [1, 2]
                },
                "property:string_enum_example": {
                    "values": ["first", "second", "third"]
                },
                "property:boolean_enum_example": {
                    "values": [true, false]
                }
            }
        }
    }
}
```

### Entity Properties Object Fields

#### `values`

:::warning
`values` property is required, and omitting this field may cause errors and failure to register the property.
:::

`values` field can be evaluated as an array of enum values, or a range of minimum and maximum values (Note that integer, float, and boolean enum values currently supports only two values):

<CodeHeader></CodeHeader>

```json
"property:range_example": {
    "values": {
      "min": 0,
      "max": 5
    }
}
```

**OR**

<CodeHeader></CodeHeader>

```json
"property:enum_example":{
    "values":[
        1,
        2
    ]
}
```

#### `default`

You can set the default value of the entity property (by default, the first value of the enum array index) through the `default` field inside the defined property object:

<CodeHeader></CodeHeader>

```json
"property:default_value_example":{
    "values":[
        true,
        false
    ],
    "default":false
}
```

As you can observe, the default property is set to `false` instead of `true` when the entity is spawned in the world.

#### `client_sync`

To sync through the Resource Pack (client-side), `client_sync` field can be used to allow the Client Entity access the Entity Properties. By default, the value is set to `false`.

<CodeHeader></CodeHeader>

```json
"property:client_sync_example": {
    "values": {
      "min": 0,
      "max": 20
    },
    "client_sync": true
}
```

### Manipulating and Accessing Entity Properties

You can access entity properties through Molang Entity Queries: - `q.property` - `q.has_property`

:::warning
These Molang Entity Queries are a part of Experimental features
:::

With entity events, you may set the entity property to a value with the `set_property` event response:

<CodeHeader></CodeHeader>

```json
"events":{
    "event:set_entity_property":{
        "set_property":{
            "property:number_enum_example":2,
            "property:string_enum_example":"'second'",
            "property:boolean_enum_example":"!q.property('property:boolean_enum_example')"
        }
    }
}
```

## Entity Aliases

:::warning
This feature has been deprecated a format_version of 1.21.10 or higher is specified
:::

You can define aliases for your entity to summon the entity with set entity property values through the `summon` command.
Entity can have various aliases with custom entity identifier to use:

<CodeHeader></CodeHeader>

```json
{
    "format_version": "1.16.0",
    "minecraft:entity": {
        "description": {
            "identifier": "wiki:properties_example",
            "is_spawnable": true,
            "is_summonable": true,
            "is_experimental": false,
            "properties": {
                "property:property_index": {
                    "client_sync": true,
                    "values": {
                        "min": 0,
                        "max": 2
                    },
                    "default": 0
                }
            },
            "aliases": {
                "wiki:default_alias": {},
                "wiki:first_alias": {
                    "property:property_index": 1
                },
                "wiki:second_alias": {
                    "property:property_index": 2
                }
            }
        }
    }
}
```

Now, the entity has multiple aliases, and you can use the defined alias identifier through the `summon` command to spawn the entity with the properties set: `/summon wiki:first_alias` will spawn the entity with the entity property `property:property_index` set to 1.
