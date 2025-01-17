---

These hooks are regular hooks that can be used in your schema with `SCHEMA:HookName(args)`, in your module with  
`MODULE:HookName(args)`, or in your addon with `hook.Add("HookName", function(args) end)`.

Below is a comprehensive list of available hooks and their purposes. Internal hooks are meant for internal usage by Lilia; you should only override them if you are absolutely certain you understand the implications.

---

### **CreateDefaultInventory**
**Description:**  
Called when creating a default inventory for a character. Should return a [deferred](https://github.com/Be1zebub/luassert-deferred) (or similar promise) object that resolves with the new inventory.

**Realm:**  
Server

**Example:**
```lua
hook.Add("CreateDefaultInventory", "MyDefaultInventory", function(character)
    local d = deferred.new()

    someInventoryCreationFunction(character)
        :next(function(inventory)
            d:resolve(inventory)
        end, function(err)
            d:reject(err)
        end)

    return d
end)
```

---

### **CharRestored**
**Description:**  
Called after a character has been restored from the database. Useful for post-restoration logic such as awarding default items or setting up data.

**Realm:**  
Server

**Example:**
```lua
hook.Add("CharRestored", "OnCharRestored", function(character)
    print("Character has been restored:", character:getName())
    -- Additional logic here
end)
```

---

### **PreCharDelete**
**Description:**  
Called before a character is deleted. Allows for clean-up tasks or checks before removal from DB.

**Realm:**  
Server

**Parameters:**
- `id` (`number`): The ID of the character to be deleted.

**Example:**
```lua
hook.Add("PreCharDelete", "LogBeforeDelete", function(id)
    print("Character with ID " .. id .. " is about to be deleted.")
end)
```

---

### **OnCharDelete**
**Description:**  
Called after a character is deleted. Finalize any remaining actions or remove associated data.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`): The player who triggered the deletion.
- `id` (`number`): The ID of the deleted character.

**Example:**
```lua
hook.Add("OnCharDelete", "NotifyCharDeletion", function(client, id)
    print("Character with ID " .. id .. " was deleted by:", client:Name())
end)
```

---

### **OnCharVarChanged**
**Description:**  
Called when a character variable changes (server-side). Useful for responding to data updates.

**Realm:**  
Server

**Parameters:**
- `character` (Character): The character object.
- `key` (`string`): The name of the variable.
- `oldValue` (`any`): The old value.
- `newValue` (`any`): The new value.

**Example:**
```lua
hook.Add("OnCharVarChanged", "HandleCharVarChange", function(character, key, oldValue, newValue)
    print("Var " .. key .. " changed from", oldValue, "to", newValue)
end)
```

---

### **PlayerModelChanged**
**Description:**  
Called when a player's model changes.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`): The player whose model changed.
- `model` (`string`): The new model path.

**Example:**
```lua
hook.Add("PlayerModelChanged", "UpdateEquipment", function(client, model)
    print(client:Name() .. " changed model to:", model)
end)
```

---

