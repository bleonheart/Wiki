---

## **Example**

```lua
MODULE.name = "A Module"
MODULE.author = "76561198312513285"
MODULE.discord = "@liliaplayer"
MODULE.version = "Stock"
MODULE.desc = "This is an Example Module."
MODULE.WorkshopContent = {"2959728255"}
MODULE.enabled = true
MODULE.CAMIPrivileges = {
    {
        Name = "Staff Permissions - Kekw",
        MinAccess = "superadmin",
        Description = "Allows access to kewking.",
    },
}
MODULE.Dependencies = {
    {
        File = MODULE.path .. "/nicebogusfile.lua",
        Realm = "server",
    },
    {
        File = MODULE.path .. "/badbogusfile.lua",
        Realm = "client",
    },
}
```

---

## **Key Variables Explained**

| **Variable**                      | **Purpose**                                                            | **Type**                         | **Example**                                                                                  |
|----------------------------------|------------------------------------------------------------------------|----------------------------------|----------------------------------------------------------------------------------------------|
| `MODULE.name`                     | Identifies the module by name.                                         | `string`                         | `MODULE.name = "A Module"`<br>*Names the module “A Module.”*                                 |
| `MODULE.author`                   | Specifies the module’s author (e.g., STEAMID64 or a name).             | `string`                         | `MODULE.author = "76561198312513285"`<br>*Sets the author’s SteamID64.*                      |
| `MODULE.discord` *(Optional)*     | Provides the Discord handle of the author or support channel.          | `string`                         | `MODULE.discord = "@liliaplayer"`<br>*Displays the author’s Discord tag.*                    |
| `MODULE.version` *(Optional)*     | Tracks the module’s version or release state.                          | `string`                         | `MODULE.version = "Stock"`<br>*Marks the module’s version as “Stock.”*                       |
| `MODULE.desc`                     | Describes the module’s functionality or purpose.                       | `string`                         | `MODULE.desc = "This is an Example Module."`<br>*Gives a short overview of the module.*      |
| `MODULE.identifier` *(Optional)*  | A unique identifier for external references to this module.            | `string`                         | `MODULE.identifier = "example_mod"`<br>*Helps in referencing the module externally.*         |
| `MODULE.CAMIPrivileges` *(Optional)* | Defines CAMI permissions for the module.                             | `table`                          | See [CAMIPrivileges Details](#7-modulecamiprivileges-optional).                                      |
| `MODULE.WorkshopContent` *(Optional)* | Lists Workshop add-on IDs required by the module.                   | `table` of `string`              | `MODULE.WorkshopContent = {"2959728255"}`<br>*Includes relevant Workshop items.*             |
| `MODULE.enabled` *(Optional)*     | Toggles module activation.                                             | `bool`                           | `MODULE.enabled = true`<br>*Enables the module.*                                             |
| `MODULE.Dependencies` *(Optional)*| Specifies files and realms this module depends on.                     | `table` of `table`               | See [Dependencies Details](#10-moduledependencies-optional).                                           |

---

## **Detailed Descriptions**

### 1. `MODULE.name`
- **Purpose:**  
  Names the module and is used for identification in logs or load orders.

- **Type:**  
  `string`

- **Example:**
  ```lua
  MODULE.name = "A Module"
  ```
  *Sets the module’s name to “A Module.”*

---

### 2. `MODULE.author`
- **Purpose:**  
  Specifies the module’s author (e.g., STEAMID64 or a name).

- **Type:**  
  `string`

- **Example:**
  ```lua
  MODULE.author = "76561198312513285"
  ```
  *Sets the author’s SteamID64.*

---

### 3. `MODULE.discord` *(Optional)*
- **Purpose:**  
  Provides a Discord handle or server invite for support or communication.

- **Type:**  
  `string`

- **Example:**
  ```lua
  MODULE.discord = "@liliaplayer"
  ```
  *Displays the author’s or module support’s Discord tag.*

---

### 4. `MODULE.version` *(Optional)*
- **Purpose:**  
  Tracks the module’s version or release state.

- **Type:**  
  `string`

- **Example:**
  ```lua
  MODULE.version = "Stock"
  ```
  *Sets the module version to “Stock.” Can help in version control.*

---

### 5. `MODULE.desc`
- **Purpose:**  
  Describes the module’s functionality or purpose.

- **Type:**  
  `string`

- **Example:**
  ```lua
  MODULE.desc = "This is an Example Module."
  ```
  *Gives users a quick summary of what the module does.*

---

### 6. `MODULE.identifier` *(Optional)*
- **Purpose:**  
  A unique identifier for external references to this module.

- **Type:**  
  `string`

- **Example:**
  ```lua
  MODULE.identifier = "example_mod"
  ```
  *Allows other scripts or systems to reference this specific module.*

---

### 7. `MODULE.CAMIPrivileges` *(Optional)*
- **Purpose:**  
  Defines CAMI (Custom Admin Mod Interface) permissions required or provided by the module.  
  Each privilege can have keys like `Name`, `MinAccess`, and `Description`.

- **Type:**  
  `table`

- **Example:**
  ```lua
  MODULE.CAMIPrivileges = {
      {
          Name = "Staff Permissions - Kekw",
          MinAccess = "superadmin",
          Description = "Allows access to kewking.",
      },
  }
  ```
  *Sets up privileges using the CAMI system so admin mods can integrate these permissions.*

---

### 8. `MODULE.WorkshopContent` *(Optional)*
- **Purpose:**  
  Lists Workshop add-on IDs required by the module, enabling automatic downloading if your server or client references them.

- **Type:**  
  `table` of `string`

- **Example:**
  ```lua
  MODULE.WorkshopContent = {"2959728255"}
  ```
  *Ensures users download the specified Workshop add-ons.*

---

### 9. `MODULE.enabled` *(Optional)*
- **Purpose:**  
  Toggles module activation. If `false`, the module may not load or function.

- **Type:**  
  `bool`

- **Example:**
  ```lua
  MODULE.enabled = true
  ```
  *Indicates that the module is currently active.*

---

### 10. `MODULE.Dependencies` *(Optional)*
- **Purpose:**  
  Specifies additional files or libraries needed by this module, along with where (client or server) they should load.

- **Type:**  
  `table` of `table`

- **Example:**
  ```lua
  MODULE.Dependencies = {
      {
          File = MODULE.path .. "/nicebogusfile.lua",
          Realm = "server",
      },
      {
          File = MODULE.path .. "/badbogusfile.lua",
          Realm = "client",
      },
  }
  ```
  *Ensures the server or client includes any files on which this module depends.*

---

## **Automatically Included Files and Folders**

When you place standard named files or folders in your module’s directory, many frameworks will auto-include them by default. Here’s a brief overview:

### **Files**  
1. **`client.lua`**  
   - Runs exclusively on the client side. Typically includes UI logic or client-specific features (HUD, VGUI, etc.).

2. **`server.lua`**  
   - Runs exclusively on the server. Contains server-side logic for gameplay, data handling, or admin commands.

3. **`config.lua`**  
   - Stores configuration variables shared across the module. Can apply to both client and server if included properly.

4. **`commands.lua`**  
   - Contains command definitions (chat or console), often used for administrative actions or gameplay-related commands.

### **Folders**

1. **`config`**  
   - Houses additional configuration files or specialized config scripts. Useful for adjusting settings without modifying core code.

2. **`dependencies`**  
   - Stores external libraries or resources that the module relies on. These might be internal scripts or third-party utilities.

3. **`libs`**  
   - Contains generic utility scripts or libraries used throughout the addon (not tied to a specific feature).

4. **`hooks`**  
   - Organizes scripts that register and manage hooks (events triggered by the game engine or framework).

5. **`libraries`**  
   - Similar to `libs` but often for more extensive or standalone systems. Each “library” can represent a significant subsystem.

6. **`commands`**  
   - Houses scripts defining custom chat or console commands relevant to the addon.

7. **`netcalls`**  
   - Manages network messaging between client and server (using Garry’s Mod’s `net` library).

8. **`meta`**  
   - Contains scripts that modify or extend metatables (e.g., adding functions to player or entity objects).

9. **`derma`**  
   - Responsible for VGUI (Visual GUI) elements. Scripts here typically create panels, menus, or HUD elements.

10. **`pim`**  
    - Focuses on the “Player Interaction Menu” system. Scripts here define actions and menus for player-to-player or player-to-entity interactions.

---