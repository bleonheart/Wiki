---

Contains information about a player's current game state.

Characters are a fundamental object type in Lilia. They are distinct from players, where players are the representation of a person's existence in the server that owns a character, and their character is their currently selected persona. All the characters that a player owns will be loaded into memory once they connect to the server. Characters are saved during a regular interval (`lia.config.CharacterDataSaveInterval`), and during specific events (e.g., when the owning player switches away from one character to another).

They contain all information that is not persistent with the player; names, descriptions, model, currency, etc. For the most part, you'll want to keep all information stored on the character since it will probably be different or change if the player switches to another character. An easy way to do this is to use `lia.char.registerVar` to easily create accessor functions for variables that automatically save to the character object.

---

## **tostring**

**Description**

Provides a human-readable string representation of the character.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **String**: A string in the format `"character[ID]"`, where `ID` is the character's unique identifier.

**Example**

```lua
print(lia.char.loaded[1])
-- Output: "character[1]"
```

---

## **eq**

**Description**

Compares this character with another character for equality based on their unique IDs.

**Realm**

- **shared**

**Parameters**

- **other** (`Character`): The other character to compare against.

**Returns**

- **Boolean**: `true` if both characters have the same ID; otherwise, `false`.

**Example**

```lua
local char1 = lia.char.loaded[1]
local char2 = lia.char.loaded[2]
print(char1 == char2)
-- Output: false
```

---

## **getID**

**Description**

Retrieves the unique database ID of this character.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Integer**: The unique identifier of the character.

**Example**

```lua
local charID = character:getID()
print(charID)
-- Output: 1
```

---

## **getPlayer**

**Description**

Obtains the player object that currently owns this character.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Player|nil**: The player who owns this character, or `nil` if no valid player is found.

**Example**

```lua
local owner = character:getPlayer()
if owner then
    print("Character is owned by:", owner:Nick())
end
```

---

## **hasMoney**

**Description**

Checks whether the character possesses at least a specified amount of in-game currency.

**Realm**

- **shared**

**Parameters**

- **amount** (`float`): The minimum amount of currency to check for. Must be a non-negative number.

**Returns**

- **Boolean**: `true` if the character's current money is equal to or exceeds the specified amount; otherwise, `false`.

**Example**

```lua
local hasEnoughMoney = character:hasMoney(100)
if hasEnoughMoney then
    print("Character has sufficient funds.")
else
    print("Character lacks sufficient funds.")
end
```

---

## **getFlags**

**Description**

Retrieves all flags associated with this character.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **String**: A concatenated string of all flags the character possesses. Each character in the string represents an individual flag.

**Example**

```lua
local flags = character:getFlags()
for i = 1, #flags do
    local flag = flags:sub(i, i)
    print("Flag:", flag)
end
```

---

## **hasFlags**

**Description**

Determines if the character has one or more specified flags.

**Realm**

- **shared**

**Parameters**

- **flags** (`string`): A string containing one or more flags to check.

**Returns**

- **Boolean**: `true` if the character has at least one of the specified flags; otherwise, `false`.

**Example**

```lua
if character:hasFlags("admin") then
    print("Character has admin privileges.")
end
```

---

## **getItemWeapon**

**Description**

Retrieves the currently equipped weapon of the character along with its corresponding inventory item.

**Realm**

- **shared**

**Parameters**

None.

**Returns**

- **Entity|false**: The equipped weapon entity if a weapon is equipped; otherwise, `false`.
- **Item|false**: The corresponding item from the character's inventory if found; otherwise, `false`.

**Example**

```lua
local weapon, item = character:getItemWeapon()
if weapon then
    print("Equipped weapon:", weapon:GetClass())
else
    print("No weapon equipped.")
end
```

---

## **setFlags**

**Description**

Sets the complete set of flags accessible by this character, replacing any existing flags.

**Note:** This method overwrites all existing flags and does not append to them.

**Realm**

- **server**

**Parameters**

- **flags** (`string`): A string containing one or more flags to assign to the character.

**Returns**

None.

**Example**

```lua
character:setFlags("petr")
-- This sets the character's flags to 'p', 'e', 't', 'r'
```

---

## **giveFlags**

**Description**