### **GetDefaultCharName**
**Description:**  
Retrieves a default name for a character during creation. Return `(defaultName, overrideBool)`.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`): The player creating the character.
- `faction` (`number`): The faction index.
- `data` (`table`): Additional creation data.

**Returns:**
- `string`: The default name.
- `bool`: Whether to override the user-provided name.

**Example:**
```lua
hook.Add("GetDefaultCharName", "FactionDefaultName", function(client, faction, data)
    if faction == FACTION_POLICE then
        return "John Doe", true
    end
end)
```

---

### **GetDefaultCharDesc**
**Description:**  
Retrieves a default description for a character during creation. Return `(defaultDesc, overrideBool)`.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `faction` (`number`)

**Returns:**
- `string`: The default description.
- `bool`: Whether to override.

**Example:**
```lua
hook.Add("GetDefaultCharDesc", "FactionDefaultDesc", function(client, faction)
    if faction == FACTION_CITIZEN then
        return "A regular citizen of the city.", true
    end
end)
```

---

### **ShouldHideBars**
**Description:**  
Determines whether all HUD bars should be hidden.

**Realm:**  
Client

**Returns:**
- `bool|nil`: `true` to hide, `nil` to allow rendering.

**Example:**
```lua
hook.Add("ShouldHideBars", "HideBarsWhileInMenu", function()
    if gui.IsGameUIVisible() then
        return true
    end
end)
```

---

### **ShouldBarDraw**
**Description:**  
Determines whether a specific HUD bar should be drawn.

**Realm:**  
Client

**Parameters:**
- `barName` (`string`): e.g. `"health"`, `"armor"`.

**Returns:**
- `bool|nil`: `false` to hide, `nil` to allow.

**Example:**
```lua
hook.Add("ShouldBarDraw", "HideArmorBar", function(barName)
    if barName == "armor" then
        return false
    end
end)
```

---

### **InitializedChatClasses**
**Description:**  
Called once all chat classes have been initialized.

**Realm:**  
Shared

**Example:**
```lua
hook.Add("InitializedChatClasses", "AddCustomChatClass", function()
    lia.chat.register("oocGlobal", {
        prefix = {"/oocg"},
        format = "[GLOBAL OOC] %s: \"%s\"",
        onCanHear = function(_, _) return true end
    })
end)
```

---

### **PlayerMessageSend**
**Description:**  
Called before a chat message is sent. Return `false` to cancel, or modify the message if returning a string.

**Realm:**  
Shared

**Parameters:**
- `speaker` (`Player`)
- `chatType` (`string`)
- `message` (`string`)
- `anonymous` (`bool`)

**Returns:**
- `bool|nil|modifiedString`: `false` to cancel, or return a modified string to change the message.

**Example:**
```lua
hook.Add("PlayerMessageSend", "ModifyICMessages", function(speaker, chatType, message, anonymous)
    if chatType == "ic" then
        return message:upper() -- Make IC messages uppercase
    end
end)
```

---

### **GetDisplayedName**
**Description:**  
Called to get the displayed name for a speaker in a specific chat type.

**Realm:**  
Shared

**Parameters:**
- `speaker` (`Player`)
- `chatType` (`string`)

**Returns:**
- `string|nil`: A custom name, or `nil` for default.

**Example:**
```lua
hook.Add("GetDisplayedName", "AdminNameTag", function(speaker, chatType)
    if speaker:isStaff() then
        return "[ADMIN] " .. speaker:Name()
    end
end)
```

---

### **CanPlayerJoinClass**
**Description:**  
Determines whether a player can join a certain class. Return `false` to block.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `class` (`number`)
- `info` (`table`)

**Returns:**  
- `bool|nil`: `false` to block, `nil` to allow.

**Example:**
```lua
hook.Add("CanPlayerJoinClass", "RestrictPoliceClass", function(client, class, info)
    if class == FACTION_POLICE and not client:isStaff() then
        return false
    end
end)
```

---

### **InitializedClasses**
**Description:**  
Called after all classes have been initialized.

**Realm:**  
Shared

**Example:**
```lua
hook.Add("InitializedClasses", "AddCustomClass", function()
    CLASS.name = "Elite Soldier"
    CLASS.faction = FACTION_POLICE
    CLASS.isDefault = false
    CLASS.index = #lia.class.list + 1
    lia.class.list[CLASS.index] = CLASS
end)
```

---

### **CanPlayerUseCommand**
**Description:**  
Determines if a player can use a specific command. Return `false` to block usage.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `command` (`string`)

**Returns:**  
- `bool|nil`: `false` to block, `nil` to allow.

**Example:**
```lua
hook.Add("CanPlayerUseCommand", "RestrictCommands", function(client, command)
    if command == "Fly" and not client:isSuperAdmin() then
        return false
    end
end)
```

---

### **ShouldDataBeSaved**
**Description:**  
Determines whether data should be saved during server shutdown.

**Realm:**  
Server

**Returns:**  
- `bool`: `true` to save, `false` to skip.

**Example:**
```lua
hook.Add("ShouldDataBeSaved", "PreventDataSave", function()
    if someCondition then
        return false
    end
end)
```

---

### **PostLoadData**
**Description:**  
Called after all data has been loaded.

**Realm:**  
Server

**Example:**
```lua
hook.Add("PostLoadData", "InitializePlayerStats", function()
    for _, ply in ipairs(player.GetAll()) do
        local stats = lia.data.get("player_" .. ply:getChar():getID(), {kills = 0, deaths = 0})
        ply:setKills(stats.kills)
        ply:setDeaths(stats.deaths)
    end
end)
```

---

### **SaveData**
**Description:**  
Saves all relevant data to disk, triggered during map cleanup and shutdown.

**Realm:**  
Server

**Example:**
```lua
hook.Add("SaveData", "SavePlayerData", function()
    for _, ply in ipairs(player.GetAll()) do
        local char = ply:getChar()
        if char then
            lia.data.set("player_" .. char:getID(), char:getData(), false, false)
        end
    end
end)
```

---

### **LoadData**
**Description:**  
Loads all relevant data from disk, typically after map cleanup.

**Realm:**  
Server

**Example:**
```lua
hook.Add("LoadData", "LoadPlayerData", function()
    for key, _ in pairs(lia.data.stored) do
        if string.StartWith(key, "player_") then
            local data = lia.data.get(key, {})
            print("Loaded data for:", key, "Kills:", data.kills)
        end
    end
end)
```

---

### **OnMySQLOOConnected**
**Description:**  
Called when MySQLOO successfully connects to the database. Use to register prepared statements or init DB logic.

**Realm:**  
Server

**Example:**
```lua
hook.Add("OnMySQLOOConnected", "RegisterDBStatements", function()
    lia.db.prepare("insertPlayer", "INSERT INTO lia_players (_steamID, _steamName) VALUES (?, ?)", {MYSQLOO_STRING, MYSQLOO_STRING})
end)
```

---

### **LiliaTablesLoaded**
**Description:**  
Called after all essential DB tables have been loaded.

**Realm:**  
Server

**Example:**
```lua
hook.Add("LiliaTablesLoaded", "InitializeGameState", function()
    print("All essential tables for Lilia have been loaded.")
end)
```

---

### **OnLoadTables**
**Description:**  
Called before the faction tables are loaded. Good spot for data setup prior to factions being processed.

**Realm:**  
Shared

**Example:**
```lua
hook.Add("OnLoadTables", "PreFactionSetup", function()
    print("Preparing to load faction tables.")
end)
```

---

### **PersistenceSave**
**Description:**  
Called to save persistent data (like map entities), often during map cleanup or shutdown.

**Realm:**  
Server

**Usage Example:**
```lua
hook.Run("PersistenceSave")
```

---

### **RegisterPreparedStatements**
**Description:**  
Called for registering DB prepared statements post-MySQLOO connection.

**Realm:**  
Server

**Usage Example:**
```lua
hook.Run("RegisterPreparedStatements")
```

---

### **CharCleanUp**
**Description:**  
Used during character cleanup routines for additional steps when removing or transitioning a character.

**Realm:**  
Server

**Parameters:**
- `character`: The character being cleaned up.

**Usage Example:**
```lua
hook.Run("CharCleanUp", character)
```

---

### **CreateInventoryPanel**
**Description:**  
Client-side call when creating the graphical representation of an inventory.

**Realm:**  
Client

**Parameters:**  
- `inventory`
- `parent` (`Panel`)

**Returns:**  
`Panel|nil` — A custom panel or nil for default logic.

**Example:**
```lua
hook.Add("CreateInventoryPanel", "CustomInventoryUI", function(inventory, parent)
    local panel = vgui.Create("DPanel", parent)
    panel:SetSize(300, 300)
    return panel
end)
```

---

### **OnItemRegistered**
**Description:**  
Called after an item has been registered. Useful for customizing item behavior or adding properties.

**Realm:**  
Shared

**Parameters:**  
- `item` (Item)

**Example:**
```lua
hook.Add("OnItemRegistered", "CustomItemSetup", function(item)
    print("Item Registered:", item.uniqueID)
end)
```

---

### **InitializedItems**
**Description:**  
Called once all item modules have been loaded from a directory.

**Realm:**  
Shared

**Example:**
```lua
hook.Add("InitializedItems", "PostItemInitialization", function()
    print("All items have been initialized.")
end)
```

---

### **OnServerLog**
**Description:**  
Called whenever a new log message is added. Allows for custom logic or modifications to log handling.

**Realm:**  
Server

**Parameters:**  
- `client` (`Player`) or `nil`
- `logType` (`string`)
- `logString` (`string`)
- `category` (`string`)
- `color` (`Color`)

**Example:**
```lua
hook.Add("OnServerLog", "NotifyAdmins", function(client, logType, logString, category, color)
    if logType == "playerJoin" then
        for _, admin in ipairs(player.GetAll()) do
            if admin:isStaff() then
                lia.log.send(admin, logString)
            end
        end
    end
end)
```

---

### **DoModuleIncludes**
**Description:**  
Called when modules include submodules. Useful for advanced module handling or dependency management.

**Realm:**  
Shared

**Parameters:**  
- `path` (`string`)
- `module` (`table`)

**Example:**
```lua
hook.Add("DoModuleIncludes", "CustomModuleInclude", function(path, module)
    print("Including submodule from path:", path)
end)
```

---

### **OnFinishLoad**
**Description:**  
Called after a module finishes loading. Ideal for final logic once all module files are processed.

**Realm:**  
Shared

**Parameters:**  
- `path` (`string`): The file path.
- `firstLoad` (`bool`): Whether this is the first load of the module.

**Example:**
```lua
hook.Add("OnFinishLoad", "PostModuleLoad", function(path, firstLoad)
    print("Module loaded from path:", path, "First Load:", firstLoad)
end)
```

---

### **InitializedSchema**
**Description:**  
Called after the schema has finished initializing.

**Realm:**  
Shared

**Example:**
```lua
hook.Add("InitializedSchema", "PostSchemaInit", function()
    print("Schema is fully initialized.")
end)
```

---

### **InitializedModules**
**Description:**  
Called after all modules are fully initialized.

**Realm:**  
Shared

**Example:**
```lua
hook.Add("InitializedModules", "PostModulesInit", function()
    print("All modules have been initialized.")
end)
```

---

### **OnPickupMoney**
**Description:**  
Called when a player picks up money from the ground.

**Realm:**  
Server

**Parameters:**  
- `client` (`Player`)
- `moneyEntity` (`Entity`)

**Example:**
```lua
function SCHEMA:OnPickupMoney(client, moneyEntity)
    print(client:Name() .. " has picked up some money!")
