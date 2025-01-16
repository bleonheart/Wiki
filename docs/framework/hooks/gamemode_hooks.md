---

These hooks are regular hooks that can be used in your schema with `SCHEMA:HookName(args)`, in your module with  
`MODULE:HookName(args)`, or in your addon with `hook.Add("HookName", function(args) end)`.

Below is a comprehensive list of available hooks and their purposes. Internal hooks are meant for internal usage by Lilia; you should only override them if you are absolutely certain you understand the implications.

---

## **OnPickupMoney**

**Description**
Called when a player picks up money from the ground.

**Realm**
- **server**

**Parameters**
- **client** (`Player`): The player who picked up the money.
- **moneyEntity** (`Entity`): The entity representing the money.

**Returns**
None.

**Example**
```lua
function SCHEMA:OnPickupMoney(client, moneyEntity)
    print(client:Name() .. " has picked up some money!")
end
```

---

## **OnItemSpawned**

**Description**
Called whenever an item entity has spawned in the world. You can access the item’s table by using `entity:getItemTable()`.

**Realm**
- **server**

**Parameters**
- **entity** (`Entity`): The spawned item entity.

**Returns**
None.

**Example**
```lua
function MODULE:OnItemSpawned(entity)
    local item = entity:getItemTable()
    print("An item has spawned: " .. (item and item.name or "Unknown"))
end
```

---

## **PlayerModelChanged**

**Description**
Called when a player's model is changed.

**Realm**
- **shared**

**Parameters**
- **client** (`Player`): The client whose model is changed.
- **model** (`string`): The new model path.

**Returns**
None.

**Example**
```lua
function SCHEMA:PlayerModelChanged(client, model)
    print(client:Name() .. " changed model to " .. model)
end
```

---

## **LoadFonts**

**Description**
Loads custom fonts for the client.

**Realm**
- **client**

**Parameters**
- **font** (`string`): The path to the font file.
- **genericFont** (`string`): The path to the generic font file.

**Returns**
None.

**Example**
```lua
function SCHEMA:LoadFonts(font, genericFont)
    -- Example: load a custom font for your schema
    surface.CreateFont("MyCustomFont", {
        font = font,
        size = 24,
        weight = 500
    })
end
```

---

## **LoadLiliaFonts (Internal)**

**Description**
Used internally by Lilia to load core fonts. Override or call this function only if you are absolutely certain about its implications.

**Realm**
- **client**

**Parameters**
- **font** (`string`): The path to the font file.
- **genericFont** (`string`): The path to the generic font file.

**Returns**
None.

*No usage example provided. This hook is meant for internal use.*

---

## **InitializedConfig**

**Description**
Called when `lia.config` has been fully initialized.

**Realm**
- **shared**

**Parameters**
None.

**Returns**
None.

**Example**
```lua
function SCHEMA:InitializedConfig()
    print("lia.config is now initialized!")
end
```

---

## **InitializedModules**

**Description**
Called when all modules have been initialized.

**Realm**
- **shared**

**Parameters**
None.

**Returns**
None.

**Example**
```lua
function SCHEMA:InitializedModules()
    print("All modules have been initialized.")
end
```

---

## **InitializedSchema**

**Description**
Called when the schema has been initialized.

**Realm**
- **shared**

**Parameters**
None.

**Returns**
None.

**Example**
```lua
function SCHEMA:InitializedSchema()
    print("The schema is fully initialized.")
end
```

---

## **ModuleLoaded**

**Description**
Called when a module has finished loading.

**Realm**
- **shared**

**Parameters**
None.

**Returns**
None.

**Example**
```lua
function SCHEMA:ModuleLoaded()
    print("A module has just been loaded.")
end
```

---

## **DatabaseConnected (Internal)**

**Description**
Used internally by Lilia, indicating that the database has been successfully connected. Override or call this function only if you are absolutely certain about its implications.

**Realm**
- **server**

**Parameters**
None.

**Returns**
None.

*No usage example provided. This hook is meant for internal use.*

---

## **OnWipeTables**

**Description**
Called after wiping tables in the database.

**Realm**
- **server**

**Parameters**
None.

**Returns**
None.

---

## **SetupDatabase (Internal)**

