
---
    
### **Example**
    
```lua
ATTRIBUTE.name = "Strength"
ATTRIBUTE.desc = "Strength Skill."
ATTRIBUTE.noStartBonus = false
ATTRIBUTE.maxValue = 50
ATTRIBUTE.startingMax = 15

function ATTRIBUTE:OnSetup(client, value)
    if value > 5 then 
        client:ChatPrint("You are very Strong!")
    end
end
```
    
---
    
### **Key Variables Explained**
    
| **Variable**                                 | **Purpose**                                                                                                     | **Type**   | **Example**                           |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------|------------|---------------------------------------|
| `ATTRIBUTE.name`                             | Specifies the display name of the attribute.                                                                    | `string`   | `ATTRIBUTE.name = "Strength"`         |
| `ATTRIBUTE.desc`                             | Provides a short description of the attribute.                                                                  | `string`   | `ATTRIBUTE.desc = "Strength Skill."`   |
| `ATTRIBUTE.noStartBonus` *(Optional)*        | Determines whether the attribute can receive a bonus at the start of the game.                                 | `bool`     | `ATTRIBUTE.noStartBonus = false`      |
| `ATTRIBUTE.maxValue` *(Optional)*            | Specifies the maximum value the attribute can reach.                                                           | `number`   | `ATTRIBUTE.maxValue = 50`              |
| `ATTRIBUTE.startingMax` *(Optional)*         | Defines the maximum value the attribute can start with.                                                        | `number`   | `ATTRIBUTE.startingMax = 15`           |
| `ATTRIBUTE:OnSetup(client, value)` *(Optional)* | Executes custom logic when the attribute is set up for a player, such as notifications or additional effects. | `function` | `ATTRIBUTE:OnSetup(client, value)`     |
    
---
    
### **Detailed Descriptions**
    
#### 1. `ATTRIBUTE.name`
    
- **Purpose:**  
  Specifies the display name of the attribute.
    
- **Type:**  
  `string`
    
- **Example:**
    ```lua
    ATTRIBUTE.name = "Strength"
    ```
    *Sets the attribute's name to "Strength."*
    
---
    
#### 2. `ATTRIBUTE.desc`
    
- **Purpose:**  
  Provides a short description of the attribute.
    
- **Type:**  
  `string`
    
- **Example:**
    ```lua
    ATTRIBUTE.desc = "Strength Skill."
    ```
    *Sets the description of the attribute to "Strength Skill."*
    
---
    
#### 3. `ATTRIBUTE.noStartBonus` *(Optional)*
    
- **Purpose:**  
  Determines whether the attribute can receive a bonus at the start of the game.
    
- **Type:**  
  `bool`
    
- **Example:**
    ```lua
    ATTRIBUTE.noStartBonus = false
    ```
    *If set to `false`, players can assign a starting bonus for this attribute.*
    
---
    
#### 4. `ATTRIBUTE.maxValue` *(Optional)*
    
- **Purpose:**  
  Specifies the maximum value the attribute can reach.
    
- **Type:**  
  `number`
    
- **Example:**
    ```lua
    ATTRIBUTE.maxValue = 50
    ```
    *Sets the maximum value for this attribute to 50.*
    
---
    
#### 5. `ATTRIBUTE.startingMax` *(Optional)*
    
- **Purpose:**  
  Defines the maximum value the attribute can start with.
    
- **Type:**  
  `number`
    
- **Example:**
    ```lua
    ATTRIBUTE.startingMax = 15
    ```
    *Limits the starting value of the attribute to 15.*
    
---
    
#### 6. `ATTRIBUTE:OnSetup(client, value)` *(Optional)*
    
- **Purpose:**  
  Runs custom logic when the attribute is set up for a player. This can include notifications or additional effects.
    
- **Type:**  
  `function`
    
- **Example:**
    ```lua
    function ATTRIBUTE:OnSetup(client, value)
        if value > 5 then 
            client:ChatPrint("You are very Strong!")
        end
    end
    ```
    *Displays a chat message to the player if their Strength attribute exceeds 5.*
    
---