end
```

---

### **OnItemSpawned**
**Description:**  
Called whenever an item entity spawns in the world. Use `entity:getItemTable()` to access item data.

**Realm:**  
Server

**Parameters:**
- `entity` (`Entity`)

**Example:**
```lua
function OnItemSpawned(entity)
    local item = entity:getItemTable()
    print("An item has spawned:", item and item.name or "Unknown")
end
```

---

### **LoadFonts**
**Description:**  
Loads custom fonts for the client.

**Realm:**  
Client

**Parameters:**
- `font` (`string`)
- `genericFont` (`string`)

**Example:**
```lua
function SCHEMA:LoadFonts(font, genericFont)
    surface.CreateFont("MyCustomFont", {
        font = font,
        size = 24,
        weight = 500
    })
end
```
### **LoadLiliaFonts**

**Description:**  
Used internally to load core fonts. Override only if you understand the implications.

**Realm:**  
Client

**Parameters:**
- `font` (`string`)
- `genericFont` (`string`)

---

### **InitializedConfig**
**Description:**  
Called when `lia.config` is fully initialized.

**Realm:**  
Shared

**Example:**
```lua
function SCHEMA:InitializedConfig()
    print("lia.config is now initialized!")
end
```

---

### **ModuleLoaded**
**Description:**  
Called when a module has finished loading (post-load logic).

**Realm:**  
Shared

**Example:**
```lua
function SCHEMA:ModuleLoaded()
    print("A module has just been loaded.")
end
```

---

### **DatabaseConnected**
**Description:**  
Indicates successful MySQLOO DB connection. Used internally.

**Internal:**  
This function is intended for internal use and should not be called directly.

**Realm:**  
Server

---

### **OnWipeTables**
**Description:**  
Called after wiping tables in the DB, typically after major resets/cleanups.

**Realm:**  
Server

**Example:**
```lua
hook.Add("OnWipeTables", "AfterWipeCleanup", function()
    print("All relevant database tables have been wiped.")
end)
```

---

### **SetupDatabase**
**Description:**  
Used internally by Lilia to set up the database.

**Internal:**  
This function is intended for internal use and should not be called directly.

**Realm:**  
Server

---

### **CanPlayerUnequipItem**
**Description:**  
Determines whether a player can unequip a given item.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `item` (`table` or custom item object`)

**Returns:**  
- `bool`: `true` if allowed, `false` if blocked.

**Example:**
```lua
function CanPlayerUnequipItem(client, item)
    return false
end
```

---

### **CanPlayerDropItem**
**Description:**  
Determines if a player is allowed to drop a specific item.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `item` (`int` or `table`)

**Returns:**  
- `bool`: `true` to allow, `false` to block.

**Example:**
```lua
function CanPlayerDropItem(client, item)
    return false
end
```

---

### **CanPlayerTakeItem**
**Description:**  
Determines if a player can pick up an item into their inventory.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `item` (`Entity` or `table`)

**Returns:**  
- `bool`: `true` if allowed, `false` otherwise.

**Example:**
```lua
function CanPlayerTakeItem(client, item)
    -- Disallow picking up items while in noclip
    return not (client:GetMoveType() == MOVETYPE_NOCLIP)
end
```

---

### **CanPlayerEquipItem**
**Description:**  
Determines if a player can equip a given item (e.g., outfits, weapons).

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `item` (`table`)

**Returns:**  
- `bool`

**Example:**
```lua
function CanPlayerEquipItem(client, item)
    return client:IsSuperAdmin()
end
```

---

### **CanPlayerInteractItem**
**Description:**  
Determines if a player can interact with an item (pick up, drop, transfer, etc.). Called after other checks.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `action` (`string`): e.g. `"drop"`, `"pickup"`, `"transfer"`
- `item` (`int` or `table`)

**Returns:**  
- `bool`: `true` if allowed, `false` otherwise.

**Example:**
```lua
function CanPlayerInteractItem(client, action, item)
    return false
end
```

---

### **EntityRemoved**
**Description:**  
Called when an entity is removed. Often used for cleaning up net variables.