**Description**
Used internally by Lilia to set up the database. Override or call this function only if you are absolutely certain about its implications.

**Realm**
- **server**

**Parameters**
None.

**Returns**
None.

*No usage example provided. This hook is meant for internal use.*

---

## **SaveData**

**Description**
Saves the server data.

**Realm**
- **server**

**Parameters**
None.

**Returns**
None.

**Example**
```lua
function SCHEMA:SaveData()
    -- Write your saving logic here
    print("Data has been saved.")
end
```

---

## **LoadData**

**Description**
Loads the server data.

**Realm**
- **server**

**Parameters**
None.

**Returns**
None.

**Example**
```lua
function SCHEMA:LoadData()
    -- Write your loading logic here
    print("Data has been loaded.")
end
```

---

## **PostLoadData**

**Description**
Called after all data has been loaded.

**Realm**
- **server**

**Parameters**
None.

**Returns**
None.

**Example**
```lua
function SCHEMA:PostLoadData()
    print("All data has finished loading.")
end
```

---

## **ShouldDataBeSaved**

**Description**
Called to determine whether data should be saved before the server shuts down.

**Realm**
- **server**

**Parameters**
None.

**Returns**
- **bool**: `true` if data should be saved, `false` otherwise.

**Example**
```lua
function SCHEMA:ShouldDataBeSaved()
    -- Always save data
    return true
end
```

---

## **CanPlayerUnequipItem**

**Description**
Whether or not a player can unequip an item.

**Realm**
- **server**

**Parameters**
- **client** (`Player`): The player attempting to unequip an item.
- **item** (`table` or custom item object): The item being unequipped.

**Returns**
- **bool**: `true` if the player can unequip the item, `false` otherwise.

**Example**
```lua
function MODULE:CanPlayerUnequipItem(client, item)
    -- Example: disallow unequipping any item
    return false
end
```

---

## **CanPlayerDropItem**

**Description**
Whether or not a player is allowed to drop the specified item.

**Realm**
- **server**

**Parameters**
- **client** (`Player`): The player attempting to drop the item.
- **item** (`int` or `table`): The instance ID or the item table being dropped.

**Returns**
- **bool**: `true` if the player can drop the item, `false` otherwise.

**Example**
```lua
function MODULE:CanPlayerDropItem(client, item)
    -- Example: never allow dropping items
    return false
end
```

---

## **CanPlayerTakeItem**

**Description**
Whether or not a player is allowed to pick up (take) an item and put it in their inventory.

**Realm**
- **server**

**Parameters**
- **client** (`Player`): The player attempting to take the item.
- **item** (`Entity` or `table`): The entity or item table corresponding to the item.

**Returns**
- **bool**: `true` if the player can take the item, `false` otherwise.

**Example**
```lua
function MODULE:CanPlayerTakeItem(client, item)
    -- Example: disallow players in observer mode from taking items
    return not (client:GetMoveType() == MOVETYPE_NOCLIP and not client:InVehicle())
end
```

---

## **CanPlayerEquipItem**

**Description**
Whether or not a player can equip the given item. This is primarily called for items with `outfit`, `pacoutfit`, or `weapons` as their base.

**Realm**
- **server**

**Parameters**
- **client** (`Player`): The player attempting to equip the item.
- **item** (`table`): The item being equipped.

**Returns**
- **bool**: `true` if the player can equip the item, `false` otherwise.

**Example**
```lua
function MODULE:CanPlayerEquipItem(client, item)
    -- Example: restrict equipping items to staff only
    return client:isStaff()
end
```

---

## **CanPlayerInteractItem**

**Description**
Determines whether or not a player can interact with an item via an inventory action (e.g., picking up, dropping, transferring inventories, etc.). This hook is called after `CanPlayerDropItem` and `CanPlayerTakeItem`.

**Realm**
- **server**

**Parameters**
- **client** (`Player`): The player attempting to interact with the item.
- **action** (`string`): The action being performed (e.g., "drop", "pickup", "transfer").
- **item** (`int` or `table`): The item’s instance ID or item table.

**Returns**
- **bool**: `true` if the player can interact with the item, `false` otherwise.

**Example**
```lua
function MODULE:CanPlayerInteractItem(client, action, item)
    -- Example: disallow all item interactions
    return false
end
```

---