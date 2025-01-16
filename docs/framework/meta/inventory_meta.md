---

Holds items within a grid layout.

Inventories are objects that contain `Item`s in a grid layout. Every `Character` will have exactly one inventory attached to it, which is the only inventory that is allowed to hold bags—any item that has its own inventory (i.e., a suitcase). Inventories can be owned by a character, or they can be individually interacted with as standalone objects. For example, the container plugin attaches inventories to props, allowing for items to be stored outside of any character inventories and remain "in the world".

---

## **getData**

**Description**

Retrieves data associated with a specified key from the inventory.

**Realm**

- **shared**

**Parameters**

- **key** (`string`): The key for the data.
- **default** (`any`, optional): The default value to return if the key does not exist.

**Returns**

- **any**: The value associated with the key, or the default value if the key does not exist.

**Example**

```lua
local health = inventory:getData("health", 100)
print("Health:", health)
```

---

## **extend**

**Description**

Extends the inventory to create a subclass with a specified class name.

**Realm**

- **shared**

**Parameters**

- **className** (`string`): The name of the subclass.

**Returns**

- **Table**: A subclass of the `Inventory` class.

**Example**

```lua
local GridInventory = Inventory:extend("GridInv")
```

---

## **configure**

**Description**

Configures the inventory.

This function is meant to be overridden in subclasses to define specific configurations.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

None.

**Example**

```lua
function GridInventory:configure()
    -- Custom configuration
end
```

---

## **addDataProxy**

**Description**

Adds a data proxy to the inventory for a specified key.

**Realm**

- **shared**

**Parameters**

- **key** (`any`): The key for the data proxy.
- **onChange** (`function`): The function to call when the data associated with the key changes.

**Returns**

None.

**Example**

```lua
inventory:addDataProxy("health", function(old, new)
    print("Health changed from", old, "to", new)
end)
```

---

## **getItemsByUniqueID**

**Description**

Retrieves items with a specified unique ID from the inventory.

**Realm**

- **shared**

**Parameters**

- **uniqueID** (`string`): The unique ID of the items to retrieve.
- **onlyMain** (`bool`): Whether to retrieve only main items.

**Returns**

- **Table**: An array containing the items with the specified unique ID.

**Example**

```lua
local weapons = inventory:getItemsByUniqueID("weapon_rifle")
for _, weapon in ipairs(weapons) do
    print("Weapon ID:", weapon:getID())
end
```

---

## **register**

**Description**

Registers the inventory with a specified type ID.

**Note:** This method sets the inventory's type and configures it accordingly.

**Realm**

- **shared**

**Parameters**

- **typeID** (`string`): The type ID to register the inventory with.

**Returns**

None.

**Example**

```lua
inventory:register("grid")
-- This sets the inventory's type to 'grid'
```

---

## **new**

**Description**

Creates a new instance of the inventory.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Table**: A new instance of the `Inventory` class.

**Example**

```lua
local newInventory = Inventory:new()
```

---

## **tostring**

**Description**

Returns a string representation of the inventory.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **String**: A string representation of the inventory, including its class name and ID.

**Example**

```lua
print(tostring(inventory))
-- Output: "GridInv[123]"
```

---

## **getType**

**Description**

Retrieves the type of the inventory.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Table**: The type information of the inventory.

**Example**

```lua
local typeInfo = inventory:getType()
print("Inventory Type:", typeInfo.typeID)
```

---

## **onDataChanged**

**Description**

Callback function called when data associated with a key changes.

**Realm**

- **shared**

**Parameters**

- **key** (`any`): The key whose data has changed.
- **oldValue** (`any`): The old value of the data.
- **newValue** (`any`): The new value of the data.

**Returns**

None.

**Example**

```lua
function Inventory:onDataChanged(key, oldValue, newValue)
    print(key, "changed from", oldValue, "to", newValue)
end
```

---

## **getItems**

**Description**

Retrieves all items in the inventory.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Table**: An array containing all items in the inventory.

**Example**

```lua
local items = inventory:getItems()
for _, item in ipairs(items) do
    print("Item ID:", item:getID())
end
```

---

## **getItemsOfType**

**Description**

Retrieves items of a specific type from the inventory.

**Realm**

- **shared**

**Parameters**

- **itemType** (`string`): The type of items to retrieve.

**Returns**

- **Table**: An array containing items of the specified type.

**Example**

```lua
local healthPacks = inventory:getItemsOfType("health_pack")
for _, pack in ipairs(healthPacks) do
    print("Health Pack ID:", pack:getID())
end
```

---

## **getFirstItemOfType**