**Realm:**  
Server

**Parameters:**  
- `entity` (`Entity`)

**Example:**
```lua
hook.Add("EntityRemoved", "nCleanUp", function(entity)
    entity:clearNetVars()
end)
```

---

### **PlayerInitialSpawn**
**Description:**  
Called when a player initially spawns on the server. Used to sync net vars or send data to the client.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

**Example:**
```lua
hook.Add("PlayerInitialSpawn", "nSync", function(client)
    client:syncVars()
end)
```

---

### **PostPlayerInitialSpawn**
**Description:**  
Called after a player has fully initialized (post-initial-spawn logic).

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

---

### **PlayerLiliaDataLoaded**
**Description:**  
Called when Lilia has finished loading all its data for a player (e.g., characters, inventories).

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

---

### **OnCharDisconnect**
**Description:**  
Called when a player disconnects while having a valid character. Often used for partial saves or cleanups.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `character` (Character)

---

### **ShouldSpawnClientRagdoll**
**Description:**  
Determines if a client ragdoll should spawn (e.g., on death or fallover).

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

**Returns:**  
- `bool` (`false` to prevent ragdoll)

---

### **PlayerUse**
**Description:**  
Called when a player attempts to use an entity recognized as a door or player.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `entity` (`Entity`)

---

### **ShouldMenuButtonShow**
**Description:**  
Determines if a particular menu button should appear (e.g., character creation).

**Realm:**  
Client

**Parameters:**
- `buttonName` (`string`)

**Returns:**  
1. `bool|nil` — Return `false` to hide, or `nil` to allow.  
2. `string|nil` — Optional reason or message.

---

### **OnItemCreated**
**Description:**  
Called when an item entity is *created* in the world (different from *OnItemSpawned*).

**Realm:**  
Server

**Parameters:**
- `itemTable` (`table`)
- `entity` (`Entity`)

---

### **getItemDropModel**
**Description:**  
Returns an alternate model path for a dropped item instead of the default.

**Realm:**  
Server

**Parameters:**
- `itemTable` (`table`)
- `entity` (`Entity`)

**Returns:**
- `string|nil`: An alternate path, or `nil` for default.

---

### **HandleItemTransferRequest**
**Description:**  
Triggered when the client sends a request to transfer an item from one inventory slot to another.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `itemID` (`number`)
- `x`, `y` (coordinates or nil)
- `invID` (target inventory ID or nil)

---

### **SetupBagInventoryAccessRules**
**Description:**  
Configure or override rules for a *bag* inventory (e.g., restricting categories).

**Realm:**  
Server or Shared

**Parameters:**
- `inventory` (Inventory)

---

### **CanPickupMoney**
**Description:**  
Determines if a player can pick up a money entity from the ground.

**Realm:**  
Server

**Parameters:**
- `activator` (`Player`)
- `entity` (`Entity`)

**Returns:**  
- `bool` (`false` to block)

---

### **GetMoneyModel**
**Description:**  
Returns a custom model path for a money entity based on its amount.

**Realm:**  
Server

**Parameters:**
- `amount` (`number`)

**Returns:**  
- `string|nil`

---

### **CanOutfitChangeModel**
**Description:**  
Determines whether an outfit item can change the player’s model.

**Realm:**  
Server

**Parameters:**
- `item` (Item)

**Returns:**  
- `bool` (`false` to block)

---

### **OnCharPermakilled**
**Description:**  
Called when a character is permanently killed.

**Realm:**  
Server or Shared

**Parameters:**
- `character` (Character)
- `time` (`number|nil`)

---

### **ItemCombine**
**Description:**  
Called when the system attempts to combine one item with another in an inventory.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `item` (Item)
- `targetItem` (Item)

**Returns:**  
- `bool`: `true` if combination is valid and consumed, `false` otherwise.

---

### **OnCharFallover**
**Description:**  
Called when a character ragdolls or “falls over.”

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `unknown` (any)
- `bool` (forced or not)

---

### **AddCreationData**
**Description:**  
Allows modifying or adding extra data steps in the character creation process (server side).

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `data` (`table`)
- `newData` (`table`)
- `originalData` (`table`)

---

### **CharListUpdated**
**Description:**  
Called when the character list for a player is updated (client side).

**Realm:**  
Client

**Parameters:**
- `oldCharList` (`table`)
- `newCharList` (`table`)

---

### **ConfigureCharacterCreationSteps**
**Description:**  
Allows schemas/modules to insert or modify steps in the char creation UI (client side).

**Realm:**  
Client

**Parameters:**
- `panel` (`Panel`)

---

### **CanDeleteChar**
**Description:**  
Checks if a character can be deleted before the UI or server attempts it.

**Realm:**  
Client or Shared

**Parameters:**
- `charID` (`number`)

**Returns:**  
- `bool`: `false` to block
- `string|nil`: Optional reason

---

### **KickedFromChar**
**Description:**  
Called when a player is forcibly kicked from their current character.

**Realm:**  
Client

**Parameters:**
- `id` (`number`): The character ID
- `isCurrentChar` (`bool`)

---

### **LiliaLoaded**
**Description:**  
Called when Lilia’s client-side scripts have fully loaded.

**Realm:**  
Client

---

### **PostRenderVGUI**
**Description:**  
For rendering hooks after VGUI is drawn (often used for extra drawing calls).

**Realm:**  
Client

---

### **VendorClassUpdated**
**Description:**  
Called when a vendor’s allowed classes are updated.

**Realm:**  
Client

**Internal:**  
This function is intended for internal use and should not be called directly.

**Parameters:**  
- `vendor` (`Entity`)
- `id` (`string`): Class ID
- `allowed` (`bool`)

---

### **VendorFactionUpdated**
**Description:**  
Called when a vendor’s allowed factions are updated.

**Realm:**  
Client

**Internal:**  
This function is intended for internal use and should not be called directly.

**Parameters:**
- `vendor` (`Entity`)
- `id` (`string`)
- `allowed` (`bool`)

---

### **VendorItemMaxStockUpdated**
**Description:**  
Called when a vendor’s item max stock is updated.

**Realm:**  
Client

**Internal:**  
This function is intended for internal use and should not be called directly.

