---

Physical objects in the game world.

Entities are physical representations of objects in the game world. Lilia extends the functionality of entities to interface between Lilia's own classes and to reduce boilerplate code.

See the [Garry's Mod Wiki](https://wiki.garrysmod.com/page/Category:Entity) for all other methods that the `Entity` class has.

---

## **isProp**

**Description**

Checks if the entity is a physics prop.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Boolean**: `true` if the entity is a physics prop, `false` otherwise.

**Example**

```lua
if entity:isProp() then
    print("Entity is a physics prop.")
else
    print("Entity is not a physics prop.")
end
```

---

## **isItem**

**Description**

Checks if the entity is an item entity.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Boolean**: `true` if the entity is an item entity, `false` otherwise.

**Example**

```lua
if entity:isItem() then
    print("Entity is an item.")
end
```

---

## **isMoney**

**Description**

Checks if the entity is a money entity.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Boolean**: `true` if the entity is a money entity, `false` otherwise.

**Example**

```lua
if entity:isMoney() then
    print("Entity is money.")
end
```

---

## **isSimfphysCar**

**Description**

Checks if the entity is a simfphys car.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Boolean**: `true` if the entity is a simfphys car, `false` otherwise.

**Example**

```lua
if entity:isSimfphysCar() then
    print("Entity is a simfphys car.")
end
```

---

## **getEntItemDropPos**

**Description**

Retrieves the drop position for an item associated with the entity.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Vector**: The drop position for the item.

**Example**

```lua
local dropPos = entity:getEntItemDropPos()
print("Item drop position:", dropPos)
```

---

## **isNearEntity**

**Description**

Checks if there is an entity near the current entity within a specified radius.

**Realm**

- **shared**

**Parameters**

- **radius** (`float`): The radius within which to check for nearby entities.

**Returns**

- **Boolean**: `true` if there is an entity nearby, `false` otherwise.

**Example**

```lua
if entity:isNearEntity(150) then
    print("There is an entity nearby.")
else
    print("No entities within the specified radius.")
end
```

---

## **GetCreator**

**Description**

Retrieves the creator of the entity.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Player|nil**: The player who created the entity, or `nil` if no valid creator is found.

**Example**

```lua
local creator = entity:GetCreator()
if creator then
    print("Entity was created by:", creator:Nick())
end
```

---

## **SetCreator**

**Description**

Assigns a creator to the entity.

**Realm**

- **server**

**Parameters**

- **client** (`Player`): The player to assign as the creator of the entity.

**Returns**

None.

**Example**

```lua
entity:SetCreator(player)
```

---

## **sendNetVar**

**Description**

Sends a networked variable.

**Realm**

- **server**
- **@internal**

**Parameters**

- **key** (`string`): Identifier of the networked variable.
- **receiver** (`Player|nil`): The players to send the networked variable to.

**Returns**

None.

**Example**

```lua
entity:sendNetVar("health", player)
```

*This function is internal and should only be used if you are absolutely certain about its implications.*

---

## **clearNetVars**

**Description**

Clears all of the networked variables.

**Realm**

- **server**
- **@internal**

**Parameters**

- **receiver** (`Player|nil`): The players to clear the networked variables for.

**Returns**

None.

**Example**

```lua
entity:clearNetVars(player)
```

*This function is internal and should only be used if you are absolutely certain about its implications.*

---

## **setNetVar**

**Description**

Sets the value of a networked variable.

**Realm**

- **server**

**Parameters**

- **key** (`string`): Identifier of the networked variable.
- **value** (`any`): New value to assign to the networked variable.
- **receiver** (`Player|nil`): The players to send the networked variable to.

**Returns**

None.

**Example**

```lua
entity:setNetVar("example", "Hello World!", player)
```

---

## **getNetVar**

**Description**

Retrieves a networked variable. If it is not set, it'll return the default that you've specified.

**Realm**

- **server**
- **client**

**Parameters**

- **key** (`string`): Identifier of the networked variable.
- **default** (`any`): The default value to return if the networked variable does not exist.

**Returns**

- **any**: The value associated with the key, or the default that was given if it doesn't exist.

**Example**

```lua
local example = entity:getNetVar("example", "Default Value")
print(example) -- Output: "Hello World!" or "Default Value"
```

---