**Description**

Retrieves the first item of a specific type from the inventory.

**Realm**

- **shared**

**Parameters**

- **itemType** (`string`): The type of item to retrieve.

**Returns**

- **Table|nil**: The first item of the specified type, or `nil` if not found.

**Example**

```lua
local firstHealthPack = inventory:getFirstItemOfType("health_pack")
if firstHealthPack then
    print("First Health Pack ID:", firstHealthPack:getID())
end
```

---

## **hasItem**

**Description**

Checks if the inventory contains an item of a specific type.

**Realm**

- **shared**

**Parameters**

- **itemType** (`string`): The type of item to check for.

**Returns**

- **Boolean**: Returns `true` if the inventory contains an item of the specified type, otherwise `false`.

**Example**

```lua
if inventory:hasItem("health_pack") then
    print("Inventory contains a health pack.")
else
    print("No health packs in inventory.")
end
```

---

## **getItemCount**

**Description**

Retrieves the total count of items in the inventory, optionally filtered by item type.

**Realm**

- **shared**

**Parameters**

- **itemType** (`string`, optional): The type of item to count. If `nil`, counts all items.

**Returns**

- **Integer**: The total count of items in the inventory, optionally filtered by item type.

**Example**

```lua
local totalItems = inventory:getItemCount()
print("Total Items:", totalItems)
local healthPackCount = inventory:getItemCount("health_pack")
print("Health Packs:", healthPackCount)
```

---

## **getID**

**Description**

Retrieves the ID of the inventory.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Integer**: The ID of the inventory.

**Example**

```lua
local invID = inventory:getID()
print("Inventory ID:", invID)
```

---

## **eq**

**Description**

Checks if two inventories are equal based on their IDs.

**Realm**

- **shared**

**Parameters**

- **other** (`Inventory`): The other inventory to compare with.

**Returns**

- **Boolean**: Returns `true` if the inventories have the same ID, otherwise `false`.

**Example**

```lua
if inventory1 == inventory2 then
    print("Both inventories are the same.")
else
    print("Inventories are different.")
end
```

---

## **addItem**

**Description**

Adds an item to the inventory.

**Realm**

- **server**

**Parameters**

- **item** (`Item`): The item to add to the inventory.
- **noReplicate** (`bool`): Set to `true` to prevent `OnItemAdded` from being called on the added item.

**Returns**

- **Inventory**: Returns the inventory itself.

**Example**

```lua
local weapon = lia.item.new("weapon_rifle")
inventory:addItem(weapon)
```

---

## **add**

**Description**

Alias for the `addItem` function.

**Realm**

- **server**

**Parameters**

- **item** (`Item`): The item to add to the inventory.

**Returns**

- **Inventory**: Returns the inventory itself.

**Example**

```lua
inventory:add(weapon)
```

---

## **syncItemAdded**

**Description**

Synchronizes the addition of an item with clients.

**Realm**

- **server**

**Parameters**

- **item** (`Item`): The item being added.

**Returns**

None.

**Example**

```lua
inventory:syncItemAdded(weapon)
```

---

## **initializeStorage**

**Description**

Initializes the storage for the inventory.

**Realm**

- **server**

**Parameters**

- **initialData** (`table`): Initial data for the inventory.

**Returns**

- **Deferred**: A deferred promise.

**Example**

```lua
local promise = inventory:initializeStorage({char = 1, item1 = "value1"})
promise:next(function(invID)
    print("Inventory initialized with ID:", invID)
end)
```

---

## **restoreFromStorage**

**Description**

Restores the inventory from storage.

**Realm**

- **server**

**Parameters**

None.

**Returns**

None.

**Example**

```lua
inventory:restoreFromStorage()
```

---

## **removeItem**

**Description**

Removes an item from the inventory.

**Realm**

- **server**

**Parameters**

- **itemID** (`int`): The ID of the item to remove.
- **preserveItem** (`bool`): Whether to preserve the item's data in the database.

**Returns**

- **Deferred**: A deferred promise.

**Example**

```lua
inventory:removeItem(12345, true):next(function()
    print("Item removed while preserving data.")
end)
```

---

## **remove**

**Description**

Alias for the `removeItem` function.

**Realm**

- **server**

**Parameters**

- **itemID** (`int`): The ID of the item to remove.

**Returns**

- **Deferred**: A deferred promise.

**Example**

```lua
inventory:remove(12345):next(function()
    print("Item removed.")
end)
```

---

## **setData**

**Description**

Sets data associated with a key in the inventory.

**Realm**

- **server**

**Parameters**