**Parameters:**  
- `vendor` (`Entity`)
- `itemType` (`string`)
- `value` (`int`)

---

### **VendorItemStockUpdated**
**Description:**  
Called when a vendor’s item stock is updated.

**Realm:**  
Client

**Internal:**  
This function is intended for internal use and should not be called directly.

**Parameters:**  
- `vendor` (`Entity`)
- `itemType` (`string`)
- `value` (`int`)

---

### **VendorItemModeUpdated**
**Description:**  
Called when a vendor’s item mode is updated.

**Realm:**  
Client

**Internal:**  
This function is intended for internal use and should not be called directly.

**Parameters:**  
- `vendor` (`Entity`)
- `itemType` (`string`)
- `value` (`int`)

---

### **VendorItemPriceUpdated**
**Description:**  
Called when a vendor’s item price is updated.

**Realm:**  
Client

**Internal:**  
This function is intended for internal use and should not be called directly.

**Parameters:**  
- `vendor` (`Entity`)
- `itemType` (`string`)
- `value` (`int`)

---

### **VendorMoneyUpdated**
**Description:**  
Called when a vendor’s money is updated.

**Realm:**  
Client

**Internal:**  
This function is intended for internal use and should not be called directly.

**Parameters:**  
- `vendor` (`Entity`)
- `money` (`int`)
- `oldMoney` (`int`)

---

### **VendorEdited**
**Description:**  
Called after a delay when a vendor’s data is edited.

**Realm:**  
Client

**Internal:**  
This function is intended for internal use and should not be called directly.

**Parameters:**  
- `vendor` (`Entity`)
- `key` (`string`)

---

### **VendorTradeEvent**
**Description:**  
Called when a player attempts to trade with a vendor.

**Realm:**  
Server

**Internal:**  
This function is intended for internal use and should not be called directly.

**Parameters:**  
- `client` (`Player`)
- `entity` (`Entity`): The vendor
- `uniqueID` (`string`): The item ID
- `isSellingToVendor` (`bool`)

---

### **VendorSynchronized**
**Description:**  
Called when vendor synchronization data is received.

**Realm:**  
Client

**Parameters:**  
- `vendor` (`Entity`)

---

### **CanPlayerAccessVendor**
**Description:**  
Determines whether a player can access a vendor.

**Realm:**  
Server

**Parameters:**  
- `client` (`Player`)
- `entity` (`Entity`): The vendor

**Returns:**  
- `bool`

---

### **getPriceOverride**
**Description:**  
Gets the price override for an item in a vendor.

**Realm:**  
Shared

**Parameters:**  
- `self` (`Entity`): The vendor
- `uniqueID` (`string`): The item ID
- `price` (`int`)
- `isSellingToVendor` (`bool`)

**Returns:**  
- `int`: Overridden price

---

### **OnCharTradeVendor**
**Description:**  
Called when a character trades with a vendor.

**Realm:**  
Server

**Parameters:**  
- `client` (`Player`)
- `vendor` (`Entity`)
- `item` (`Entity` or item table)
- `isSellingToVendor` (`bool`)
- `character` (Character)

---

### **PlayerAccessVendor**
**Description:**  
Called when a player accesses a vendor.

**Realm:**  
Server

**Parameters:**  
- `activator` (`Player`)
- `self` (`Entity`): The vendor

---

### **VendorExited**
**Description:**  
Called when a player exits from interacting with a vendor.

**Realm:**  
Client

---

### **VendorOpened**
**Description:**  
Called when a vendor is opened (client side).

**Realm:**  
Client

**Parameters:**  
- `vendor` (`Entity`)

---

### **OnOpenVendorMenu**
**Description:**  
Called when the vendor menu is opened.

**Realm:**  
Client

**Parameters:**  
- `self` (`Entity`): The vendor

---

### **FactionOnLoadout**
**Description:**  
Called after `PlayerLoadout` is executed, specifically for faction loadout.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

---

### **ClassOnLoadout**
**Description:**  
Called after `FactionOnLoadout` is executed, specifically for class loadout.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

---

### **FactionPostLoadout**
**Description:**  
Called after `ClassOnLoadout`, for extra faction loadout logic.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

---

### **ClassPostLoadout**
**Description:**  
Called after `FactionPostLoadout`, for extra class loadout logic.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

---

### **PostPlayerLoadout**
**Description:**  
Called after all player loadout hooks (PlayerLoadout, FactionOnLoadout, ClassOnLoadout) have finished.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

---

### **ChatTextChanged**
**Description:**  
Called when the text in the chat input box changes.

**Realm:**  
Client

**Parameters:**
- `text` (`string`)

---

### **FinishChat**
**Description:**  
Called when the chat input box is closed.

**Realm:**  
Client

---

### **ChatAddText**
**Description:**  
Called to add text to the chat. Allows formatting or modifying text before display.

**Realm:**  
Client

**Parameters:**
- `text` (`string`) — the markup
- `...` additional text arguments

**Returns:**  
- `string` modified text markup

---

### **StartChat**
**Description:**  
Called when the chat input box is opened.

**Realm:**  
Client

---

### **PostPlayerSay**
**Description:**  
Called after a player sends a chat message.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `message` (`string`)
- `chatType` (`string`)
- `anonymous` (`bool`)

---

### **OnChatReceived**
**Description:**  
Called after a player sends a chat message.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `message` (`string`)
- `chatType` (`string`)
- `anonymous` (`bool`)

---

### **CanDisplayCharInfo**
**Description:**  
Determines whether certain information can be displayed in the character info panel of the F1 menu.

**Realm:**  
Client

**Parameters:**
- `suppress` (table)  
  - Example keys: `name`, `desc`, `faction`, `money`, `class`

**Example Usage:**
```lua
function MODULE:CanDisplayCharInfo(suppress)
    suppress.faction = true
end
```

---

### **CanPlayerViewInventory (F1Menu)**
*(Alias for F1 menu usage.)*  
Determines if a player is allowed to open/view their inventory from F1 menu.

**Realm:**  
Client

**Returns:**  
- `bool`

---

### **BuildHelpMenu**
**Description:**  
Called when the help menu is being built.

**Realm:**  
Client

**Parameters:**  
- `tabs` (table): Contains help menu tabs.

---