Adds one or more flags to the character's existing set of accessible flags without removing existing ones.

**Realm**

- **server**

**Parameters**

- **flags** (`string`): A string containing one or more flags to add.

**Returns**

None.

**Example**

```lua
character:giveFlags("pet")
-- Adds 'p', 'e', and 't' flags to the character
```

---

## **takeFlags**

**Description**

Removes one or more flags from the character's set of accessible flags.

**Realm**

- **server**

**Parameters**

- **flags** (`string`): A string containing one or more flags to remove.

**Returns**

None.

**Example**

```lua
-- For a character with "pet" flags
character:takeFlags("p")
-- The character now only has 'e' and 't' flags
```

---

## **save**

**Description**

Persists the character's current state and data to the database.

**Realm**

- **server**

**Parameters**

- **callback** (`function`, optional): An optional callback function to execute after the save operation completes successfully.

**Returns**

None.

**Example**

```lua
character:save(function()
    print("Character saved successfully!")
end)
```

---

## **sync**

**Description**

Synchronizes the character's data with clients, making them aware of the character's current state.

This method handles different synchronization scenarios:
- If `receiver` is `nil`, the character's data is synced to all connected players.
- If `receiver` is the owner of the character, full character data is sent.
- If `receiver` is not the owner, only limited data is sent to prevent unauthorized access.

**Realm**

- **server**
- **@internal**

**Parameters**

- **receiver** (`Player|nil`): The specific player to send the character data to. If `nil`, data is sent to all players.

**Returns**

None.

**Example**

```lua
character:sync()
-- Syncs character data to all players
```

*This function is internal and should only be used if you are absolutely certain about its implications.*

---

## **setup**

**Description**

Configures the character's appearance and synchronizes this information with the owning player. This includes setting the player's model, faction, body groups, and skin. Optionally, it can prevent networking to other clients.

**Realm**

- **server**
- **@internal**

**Parameters**

- **noNetworking** (`bool`, optional): If set to `true`, the character's information will not be synchronized with other players.

**Returns**

None.

**Example**

```lua
character:setup()
-- Sets up the character and syncs data with the owning player
```

*This function is internal and should only be used if you are absolutely certain about its implications.*

---

## **kick**

**Description**

Forces the player to exit their current character and redirects them to the character selection menu. This is typically used when a character is banned or deleted.

**Realm**

- **server**

**Parameters**

None.

**Returns**

None.

**Example**

```lua
character:kick()
```

---

## **ban**

**Description**

Bans the character, preventing it from being used for a specified duration or permanently if no duration is provided. This action also forces the player out of the character.

**Realm**

- **server**

**Parameters**

- **time** (`float`, optional): The duration of the ban in seconds. If omitted or `nil`, the ban is permanent.

**Returns**

None.

**Example**

```lua
character:ban(3600) -- Bans the character for 1 hour
character:ban() -- Permanently bans the character
```

---

## **delete**

**Description**

Removes the character from the database and memory, effectively deleting it permanently.

**Realm**

- **server**

**Parameters**

None.

**Returns**

None.

**Example**

```lua
character:delete()
```

---

## **destroy**

**Description**

Destroys the character instance, removing it from memory and ensuring it is no longer tracked by the server. This does not delete the character from the database.

**Realm**

- **server**

**Parameters**

None.

**Returns**

None.

**Example**

```lua
character:destroy()
```

---

## **giveMoney**

**Description**

Adds or subtracts money from the character's wallet. This function adds money to the wallet and optionally handles overflow by dropping excess money on the ground.

**Realm**

- **server**

**Parameters**

- **amount** (`float`): The amount of money to add or subtract.

**Returns**

- **Boolean**: Always returns `true` to indicate the operation was processed.

**Example**

```lua
character:giveMoney(500) -- Adds 500 to the character's wallet
```

---

## **takeMoney**

**Description**

Specifically removes money from the character's wallet. This function ensures that only positive values are used to subtract from the wallet.

**Realm**

- **server**

**Parameters**

- **amount** (`float`): The amount of money to remove. Must be a positive number.

**Returns**

- **Boolean**: Always returns `true` to indicate the operation was processed.

**Example**

```lua
character:takeMoney(100) -- Removes 100 from the character's wallet
```

---