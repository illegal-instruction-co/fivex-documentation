Take me to [pookie](#pookie)
### <a name="tith"></a>This is the Heading

## **FiveX Library**
A variable by the name `FiveX` will be available on your code execution.
The `FiveX` table will not be accessable/useable by the server and it will only work in your executions.

Here's what it has to offer:
#### Identifiers

| Function | Description  | Arguments |
| -------- | ------------ | --------- |
| ID | Returns string. Your client-id | - |

#### Network

| Function | Description  | Arguments |
| -------- | ------------ | --------- |
| HttpGet ([similar](https://docs.fivem.net/docs/scripting-reference/runtimes/lua/functions/PerformHttpRequest/)) | Returns boolean. If request was successful | string, function |
| HttpPost | Returns boolean. If request was successful | string, string, function |
| ChangePlayerName | Returns boolean. If name has been changed | string |
| SendNetEvent | Returns boolean. If event was sent | string, string |

#### Controls

| Function | Description  | Arguments |
| -------- | ------------ | --------- |
| IsKeyPressed | Returns boolean. If key was pressed | int |
| IsKeyReleased | Returns boolean. If key was pressed and released | int |
| IsKeyHeld | Returns boolean. If key is being held | int |
| SimulateKeyPress | Returns boolean. If key has been simulated | int |

#### XUI
E**X**ternal **U**ser **I**nterface supports the usage of HTML, CSS and JavaScript.
For HTML side check [this](https://raw.githubusercontent.com/illegal-instruction-co/text-xui/main/the.html).
| Function | Description  | Arguments |
| -------- | ------------ | --------- |
| CreateXui | Returns string. XUI handle | string, int, int |
| DestroyXui | Returns boolean. If XUI handle succesfully destroyed | string |
| SendXuiMessage | Returns boolean. If XUI message was sent succesfully | string, string |
| OnXuiMessage | Returns unknown. Listener for XUI | function |

#### Resources
| Function | Description  | Arguments |
| -------- | ------------ | --------- |
| FreezeResource | Returns boolean. If resource has been frozen | string |
| ThaweResource | Returns boolean. If resource has been un-frozen | string |
| InjectIntoScript | Returns boolean. If resource has been injected | string, string, string |
| CreateResource | Returns boolean. If resource has been created | string, string |

#### Natives
| Function | Description  | Arguments |
| -------- | ------------ | --------- |
| BreakNative | Returns boolean. If native has been broken | integer |
| RepairNative | Return boolean. If native has been repaired  | integer |
| HookNative | Returns boolean. If native has been hooked | integer, function |


# **FiveX Library Usage**

#### Identifiers
```lua
local client_id = FiveX.ID()
print(client_id) --[[will print our client id]]
```

#### Network
Change our in-game/display name to `Yeetus`:
```lua
FiveX.ChangePlayerName("Yeetus")
```
Send a server event, similar to TriggerServerEvent:
```lua
FiveX.SendNetEvent("some_event", "some_data")
```
Perform a Http GET request:
```lua
FiveX.HttpGet("https://google.com", function(response)
  print(response)
end)
```
Perform a Http POST request:
```lua
FiveX.HttpPost("https://google.com", json.encode({ somedata = "somedata"}), function(response)
  print(response)
end)
```

#### Controls
Check virtual key codes [here](https://docs.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes).
If key pressed we will use this: `FiveX.IsKeyPressed`
If key released we will use this: `FiveX.IsKeyReleased`
If key held we will use this: `FiveX.IsKeyHeld`

Check if a key was pressed:
```lua
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(0)
      if (FiveX.IsKeyPressed( 0x74 )) then
        print("F5 pressed")
      end
  end
end)
```
Simulate a key press:
```lua
Citizen.CreateThread(function()
  while true do
    Citizen.Wait(0)
    if (FiveX.SimulateKeyPress( 0x74 )) then
      print("F5 has been simulated")
    end
  end
end)
```

#### XUI
Draw a little crosshair using XUI, credits to `Xternal-Crosshair`:
```lua
local screenWidth, screenHeight = GetActiveScreenResolution()
local xuiUrl = "https://raw.githubusercontent.com/illegal-instruction-co/Xternal-Crosshair/main/index.html"
local xuiHandle = FiveX.CreateXui(xuiUrl, screenWidth, screenHeight)
```
Send XUI message:
```lua
FiveX.SendXuiMessage(xuiHandle, "HELLO WORLD!")
```
Listen to XUI message:
```lua
FiveX.OnXuiMessage(function(handle, message)
  print("XUI: "..handle, "MESSAGE: ".. message)
end)
```
Destroy XUI:
```lua
if (FiveX.DestroyXui(xuiHandle)) then
  print("XUI destroyed")
end
```

#### Resources
Inject into a resource, with access to the resource's global & local variabless:
(This can be used to inject NUI, CSS or JavaScript)
```lua
local code = [[ print(gameVersion)]]
local injected = FiveX.InjectIntoScript("pma-voice", "client/init/main.lua", code)
print(injected)
```
Freeze a resource:
```lua
FiveX.FreezeResource("anticheat")
```
Un-freeze a resource:
```lua
FiveX.ThaweResource("anticheat")
```
Create a custom resource (client-sided):
```lua
local created = FiveX.CreateResource("my_new_resource", "C:\my_resource_folder")
```

#### Natives
[FiveM Natives](https://docs.fivem.net/natives/)

Break a native:
```lua
--[[NETWORK_IS_IN_SPECTATOR_MODE]]
FiveX.BreakNative(0x048746E388762E11)
FiveX.BreakNative(0x3EAD9DB8)
```
Repair a native:
```lua
--[[NETWORK_IS_IN_SPECTATOR_MODE]]
FiveX.RepairNative(0x048746E388762E11)
FiveX.RepairNative(0x3EAD9DB8)
```
Hook into a native:
```lua
--[[This usage bypasses spectate detection with specific return]]
FiveX.HookNative(0x048746E388762E11, function(originalResult)
  if not originalResult then
    originalResult = true
  end
  return originalResult
end)
```
# [Star Formation Theory][sft]