### **CreateMenuButtons**
**Description:**  
Creates menu buttons for the F1 menu.

**Realm:**  
Client

**Parameters:**  
- `tabs` (table)

---

### **PostDrawInventory (F1Menu)**
*(Alias for after the player's inventory is drawn in the F1 menu.)*

**Realm:**  
Client

**Parameters:**  
- `panel` (the inventory panel)

---

### **ShouldDrawAmmoHUD**
**Description:**  
Whether or not the ammo HUD should be drawn.

**Realm:**  
Client

**Parameters:**
- `weapon` (Entity or table)

**Returns:**  
- `bool`

---

### **ShouldDrawCrosshair (Alias 1)**
**Description:**  
Determines whether the crosshair should be drawn for a specific client & weapon.

**Realm:**  
Client

**Parameters:**
- `client` (`Player`)
- `weapon` (Entity or string)

**Returns:**  
- `bool`

*(Note: Some sources also show a separate `ShouldDrawCrosshair()` with no parameters—naming can differ by codebase.)*

---

### **ShouldDrawCharInfo**
**Description:**  
Determines whether character info should be drawn for the given entity/character.

**Realm:**  
Client

**Parameters:**
- `entity` (`Entity`)
- `character` (Character)
- `charInfo` (table)

**Returns:**  
- `bool`

---

### **ShouldDrawEntityInfo**
**Description:**  
Determines whether entity info should be drawn for the given entity.

**Realm:**  
Client

**Parameters:**
- `entity` (`Entity`)

**Returns:**  
- `bool`

---

### **DrawEntityInfo**
**Description:**  
Draws information about the given entity.

**Realm:**  
Client

**Parameters:**
- `entity` (`Entity`)
- `alpha` (`float`)

---

### **DrawCrosshair (Alias 1)**
**Description:**  
Draws the crosshair (if `ShouldDrawCrosshair` is `true`).

**Realm:**  
Client

---

### **ShouldDrawVignette**
**Description:**  
Determines whether the vignette effect should be drawn.

**Realm:**  
Client

**Returns:**  
- `bool`

---

### **DrawVignette**
**Description:**  
Draws the vignette effect.

**Realm:**  
Client

---

### **ShouldDrawBranchWarning**
**Description:**  
Determines whether a branching path warning should be drawn.

**Realm:**  
Client

**Returns:**  
- `bool`

---

### **DrawBranchWarning**
**Description:**  
Draws a branching path warning.

**Realm:**  
Client

---

### **ShouldDrawBlur**
**Description:**  
Determines whether the blur effect should be drawn.

**Realm:**  
Client

**Returns:**  
- `bool`

---

### **DrawBlur**
**Description:**  
Draws the blur effect.

**Realm:**  
Client

---

### **ShouldDrawPlayerInfo**
**Description:**  
Determines whether player information should be drawn.

**Realm:**  
Client

**Returns:**  
- `bool`

---

### **TooltipInitialize**
**Description:**  
Initializes the tooltip before it is displayed.

**Realm:**  
Client

**Parameters:**
- `panel` (tooltip panel)
- `targetPanel` (the panel for which the tooltip is displayed)

---

### **TooltipPaint**
**Description:**  
Handles painting of the tooltip.

**Realm:**  
Client

**Parameters:**
- `panel`
- `w`, `h` (width, height)

---

### **TooltipLayout**
**Description:**  
Handles layout of the tooltip.

**Realm:**  
Client

**Parameters:**
- `panel`

---

### **AdjustBlurAmount**
**Description:**  
Adjusts the amount of blur applied to the screen.

**Realm:**  
Client

**Parameters:**
- `blurGoal` (`int`)

**Returns:**  
- `number`: The adjusted blur amount.

---

### **SetupQuickMenu**
**Description:**  
Sets up the quick menu by adding buttons, sliders, etc. Called during the panel’s initialization.

**Realm:**  
Client

**Parameters:**
- `panel` (the quick menu panel)

---

### **DrawLiliaModelView**
**Description:**  
Called to draw additional content within a model view panel.

**Realm:**  
Client

**Parameters:**
- `panel`
- `entity`

---

### **ShouldShowPlayerOnScoreboard**
**Description:**  
Determines if a player should be shown on the scoreboard.

**Realm:**  
Client

**Parameters:**
- `client` (`Player`)

**Returns:**  
- `bool`

---

### **ShowPlayerOptions**
**Description:**  
Provides options for the player context menu on the scoreboard.

**Realm:**  
Client

**Parameters:**
- `client` (`Player`)
- `options` (table)  
  Each option is a table `{icon, callback}`.

---

### **ShouldAllowScoreboardOverride**
**Description:**  
Determines whether a scoreboard value should be overridden (e.g., name, desc).

**Realm:**  
Client

**Parameters:**
- `client` (`Player`)
- `var` (`string`)

**Returns:**  
- `bool` (true if override)

---

### **CanPlayerAccessDoor**
**Description:**  
Called when a player tries to use abilities on the door (locking, etc.).

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `door` (`Entity`)
- `access` (`int`)

**Returns:**  
- `bool`

---

### **OnPlayerPurchaseDoor**
**Description:**  
Called when a player purchases or sells a door.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `entity` (`Entity`): The door
- `buying` (`bool`): True if buying, false if selling
- `CallOnDoorChild` (function)

---

### **CanPlayerUseDoor**
**Description:**  
Determines if a player is allowed to use a door entity.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `entity` (`Entity`)

**Returns:**  
- `bool`

---

### **PlayerUseDoor**
**Description:**  
Called when a player attempts to use a door entity.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `entity` (`Entity`)

**Returns:**  
- `bool|nil`: `false` to disallow, `true` to allow, or `nil` to let other hooks decide.

---

### **KeyLock**
**Description:**  
Called when a player attempts to lock a door.

**Realm:**  
Server

**Parameters:**
- `owner` (`Player`)
- `entity` (`Entity`)
- `time` (`float`)

---

### **KeyUnlock**
**Description:**  
Called when a player attempts to unlock a door.

**Realm:**  
Server

**Parameters:**
- `owner` (`Player`)
- `entity` (`Entity`)
- `time` (`float`)

---

### **ToggleLock**
**Description:**  
Toggles the lock state of a door.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `door` (`Entity`)
- `state` (`bool`): `true`=locked, `false`=unlocked

---

### **callOnDoorChildren**
**Description:**  
Calls a function on all child entities of a door.

**Realm:**  
Server

**Parameters:**
- `entity` (`Entity`)
- `callback` (function)

---

### **copyParentDoor**
**Description:**  
Copies the parent door's properties to a child door.

**Realm:**  
Server

**Parameters:**
- `child` (`Entity`)

---

### **ShouldDeleteSavedItems**
**Description:**  
Determines if saved items should be deleted on server restart.

**Realm:**  
Server

**Returns:**  
- `bool`

---

### **OnSavedItemLoaded**
**Description:**  
Called after saved items are loaded from the database.

**Realm:**  
Server

**Parameters:**
- `loadedItems` (table): Contains loaded item entities.

---

### **thirdPersonToggled**
**Description:**  
Called when third-person mode is toggled.

**Realm:**  
Client

**Parameters:**
- `state` (`bool`): `true` if enabled, `false` if disabled

---

### **ShouldDisableThirdperson**
**Description:**  
Checks if third-person view is allowed or disabled.

**Realm:**  
Client

**Parameters:**
- `client` (`Player`)

**Returns:**  
- `bool` (true if 3rd-person should be disabled)

---

### **OnCharAttribBoosted**
**Description:**  
Called when a character’s attribute is updated.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `character` (Character)
- `key` (`string`)
- `value` (`int`)

---

### **PlayerStaminaGained**
**Description:**  
Called when a player gains stamina.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

---

### **PlayerStaminaLost**
**Description:**  
Called when a player loses stamina.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

---

### **CanPlayerThrowPunch**
**Description:**  
Determines if a player can throw a punch.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)

