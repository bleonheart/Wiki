---

Physical representation of connected players.

Players are a type of Entity. They are a physical representation of a `Character` - and can possess at most one `Character` object at a time that you can interface with.

See the [Garry's Mod Wiki](https://wiki.garrysmod.com/page/Category:Player) for all other methods that the `Player` class has.

---

## **getChar**

**Description**

Retrieves this player's currently possessed `Character` object if it exists.

**Realm**

- **Shared**

**Returns**

- **Character|nil**: The currently loaded character or `nil` if no character is loaded.

**Example**

```lua
local char = player:getChar()
if char then
    print("Character Name:", char:getName())
end
```

---

## **Name**

**Description**

Returns this player's current name. If the player has a character loaded, it returns the character's name; otherwise, it returns the player's Steam name.

**Realm**

- **Shared**

**Returns**

- **String**: The name of the player's currently loaded character or the player's Steam name if no character is loaded.

**Example**

```lua
print("Player Name:", player:Name())
```

---

## **hasPrivilege**

**Description**

Checks if the player has a specified CAMI privilege.

**Realm**

- **Shared**

**Parameters**

- **privilegeName** (`String`): The name of the privilege to check.

**Returns**

- **Boolean**: `true` if the player has the privilege, `false` otherwise.

**Example**

```lua
if player:hasPrivilege("admin") then
    print("Player is an admin.")
end
```

---

## **getCurrentVehicle**

**Description**

Gets the current vehicle the player is in, if any.

**Realm**

- **Shared**

**Returns**

- **Entity|nil**: The current vehicle entity or `nil` if the player is not in a vehicle.

**Example**

```lua
local vehicle = player:getCurrentVehicle()
if vehicle then
    print("Player is in a vehicle:", vehicle:GetClass())
end
```

---

## **hasValidVehicle**

**Description**

Checks if the player is in a valid vehicle.

**Realm**

- **Shared**

**Returns**

- **Boolean**: `true` if the player is in a valid vehicle, `false` otherwise.

**Example**

```lua
if player:hasValidVehicle() then
    print("Player is in a valid vehicle.")
end
```

---

## **isNoClipping**

**Description**

Checks if the player is currently in noclip mode.

**Realm**

- **Shared**

**Returns**

- **Boolean**: `true` if the player is in noclip mode, `false` otherwise.

**Example**

```lua
if player:isNoClipping() then
    print("Player is in noclip mode.")
end
```

---

## **hasRagdoll**

**Description**

Checks if the player has a valid ragdoll entity.

**Realm**

- **Shared**

**Returns**

- **Boolean**: `true` if the player has a valid ragdoll entity, `false` otherwise.

**Example**

```lua
if player:hasRagdoll() then
    print("Player has a ragdoll.")
end
```

---

## **getRagdoll**

**Description**

Returns the player's ragdoll entity if it exists and is valid.

**Realm**

- **Shared**

**Returns**

- **Entity|nil**: The player's ragdoll entity or `nil` if none exists.

**Example**

```lua
local ragdoll = player:getRagdoll()
if ragdoll then
    print("Ragdoll found:", ragdoll:GetClass())
end
```

---

## **isStuck**

**Description**

Determines whether the player is stuck using a trace.

**Realm**

- **Shared**

**Returns**

- **Boolean**: `true` if the player is stuck, `false` otherwise.

**Example**

```lua
if player:isStuck() then
    print("Player is stuck.")
end
```

---

## **isNearPlayer**

**Description**

Checks if the player is near another entity within a specified radius.

**Realm**

- **Shared**

**Parameters**

1. **radius** (`int`): The radius within which to check for proximity.
2. **entity** (`Entity`): The entity to check proximity to.

**Returns**

- **Boolean**: `true` if the player is near the specified entity within the given radius, `false` otherwise.

**Example**

```lua
if player:isNearPlayer(100, targetPlayer) then
    print("Player is near the target.")
end
```

---

## **entitiesNearPlayer**

**Description**

Retrieves entities near the player within a specified radius. Can filter to only include players.

**Realm**

- **Shared**

**Parameters**

1. **radius** (`int`): The radius within which to search for entities.
2. **playerOnly** (`Bool`): If `true`, only return player entities.

**Returns**

- **Table**: A table containing the entities near the player.

**Example**

```lua
local nearbyPlayers = player:entitiesNearPlayer(200, true)
for _, p in ipairs(nearbyPlayers) do
    print("Nearby Player:", p:Nick())
end
```

---

## **getItemWeapon**

**Description**

Retrieves the active weapon item of the player along with its corresponding inventory item.

**Realm**

- **Shared**

**Returns**

- **Entity|nil**: The active weapon entity or `nil` if not found.
- **Item|nil**: The corresponding inventory item or `nil` if not found.

**Example**

```lua
local weapon, item = player:getItemWeapon()
if weapon then
    print("Active Weapon:", weapon:GetClass())
end
```

---

## **isRunning**

**Description**

Checks if the player is running based on their velocity compared to their walk speed.

**Realm**

- **Shared**

**Returns**

- **Boolean**: `true` if the player is running, `false` otherwise.

**Example**

```lua
if player:isRunning() then
    print("Player is running.")
end
```

---

## **isFemale**

**Description**

Determines if the player's character model is female based on model naming conventions.

**Realm**

- **Shared**

**Returns**

- **Boolean**: `true` if the player's character is female, `false` otherwise.

**Example**

```lua
if player:isFemale() then
    print("Player character is female.")
end
```

---

## **getItemDropPos**

**Description**

Calculates the position to drop an item from the player's inventory based on their current view.

**Realm**

- **Shared**

**Returns**

- **Vector**: The position where the item should be dropped.

**Example**

```lua
local dropPos = player:getItemDropPos()
item:spawn(dropPos, Angle(0, 0, 0))
```

---

## **getItems**

**Description**

Retrieves the items from the player's character inventory.

**Realm**

- **Shared**

**Returns**

- **Table|nil**: A table containing the items in the player's character inventory or `nil` if not found.

**Example**

```lua
local items = player:getItems()
if items then
    for _, item in ipairs(items) do
        print("Item:", item.uniqueID)
    end
end
```

---

## **getTracedEntity**

**Description**

Retrieves the entity traced by the player's aim within a short distance.

**Realm**

- **Shared**

**Returns**

- **Entity|nil**: The entity traced by the player's aim or `nil` if none is found.

**Example**

```lua
local target = player:getTracedEntity()
if target then
    print("Traced Entity:", target:GetClass())
end
```

---

## **getTrace**

**Description**

Performs a hull trace from the player's view and returns the trace result.

**Realm**

- **Shared**

**Returns**

- **Table**: A table containing the trace result.

**Example**

```lua
local trace = player:getTrace()
if trace.Hit then
    print("Trace hit:", trace.Entity:GetClass())
end
```

---

## **getEyeEnt**

**Description**

Retrieves the entity within the player's line of sight up to a specified distance.

**Realm**

- **Shared**

**Parameters**

- **distance** (`int`, optional): The maximum distance to consider. Defaults to `150`.

**Returns**

- **Entity|nil**: The entity within the player's line of sight or `nil` if none is found.

**Example**

```lua
local eyeEnt = player:getEyeEnt(200)
if eyeEnt then
    print("Entity in sight:", eyeEnt:GetClass())
end
```

---

## **loadLiliaData**

**Description**

Loads Lilia data for the player from the database.

**Realm**

- **Server**

**Parameters**

- **callback** (`function`): A function to call after the data is loaded, receiving the loaded data as an argument.

**Example**

```lua
player:loadLiliaData(function(data)
    print("Data loaded:", data)
end)
```

---

## **saveLiliaData**

**Description**

Saves the player's Lilia data to the database.

**Realm**

- **Server**

**Example**

```lua
player:saveLiliaData()
```

---

## **setLiliaData**

**Description**

Sets a key-value pair in the player's Lilia data. Optionally suppresses networking of the data update.

**Realm**

- **Server**

**Parameters**

1. **key** (`String`): The key for the data.
2. **value** (`any`, optional): The value to set for the specified key. Defaults to `nil` if not provided.
3. **noNetworking** (`boolean`, optional): If `true`, the data update will not be sent to clients. Defaults to `false`.

**Example**

```lua
-- Example 1: Setting a key-value pair with networking
player:setLiliaData("score", 1500)

-- Example 2: Setting a key-value pair without networking
player:setLiliaData("health", 100, true)
```

---

## **chatNotify**

**Description**

Notifies the player with a message in the chatbox.

**Realm**

- **Server** and ****Client****

**Parameters**

- **message** (`String`): The message to notify the player.

**Example**

```lua
player:chatNotify("Welcome to the server!")
```

---

## **chatNotifyLocalized**

**Description**

Notifies the player with a localized message in the chatbox.

**Realm**

- **Server** and ****Client****

**Parameters**

- **message** (`String`): The key of the localized message to notify the player.
- **...** (`any`): Additional arguments to format the localized message.

**Example**

```lua
player:chatNotifyLocalized("welcome_message", player:Nick())
```

---

## **chatError**

**Description**

Notifies the player with an error message in the chatbox.

**Realm**

- **Server** and ****Client****

**Parameters**

- **message** (`String`): The error message to notify the player.

**Example**

```lua
player:chatError("An error occurred!")
```

---

## **chatErrorLocalized**

**Description**

Notifies the player with a localized error message in the chatbox.

**Realm**

- **Server** and ****Client****

**Parameters**

- **message** (`String`): The key of the localized error message to notify the player.
- **...** (`any`): Additional arguments to format the localized message.

**Example**

```lua
player:chatErrorLocalized("error_message", player:Nick())
```

---

## **getLiliaData**

**Description**

Retrieves a value from the player's Lilia data.

**Realm**

- **Server** and ****Client****

**Parameters**

1. **key** (`String`): The key for the data.
2. **default** (`any`, optional): The default value to return if the key does not exist.

**Returns**

- **any**: The value corresponding to the key, or the default value if the key does not exist.

**Example**

```lua
local level = player:getLiliaData("level", 1)
print("Player Level:", level)
```

---

## **getAllLiliaData**

**Description**

Retrieves the full Lilia data table for the player.

**Realm**

- **Server** and ****Client****

**Returns**

- **table**: The entire Lilia data table for the player.

**Example**

```lua
local fullData = player:getAllLiliaData()
-- Print all data for debugging
PrintTable(fullData)
```

---

## **setRagdoll**

**Description**

Sets the player's ragdoll entity.

**Realm**

- **Server**

**Parameters**

- **entity** (`Entity`): The entity to set as the player's ragdoll.

**Example**

```lua
local ragdoll = player:createServerRagdoll()
player:setRagdoll(ragdoll)
```

---

## **setAction**

**Description**

Sets an action bar for the player with optional duration and callback.

**Realm**

- **Server**

**Parameters**

1. **text** (`String`): The text to display on the action bar.
2. **time** (`int`, optional): The duration for the action bar to display in seconds. Set to `0` or `nil` to remove the action bar immediately.
3. **callback** (`function`, optional): Function to execute when the action bar timer expires.
4. **startTime** (`int`, optional): The start time of the action bar, defaults to the current time.
5. **finishTime** (`int`, optional): The finish time of the action bar, defaults to `startTime + time`.

**Example**

```lua
player:setAction("Processing...", 10, function(p) print("Action completed for", p:Nick()) end)
```

---

## **stopAction**

**Description**

Stops the action bar currently being displayed for the player.

**Realm**

- **Server**

**Example**

```lua
player:stopAction()
```

---

## **playSound**

**Description**

Plays a sound for the player.

**Realm**

- **Server**

**Parameters**

1. **sound** (`String`): The sound to play.
2. **volume** (`int`, optional): The volume of the sound. Defaults to `75`.
3. **pitch** (`int`, optional): The pitch of the sound. Defaults to `100`.
4. **shouldEmit** (`Bool`): Whether to emit the sound server-side (`true`) or send it to the client (`false`).

**Example**

```lua
player:playSound("ambient/alarms/warningbell1.wav", 100, 100, false)
```

---

## **openUI**

**Description**

Opens a VGUI panel for the player.

**Realm**

- **Server**

**Parameters**

- **panel** (`String`): The name of the VGUI panel to open.

**Example**

```lua
player:openUI("InventoryPanel")
```

---

## **openPage**

**Description**

Opens a web page for the player.

**Realm**

- **Server**

**Parameters**

- **url** (`String`): The URL of the web page to open.

**Example**

```lua
player:openPage("https://example.com")
```

---

## **requestDropdown**

**Description**

Requests a dropdown selection from the player.

**Realm**

- **Shared**

**Parameters**

1. **title** (`String`): The title of the request.
2. **subTitle** (`String`): The subtitle of the request.
3. **options** (`Table`): The table of options to choose from.
4. **callback** (`function`): The function to call upon receiving the selected option.

**Example**

```lua
player:requestDropdown("Choose Option", "Select one of the following:", {"Option1", "Option2"}, function(selected)
    print("Player selected:", selected)
end)
```

---

## **requestOptions**

**Description**

Requests multiple options selection from the player.

**Realm**

- **Server**

**Parameters**

1. **title** (`String`): The title of the request.
2. **subTitle** (`String`): The subtitle of the request.
3. **options** (`Table`): The table of options to choose from.
4. **limit** (`int`): The maximum number of selectable options.
5. **callback** (`function`): The function to call upon receiving the selected options.

**Example**

```lua
player:requestOptions("Select Items", "Choose up to 3 items:", {"Item1", "Item2", "Item3"}, 3, function(selected)
    print("Player selected:", table.concat(selected, ", "))
end)
```

---

## **requestString**

**Description**

Requests a string input from the player. Supports both callback and promise-based usage.

**Realm**

- **Server**

**Parameters**

1. **title** (`String`): The title of the string input dialog.
2. **subTitle** (`String`): The subtitle or description of the string input dialog.
3. **callback** (`function`, optional): The function to call with the entered string.
4. **default** (`String`, optional): The default value for the string input.

**Returns**

- **Promise|nil**: A promise object resolving with the entered string, or `nil` if a callback is provided.

**Example**

```lua
-- Using callback
player:requestString("Enter Name", "Please enter your name:", function(name)
    print("Player entered:", name)
end)

-- Using promise
local promise = player:requestString("Enter Name", "Please enter your name:", "DefaultName")
if promise then
    promise:next(function(name)
        print("Player entered:", name)
    end)
end
```

---

## **binaryQuestion**

**Description**

Requests a binary choice from the player.

**Realm**

- **Server**

**Parameters**

1. **question** (`String`): The question to present to the player.
2. **option1** (`String`): The text for the first option.
3. **option2** (`String`): The text for the second option.
4. **manualDismiss** (`Bool`): Whether the notice should be manually dismissed.
5. **callback** (`function`): The function to call with the choice (`0` or `1`) when the player selects an option.

**Example**

```lua
player:binaryQuestion("Confirm Action", "Yes", "No", false, function(choice)
    if choice == 0 then
        print("Player chose Yes.")
    else
        print("Player chose No.")
    end
end)
```

---

## **getPlayTime**

**Description**

Retrieves the player's total playtime.

**Realm**

- **Server** and ****Client****

**Returns**

- **Float**: The total playtime of the player in seconds.

**Example**

```lua
local playTime = player:getPlayTime()
print("Playtime:", playTime, "seconds")
```

---

## **createServerRagdoll**

**Description**

Creates a ragdoll entity for the player on the server.

**Realm**

- **Server**

**Parameters**

- **dontSetPlayer** (`Bool`): Determines whether to associate the player with the ragdoll.

**Returns**

- **Entity**: The created ragdoll entity.

**Example**

```lua
local ragdoll = player:createServerRagdoll()
```

---

## **doStaredAction**

**Description**

Performs a stared action towards an entity for a certain duration. Executes callbacks upon completion or cancellation.

**Realm**

- **Server**

**Parameters**

1. **entity** (`Entity`): The entity towards which the player performs the stared action.
2. **callback** (`function`): The function to call when the stared action is completed.
3. **time** (`int`, optional): The duration of the stared action in seconds. Defaults to `5`.
4. **onCancel** (`function`): The function to call if the stared action is canceled.
5. **distance** (`int`, optional): The maximum distance for the stared action. Defaults to `96`.

**Example**

```lua
player:doStaredAction(targetEntity, function()
    print("Stared action completed.")
end, 10, function()
    print("Stared action canceled.")
end, 150)
```

---

## **notify**

**Description**

Notifies the player with a message using the Lilia notice system.

**Realm**

- **Server**

**Parameters**

- **message** (`String`): The message to notify the player.

**Example**

```lua
player:notify("You have received a new item!")
```

---

## **notifyLocalized**

**Description**

Notifies the player with a localized message using the Lilia notice system.

**Realm**

- **Server**

**Parameters**

- **message** (`String`): The key of the localized message to notify the player.
- **...** (`any`): Additional arguments to format the localized message.

**Example**

```lua
player:notifyLocalized("welcome_message", player:Nick())
```

---

## **createRagdoll**

**Description**

Creates a ragdoll entity for the player. Can optionally freeze the ragdoll initially.

**Realm**

- **Server**

**Parameters**

- **freeze** (`Bool`): Whether to freeze the ragdoll initially.

**Returns**

- **Entity**: The created ragdoll entity.

**Example**

```lua
local ragdoll = player:createRagdoll(true)
```

---

## **setRagdolled**

**Description**

Sets the player to a ragdolled state or removes the ragdoll.

**Realm**

- **Server**

**Parameters**

1. **state** (`Bool`): Whether to set the player to a ragdolled state (`true`) or remove the ragdoll (`false`).
2. **time** (`int`, optional): The duration for which the player remains ragdolled.
3. **getUpGrace** (`int`, optional): The grace period for the player to get up before the ragdoll is removed.
4. **getUpMessage** (`String`, optional): The message displayed when the player is getting up. Defaults to `"@wakingUp"`.

**Example**

```lua
player:setRagdolled(true, 10, 5, "@gettingUp")
```

---

## **syncVars**

**Description**

Synchronizes networked variables with the player.

**Realm**

- **Server**

**Example**

```lua
player:syncVars()
```

---

## **setLocalVar**

**Description**

Sets a local variable for the player and synchronizes it over the network.

**Realm**

- **Server**

**Parameters**

1. **key** (`String`): The key of the variable.
2. **value** (`any`): The value of the variable.

**Example**

```lua
player:setLocalVar("health", 100)
```

---