- **key** (`any`): The key to associate the data with.
- **value** (`any`): The value to set for the key.

**Returns**

- **Inventory**: Returns the inventory itself.

**Example**

```lua
inventory:setData("owner", player)
```

---

## **canAccess**

**Description**

Checks if a certain action is permitted for the inventory.

**Realm**

- **server**

**Parameters**

- **action** (`string`): The action to check for access.
- **context** (`table`): Additional context for the access check.

**Returns**

- **Boolean|nil**: Returns `true` if the action is permitted, `false` if denied, or `nil` if not applicable.
- **String** (optional): A reason for the access result.

**Example**

```lua
local canAccess, reason = inventory:canAccess("remove_item", {client = player})
if canAccess then
    print("Access granted.")
else
    print("Access denied:", reason)
end
```

---

## **addAccessRule**

**Description**

Adds an access rule to the inventory.

**Realm**

- **server**

**Parameters**

- **rule** (`function`): The access rule function.
- **priority** (`int`, optional): The priority of the access rule.

**Returns**

- **Inventory**: Returns the inventory itself.

**Example**

```lua
inventory:addAccessRule(function(inv, action, context)
    if action == "remove_item" and context.client:IsAdmin() then
        return true
    end
end, 10)
```

---

## **removeAccessRule**

**Description**

Removes an access rule from the inventory.

**Realm**

- **server**

**Parameters**

- **rule** (`function`): The access rule function to remove.

**Returns**

- **Inventory**: Returns the inventory itself.

**Example**

```lua
inventory:removeAccessRule(existingRuleFunction)
```

---

## **getRecipients**

**Description**

Retrieves the recipients for synchronization.

**Realm**

- **server**

**Parameters**

None.

**Returns**

- **Table**: An array containing the recipients for synchronization.

**Example**

```lua
local recipients = inventory:getRecipients()
for _, client in ipairs(recipients) do
    print("Syncing with client:", client:Nick())
end
```

---

## **onInstanced**

**Description**

Initializes an instance of the inventory.

**Realm**

- **server**

**Parameters**

None.

**Returns**

None.

**Example**

```lua
inventory:onInstanced()
```

---

## **onLoaded**

**Description**

Callback function called when the inventory is loaded.

**Realm**

- **server**

**Parameters**

None.

**Returns**

None.

**Example**

```lua
function Inventory:onLoaded()
    print("Inventory loaded.")
end
```

---

## **loadItems**

**Description**

Loads items from the database into the inventory.

**Realm**

- **server**

**Parameters**

None.

**Returns**

- **Deferred**: A deferred promise.

**Example**

```lua
inventory:loadItems():next(function(items)
    print("Items loaded:", #items)
end)
```

---

## **onItemsLoaded**

**Description**

Callback function called when items are loaded into the inventory.

**Realm**

- **server**

**Parameters**

None.

**Returns**

None.

**Example**

```lua
function Inventory:onItemsLoaded(items)
    print("Loaded", #items, "items into the inventory.")
end
```

---

## **instance**

**Description**

Instantiates a new inventory instance.

**Realm**

- **server**

**Parameters**

- **initialData** (`table`): Initial data for the inventory instance.

**Returns**

- **Table**: The newly instantiated inventory instance.

**Example**

```lua
local instance = inventory:instance({char = 1, item1 = "value1"})
```

---

## **syncData**

**Description**

Synchronizes data changes with clients.

**Realm**

- **server**

**Parameters**

- **key** (`any`): The key whose data has changed.
- **recipients** (`table`): The recipients to synchronize with.

**Returns**

None.

**Example**

```lua
inventory:syncData("health", {client = player})
```

---

## **sync**

**Description**

Synchronizes the inventory with clients.

**Realm**

- **server**

**Parameters**

- **recipients** (`table`, optional): The recipients to synchronize with.

**Returns**

None.

**Example**

```lua
inventory:sync()
```

---

## **delete**

**Description**

Deletes the inventory.

**Realm**

- **server**

**Parameters**

None.

**Returns**

None.

**Example**

```lua
inventory:delete()
```

---

## **destroy**

**Description**

Destroys the inventory and its associated items.

**Realm**

- **server**

**Parameters**

None.

**Returns**

None.

**Example**

```lua
inventory:destroy()
```

---

## **show**

**Description**

Displays the inventory UI to the specified parent element.

**Realm**

- **client**

**Parameters**

- **parent** (`any`, optional): The parent element to which the inventory UI will be displayed.

**Returns**

- **any**: The result of the `lia.inventory.show` function.

**Example**

```lua
inventory:show(panel)
```

---