**Returns:**  
- `bool`

---

### **AdjustStaminaOffsetRunning**
**Description:**  
Adjusts stamina offset when a player is running.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `offset` (`float`)

**Returns:**  
- `number`: The modified offset

---

### **AdjustStaminaRegeneration**
**Description:**  
Adjusts the rate at which a player regenerates stamina.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `offset` (`float`)

**Returns:**  
- `number`

---

### **AdjustStaminaOffset**
**Description:**  
Adjusts the stamina offset otherwise (generic hook).

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `offset` (`float`)

**Returns:**  
- `number`

---

### **CalcStaminaChange**
**Description:**  
Calculates the change in a player’s stamina (positive or negative).

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)

**Returns:**  
- `number` (the stamina change)

---

### **CanPlayerViewAttributes**
**Description:**  
Checks if a player is allowed to view their attributes.

**Realm:**  
Client

**Parameters:**
- `client` (`Player`)

**Returns:**  
- `bool`

---

### **GetStartAttribPoints**
**Description:**  
Retrieves the initial number of attribute points a player starts with.

**Realm:**  
Client

**Parameters:**
- `client` (`Player`)
- `context` (table)

**Returns:**  
- `number`: The starting attribute points

---

### **PlayerCanPickupItem (Attributes)**
**Description:**  
Determines if a player can pick up an item with their hands.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `item` (Item)

**Returns:**  
- `bool`

---

### **CharMaxStamina**
**Description:**  
Determines the maximum stamina for a character.

**Realm:**  
Shared

**Parameters:**
- `character` (Character)

**Returns:**  
- `number`: The maximum stamina

---

### **CanPlayerViewInventory (Inventory)**
*(Separate usage context from F1 menu.)*  
Determines whether a player can view their inventory.

**Realm:**  
Client

**Returns:**  
- `bool`

---

### **PostDrawInventory (Inventory)**
**Description:**  
Called after the player's inventory is drawn.

**Realm:**  
Client

**Parameters:**
- `panel` (inventory panel)

---

### **InterceptClickItemIcon**
**Description:**  
Called when a player clicks on an item icon.

**Realm:**  
Client

**Parameters:**
- `self` (panel)
- `itemIcon` (panel)
- `keyCode` (int)

---

### **OnRequestItemTransfer**
**Description:**  
Called when an item transfer is requested (client side).

**Realm:**  
Client

**Parameters:**
- `self` (panel)
- `itemID` (int)
- `inventoryID` (int)
- `x` (int)
- `y` (int)

---

### **ItemPaintOver**
**Description:**  
Called when an item is being painted over in the inventory.

**Realm:**  
Client

**Parameters:**
- `self` (panel)
- `itemTable` (table)
- `w` (int)
- `h` (int)

---

### **OnCreateItemInteractionMenu**
**Description:**  
Called when an item interaction menu is created (e.g. right-click menu).

**Realm:**  
Client

**Parameters:**
- `self` (panel)
- `menu` (panel)
- `itemTable` (table)

---

### **CanRunItemAction**
**Description:**  
Determines if a specific action can be run on an item.

**Realm:**  
Client

**Parameters:**
- `itemTable` (table)
- `action` (string)

**Returns:**  
- `bool`

---

### **CanItemBeTransfered**
**Description:**  
Determines whether an item can be transferred between inventories.

**Realm:**  
Shared

**Parameters:**
- `item` (Item)
- `currentInv` (Inventory)
- `oldInv` (Inventory)

**Returns:**  
- `bool|string`: `true` to allow, or `false`/string for disallowed & reason.

---

### **ItemDraggedOutOfInventory**
**Description:**  
Called when an item is dragged out of an inventory.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `item` (Item)

---

### **ItemTransfered**
**Description:**  
Called when an item is transferred between inventories.

**Realm:**  
Server

**Parameters:**
- `context` (table)

---

### **OnPlayerLostStackItem**
**Description:**  
Called when a player drops a stackable item.

**Realm:**  
Shared

**Parameters:**
- `itemTypeOrItem`

---

### **CanPlayerSpawnStorage**
**Description:**  
Whether a player is allowed to spawn a container entity.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `entity` (`Entity`)
- `data` (table)

**Returns:**  
- `bool`

---

### **isSuitableForTrunk**
**Description:**  
Determines whether an entity is suitable for use as storage (e.g. a trunk).

**Realm:**  
Shared

**Parameters:**
- `entity` (`Entity`)

**Returns:**  
- `bool`

---

### **OnCreateStoragePanel**
**Description:**  
Called when a storage panel is created (client side).

