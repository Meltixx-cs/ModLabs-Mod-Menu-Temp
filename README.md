<div align="center">
ModLabs Menu Template
A clean, customizable Gorilla Tag mod-menu template built for community developers.
![Installs](https://img.shields.io/github/downloads/YOUR_GITHUB_NAME/YOUR_REPO_NAME/total?style=for-the-badge&label=Installs)
![Discord Online](https://img.shields.io/discord/YOUR_DISCORD_SERVER_ID?style=for-the-badge&label=Discord%20Online&logo=discord&logoColor=white)
![Latest Release](https://img.shields.io/github/v/release/YOUR_GITHUB_NAME/YOUR_REPO_NAME?style=for-the-badge&label=Latest)
<br>
<img src="assets/modlabs-menu.png" alt="ModLabs Menu Preview" width="650">
<br>
Made to be easy to edit, easy to understand, and easy to build.
</div>
---
About
ModLabs Menu Template is a community-focused Gorilla Tag menu template designed to make adding categories, mods, colors, branding, and layout changes as simple as possible.
The project keeps most of the stuff a feature developer will want to change inside `Config.cs`, while the actual mod logic stays organized inside the `Mods` folder.
Included
Custom ModLabs menu model
PC and VR menu support
Home, Settings, Disconnect, Previous Page, and Next Page controls
Animated cyan ↔ white title gradient
Custom ModLabs logo next to the title
White menu/button outlines
Configurable colors
Configurable font sizes
Configurable title and developer credits
Dynamic Enabled Mods category
Easy category registration
Easy mod registration
Button enabled/disabled colors
Join/leave notification settings
Movement, Safety, and Visual mod sections
AssetBundle-based custom menu model
---
Requirements
Before building, make sure you have:
Gorilla Tag installed through Steam
BepInEx 5
Visual Studio 2022 with .NET desktop development
The Gorilla Tag managed DLLs available from your game installation
The included `modlabsmenu.bundle`
A typical Gorilla Tag install path is:
```text
C:\Program Files (x86)\Steam\steamapps\common\Gorilla Tag
```
If your game is installed somewhere else, update the project path before building.
---
Installation
1. Download the project
Download the newest release from the Releases page and extract the ZIP into a normal folder.
Do not build directly from inside the ZIP.
Example:
```text
Downloads
└── ModLabs_Menu_Template
    ├── ModLabs.sln
    ├── ModLabs.csproj
    ├── Config.cs
    ├── Mods
    ├── Assets
    └── ...
```
---
2. Check the Gorilla Tag path
Open:
```text
Directory.Build.props
```
Find the Gorilla Tag path and make sure it matches your installation.
Example:
```xml
<GamePath>C:\Program Files (x86)\Steam\steamapps\common\Gorilla Tag</GamePath>
```
If Gorilla Tag is on another drive, change it.
Example:
```xml
<GamePath>D:\SteamLibrary\steamapps\common\Gorilla Tag</GamePath>
```
---
3. Open the solution
Open:
```text
ModLabs.sln
```
in Visual Studio.
Wait for Visual Studio to finish loading the project.
---
4. Build
In Visual Studio:
```text
Build
→ Rebuild Solution
```
The compiled DLL should appear inside something similar to:
```text
bin\Debug\netstandard2.1\ModLabs.dll
```
---
5. Install the DLL
Create a folder like:
```text
Gorilla Tag
└── BepInEx
    └── plugins
        └── ModLabs
```
Copy:
```text
ModLabs.dll
```
into that folder.
Make sure you do not have multiple old copies of `ModLabs.dll` installed at the same time.
---
6. Launch Gorilla Tag
Start Gorilla Tag normally through Steam.
On PC, use the menu key configured by the template.
If the menu does not appear, check:
```text
Gorilla Tag\BepInEx\LogOutput.log
```
for ModLabs errors.
---
Customizing the Menu
Most community developers should start with:
```text
Config.cs
```
That file contains the main template settings.
---
Change the menu title
Find:
```csharp
public const string MenuTitle = "ModLabs Temp";
```
Change it to anything you want:
```csharp
public const string MenuTitle = "My Menu";
```
---
Change the developer credit
Find:
```csharp
public const string DeveloperCredit = "Developed by Meltixx";
```
Example:
```csharp
public const string DeveloperCredit = "Developed by YourName";
```
---
Change the developer credit size
Find:
```csharp
public const float CreditFontSize = 8.0f;
```
A good range is:
```text
6.5  = small
7.5  = medium
8.0  = recommended
9.0  = larger
```
---
Colors
The main menu colors are controlled in `Config.cs`.
Example:
```csharp
public static readonly Color MainColor =
    new Color32(22, 74, 136, 255);

public static readonly Color ButtonColor =
    new Color32(43, 105, 181, 255);

public static readonly Color ButtonEnabledColor =
    new Color32(5, 28, 58, 255);

public static readonly Color OutlineColor =
    Color.white;

public static readonly Color TextColor =
    Color.white;
```
What each setting does
Setting	Controls
`MainColor`	Main menu body
`ButtonColor`	Normal buttons
`ButtonEnabledColor`	Enabled/toggled buttons
`OutlineColor`	Menu and button outlines
`TextColor`	Standard menu text
`PageArrowColor`	Left/right page arrows
`TextOutlineColor`	Text outline color
---
Title Gradient
The title can cycle through multiple colors.
Example:
```csharp
public static readonly Color[] TitleGradientColors =
{
    Color.cyan,
    Color.white
};

public const float TitleGradientSpeed = 0.48f;
```
To make the animation faster:
```csharp
public const float TitleGradientSpeed = 0.8f;
```
To make it slower:
```csharp
public const float TitleGradientSpeed = 0.25f;
```
---
Adding a Category
Categories are registered inside `Config.cs`.
Example:
```csharp
new(
    name: "Fun Mods",
    showOnHome: true,
    buttons: new ButtonInfo[]
    {
        ButtonInfo.Toggle(
            "Example Mod",
            tick: FunMods.ExampleMod,
            tip: "Example description.")
    }),
```
`showOnHome`
```csharp
showOnHome: true
```
means the category appears on the main Home page.
Use:
```csharp
showOnHome: false
```
for hidden/internal categories.
---
Adding a Toggle Mod
For a mod that stays enabled until the user turns it off:
```csharp
ButtonInfo.Toggle(
    "Example Mod",
    tick: ExampleMods.Run,
    onEnable: ExampleMods.Enable,
    onDisable: ExampleMods.Disable,
    tip: "What the mod does."),
```
You do not need all three methods.
For a simple continuously-running mod:
```csharp
ButtonInfo.Toggle(
    "Example Mod",
    tick: ExampleMods.Run),
```
---
Adding a One-Press Action
For a button that runs one time:
```csharp
ButtonInfo.Action(
    "Example Action",
    ExampleMods.DoSomething,
    tip: "Runs once when pressed."),
```
---
Creating the Actual Mod Code
Keep the actual feature logic inside the `Mods` folder.
Example:
```text
Mods
├── Movement.cs
├── Safety.cs
├── Visual.cs
└── FunMods.cs
```
A basic mod class can look like:
```csharp
using UnityEngine;

namespace ModLabs.Mods
{
    public static class FunMods
    {
        public static void ExampleMod()
        {
            // Runs every frame while enabled.
        }

        public static void EnableExample()
        {
            // Runs once when enabled.
        }

        public static void DisableExample()
        {
            // Runs once when disabled.
        }
    }
}
```
---
Enabled Mods
You do not need to manually add mods to the Enabled Mods page.
When a toggle is enabled, ModLabs automatically adds it to:
```text
Enabled Mods
```
Clicking it there disables the mod and removes it from the list.
---
Settings
The Settings page can contain template-level features such as:
Hide Notifications
Join Alerts
Leave Alerts
Menu options
Theme options
Developer options
Add them the same way as any other button in `Config.cs`.
---
Replacing the Menu Model
The menu model is loaded from:
```text
Assets\modlabsmenu.bundle
```
The prefab inside the bundle should be named:
```text
ModLabsMenu
```
Keep the expected object names when making a replacement model so the scripts can find the buttons and text.
Important objects include:
```text
ModLabsMenu
├── Base
├── MenuName
└── Buttons
    ├── Button1
    ├── LeftPageButton
    ├── RightPageButton
    ├── HomeButton
    ├── SettingsButton
    └── DisconnectButton
```
If you change these names, update the corresponding object lookup names in the menu scripts.
---
Building a New AssetBundle
Use the included Unity AssetBundle builder.
Your prefab must be named:
```text
ModLabsMenu
```
Then build the Windows 64-bit bundle.
The finished bundle should be named:
```text
modlabsmenu.bundle
```
Replace the old bundle in the project's `Assets` folder, then rebuild the solution.
---
Troubleshooting
The project does not load
Make sure you extracted the ZIP before opening the `.sln`.
Do not open the solution directly from the compressed ZIP.
---
Missing references
Check that `Directory.Build.props` points to the correct Gorilla Tag folder.
---
Menu does not open
Check:
```text
BepInEx\LogOutput.log
```
Search for:
```text
ModLabs
```
---
The old version still appears
Remove older copies of:
```text
ModLabs.dll
```
from every folder inside:
```text
BepInEx\plugins
```
Then install only the newest version.
---
The model is missing
Confirm that:
```text
Assets\modlabsmenu.bundle
```
exists before building.
---
Text or layout looks wrong
Make sure the correct AssetBundle is included and that the prefab still uses the object names expected by the scripts.
---
For Community Developers
The goal of ModLabs is to keep feature development simple.
For most changes:
```text
Want to change colors?
→ Config.cs

Want to add a category?
→ Config.cs

Want to add a button?
→ Config.cs

Want to make the button actually do something?
→ Mods/*.cs

Want to change the menu model?
→ modlabsmenu.bundle
```
Avoid hardcoding theme values directly into mod files. Put configurable values in `Config.cs` so other developers can easily change them.
---
GitHub Install Counter
The badge at the top uses total GitHub release downloads:
```text
https://img.shields.io/github/downloads/YOUR_GITHUB_NAME/YOUR_REPO_NAME/total
```
Replace:
```text
YOUR_GITHUB_NAME
YOUR_REPO_NAME
```
with your actual GitHub repository information.
For the counter to increase, users should download builds through GitHub Releases.
---
Discord Online Counter
The Discord badge uses your Discord server ID.
Replace:
```text
YOUR_DISCORD_SERVER_ID
```
with your server's numeric ID.
Also replace:
```text
YOUR_DISCORD_INVITE
```
with your Discord invite.
Finding your Discord server ID
Open Discord.
Go to User Settings → Advanced.
Turn on Developer Mode.
Right-click your server.
Select Copy Server ID.
Paste that number into the badge URL.
The badge can then show your server's live Discord member/online status when supported by Discord/Shields.
---
Releases
When publishing a new version:
Build `ModLabs.dll`.
Test it in Gorilla Tag.
Create a new GitHub Release.
Upload the release ZIP/DLL.
Give the release a version such as:
```text
v1.0.0
v1.1.0
v1.2.0
```
Using GitHub Releases is also what allows the install/download counter at the top of this README to work.
---
Credits
<div align="center">
ModLabs Template
Developed by Meltixx
</div>
---
Disclaimer
This is a community-created project and is not affiliated with Another Axiom or Gorilla Tag.
Use mods responsibly and respect the rules of the game, servers, and communities you participate in.