**Realm:**  
Client

**Parameters:**
- `localInvPanel` (panel)
- `storageInvPanel` (panel)
- `storage` (entity)

---

### **CanSaveData**
**Description:**  
Determines whether data associated with a storage entity should be saved.

**Realm:**  
Server

**Parameters:**
- `entity` (`Entity`)
- `inventory` (Inventory)

**Returns:**  
- `bool`

---

### **StorageRestored**
**Description:**  
Called when a storage entity is restored.

**Realm:**  
Server

**Parameters:**
- `storage` (`Entity`)
- `inventory` (Inventory)

---

### **StorageOpen**
**Description:**  
Called when a storage is opened (car trunk or other).

**Realm:**  
Shared

**Parameters:**
- `entity` (`Entity`)
- `isCar` (`bool`)

---

### **StorageUnlockPrompt**
**Description:**  
Called when a prompt to unlock storage is displayed (client side).

**Realm:**  
Client

**Parameters:**
- `entity` (`Entity`)

---

### **StorageCanTransferItem**
**Description:**  
Determines whether a player can transfer an item into storage.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `storage` (`Entity`)
- `item` (Item)

**Returns:**  
- `bool`

---

### **StorageEntityRemoved**
**Description:**  
Called when a storage entity is removed.

**Realm:**  
Server

**Parameters:**
- `entity` (`Entity`)
- `inventory` (Inventory)

---

### **StorageInventorySet**
**Description:**  
Called when the inventory of a storage entity is set.

**Realm:**  
Shared

**Parameters:**
- `entity` (`Entity`)
- `inventory` (Inventory)
- `isInitial` (`bool`)

---

### **isCharRecognized**
**Description:**  
Checks if a character is recognized.

**Realm:**  
Shared

**Parameters:**
- `character` (Character)
- `id` (int)

**Returns:**  
- `bool`

---

### **isCharFakeRecognized**
**Description:**  
Checks if a character is *fake* recognized.

**Realm:**  
Shared

**Parameters:**
- `character` (Character)
- `id` (int)

**Returns:**  
- `bool`

---

### **isFakeNameExistant**
**Description:**  
Checks if a fake name exists in a given name list.

**Realm:**  
Shared

**Parameters:**
- `name` (string)
- `nameList` (table)

**Returns:**  
- `bool`

---

### **OnCharRecognized**
**Description:**  
Called when a character is recognized.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `id` (int)

---

### **CharRecognize**
**Description:**  
Initiates a character recognition process (client side).

**Realm:**  
Client

**Parameters:**
- `level` (int)
- `name` (string)

---

### **GetDisplayedDescription**
**Description:**  
Retrieves the displayed description of an entity (HUD or otherwise).

**Realm:**  
Shared

**Parameters:**
- `entity` (`Entity`)
- `isHUD` (`bool`)

**Returns:**  
- `string`

---

### **GetDisplayedName (Recognition)**
*(Separate from the chat-based version.)*  
**Description:**  
Retrieves a displayed name for a client/character in a recognition context.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `chatType` (`string`) or other context param

**Returns:**  
- `string`

---

### **isRecognizedChatType**
**Description:**  
Determines if a chat type is recognized by the recognition system.

**Realm:**  
Shared

**Parameters:**
- `chatType` (string)

**Returns:**  
- `bool`

---

### **GetSalaryLimit**
**Description:**  
Retrieves the salary limit for a player.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `faction` (table)
- `class` (table)

**Returns:**  
- `any`: The salary limit

---

### **GetSalaryAmount**
**Description:**  
Retrieves the amount of salary a player should receive.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `faction` (table)
- `class` (table)

**Returns:**  
- `any`: The salary amount

---

### **CanPlayerEarnSalary**
**Description:**  
Determines if a player is allowed to earn salary.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `faction` (table)
- `class` (table)

**Returns:**  
- `bool`

---

### **CreateSalaryTimer**
**Description:**  
Creates a timer to manage player salary.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)

---

### **CanPlayerJoinClass (Alternative Usage)**
*(Already documented, but may appear with different logic. Merged above.)*

### **CharHasFlags**
**Description:**  
Determines if a character has the given flags.

**Realm:**  
Shared

**Parameters:**
- `character` (Character)
- `flags` (string)

**Returns:**  
- `bool`

---

### **CanPlayerUseChar**
**Description:**  
Checks if a player is allowed to use a specific character.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)
- `character` (Character)

**Returns:**  
- `bool`
- `string|nil` (Reason if disallowed)

---

### **CanPlayerCreateChar**
**Description:**  
Whether a player can create a new character.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)

**Returns:**  
- `bool` (`false` to disallow)
- `string` (Lang phrase or reason)
- `...` (Args for the phrase)

---

### **PostCharDelete**
**Description:**  
Called after a character is deleted.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `character` (Character)

---

### **GetMaxPlayerChar**
**Description:**  
Retrieves the max number of characters a player can have.

**Realm:**  
Shared

**Parameters:**
- `client` (`Player`)

**Returns:**  
- `int`

---

### **CharDeleted**
*(Synonym to OnCharDelete or PostCharDelete in some code.)*  
**Description:**  
Called after a character is deleted.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `character` (Character)

---

### **PrePlayerLoadedChar**
**Description:**  
Called before a player's character is loaded.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `character` (Character)
- `currentChar` (Character)

---

### **PlayerLoadedChar**
**Description:**  
Called when a player's character is loaded.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `character` (Character)
- `currentChar` (Character)

---

### **PostPlayerLoadedChar**
**Description:**  
Called after a player's character is loaded.

**Realm:**  
Server

**Parameters:**
- `client` (`Player`)
- `character` (Character)
- `currentChar` (Character)

---

### **CharLoaded**
**Description:**  
Called after a character has been successfully loaded (sometimes shared realm usage).

**Realm:**  
Shared or Server

**Parameters:**
- `character` (Character)

---

### **CharPostSave**
**Description:**  
Called after a character is saved.

**Realm:**  
Server

**Parameters:**
- `character` (Character)

---

### **CharPreSave**
**Description:**  
Called before a character is saved.

**Realm:**  
Shared (or Server, depending on code)

**Parameters:**
- `character` (Character)

---