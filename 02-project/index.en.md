# Part 2 - Unity Project File Organization

**Author:** Valeria Pudova (hww)  
**Telegram:** [@core_systems_eng](https://t.me/core_systems_eng)  
**Email:** [valery.hww@gmail.com](mailto:valery.hww@gmail.com)  
**Publication Date:** October 5, 2022

![Title](images/title.png)

---

## Abstract

To streamline the workflow and facilitate rapid onboarding of new employees, a comprehensive set of guidelines for file organization within the project is essential. This document proposes a general project structure and provides recommendations for creating game software modules.

This document is part of a series of articles on structuring game projects.

**The first article in the series** — ["File Naming in Unity 3D"](https://docs.google.com/document/d/e/2PACX-1vS3qR_vg_-AnNhv7eWRRUbA8_4-ssVMO2dpC5t2GLzU101CbTGHAxcUVCsRmgXSkyizKkovPKNUnAHx/pub) — covers file naming techniques.

---

## Tags

`#unity` `#unity3d` `#namingconvention` `#gamedev` `#gamedeveloper` `#gamedevelopment` `#indiedev` `#indiegaming` `#indiegamedev`

---

## Revision History

| Date | Version | Description |
|------|---------|-------------|
| 06/10/2022 | R01 | First published version |

---

## Table of Contents

- [Part 2 - Unity Project File Organization](#part-2---unity-project-file-organization)
  - [Abstract](#abstract)
  - [Tags](#tags)
  - [Revision History](#revision-history)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Project Root Folder](#project-root-folder)
    - [Unity Special Folders](#unity-special-folders)
    - [Unity Hidden Assets](#unity-hidden-assets)
  - [File Aspects](#file-aspects)
    - [File Content (Gist)](#file-content-gist)
    - [Content Groups (Gist Group)](#content-groups-gist-group)
    - [File Type (Type)](#file-type-type)
    - [File Type Groups (Team)](#file-type-groups-team)
    - [File Origin (Origin)](#file-origin-origin)
    - [Other Aspects](#other-aspects)
  - [Folder Naming by File Aspects](#folder-naming-by-file-aspects)
  - [Hierarchy Depth Optimization](#hierarchy-depth-optimization)
  - [Project Folder Naming in the Root Directory](#project-folder-naming-in-the-root-directory)
  - [Second-Level Directories](#second-level-directories)
    - [Internal](#internal)
    - [External](#external)
  - [Project Special Folders](#project-special-folders)
  - [Contents of Code and Tools Folders](#contents-of-code-and-tools-folders)
    - [Modules](#modules)
    - [Module Examples](#module-examples)
    - [Module Structure](#module-structure)
      - [1. IGM Module](#1-igm-module)
      - [2. UPM Module](#2-upm-module)
  - [Conclusions](#conclusions)
  - [References](#references)

---

## Introduction

The primary goal of thoughtful project structuring is to streamline the workflow and reduce the onboarding time for new employees. Additionally, it is necessary to consider the game studio's workflow, minimize mutual conflicts where possible, and ensure the project can be split into multiple repositories.

The purpose of this research is to propose project folder naming patterns for at least two levels of hierarchy from the root directory. Therefore, the contents of the root folder must be defined first.

---

## Project Root Folder

The root folder containing the game project is also used for Unity's special folders [5]. Additionally, it is used for folders of some third-party packages. All of this results in content within the root folder that is not subject to structuring. Therefore, the best strategy is **not to create too many elements** in the project's root folder.

### Unity Special Folders

These folders have names that Unity interprets in specific ways, so their contents must be handled accordingly. For example, to ensure editor tool source code works correctly, it must be placed in a folder named `Editor`.

There are three main types of special folders:

1. **Absolute** — The folder must be unique and located in the project's root directory. A subfolder with the module name should be created inside it for the files it contains. This type represents a Unity constraint.
2. **Relative** — The folder can be placed anywhere in the directory tree. Files can be placed directly inside it, but it is preferable not to use this folder in the root directory.
3. **Relative Merged** — The folder can be placed anywhere in the tree, but its contents will be concatenated (merged) in the final build. A subfolder with the module name should be created inside it. It is also preferable not to use this folder in the root directory.

Below is the complete list of special folder names used by Unity.

| Folder Name | Type | Description |
|-------------|------|-------------|
| `Assets` | — | The project's root folder. Contains all assets that can be used by the Unity project. |
| `Editor` | Relative | Folder for editor extensions. Can be placed anywhere in the structure. |
| `Editor Default Resources` | Absolute | Editor resources loaded on demand via the `EditorGUIUtility.Load` method. Located in the root directory. |
| `Gizmos` | Absolute | Custom editor gizmos. Located in the project's root directory. |
| `Plugins` | Absolute | Many third-party Unity packages install into this folder. Its contents compile before other files, which can be utilized for modular project decomposition [6]. |
| `Resources` | Relative Merged | Files located in this folder are accessible for runtime loading. The ability to place this folder anywhere in the project is a valuable feature. |
| `Standard Assets` / `Pro Standard Assets` | Absolute | Used for certain Unity packages. |
| `StreamingAssets` | Absolute | Used for files loaded or saved at runtime. Located in the root directory. |

### Unity Hidden Assets

The Unity Editor completely ignores files and folders that:

- Are named `cvs`;
- Begin with a `.` (dot);
- End with a `~` (tilde);
- Have the `.tmp` extension.

---

## File Aspects

There are numerous recommendations and conventions for project structuring available online [1, 2, 3, 4]. While all of this information is valuable, it is better to analyze your own project and structure it independently. The best starting point is to classify files by their **aspects**.

Any project file can have multiple aspects and, theoretically, could be placed in various locations. Files can be grouped into folders by one of their aspects. Below are some possible aspects.

### File Content (Gist)

What the object inside the file actually represents: `TinyHouse`, `LargeTower`.

### Content Groups (Gist Group)

The group to which the file's object belongs: `Maps`, `Characters`, `Vehicles`, `Weapons`, `Effects`, `Terrain`, `Background`, `Props`, `Vegetation`, `Details`.

In some studios, different teams work on these groups, allowing for clear division of responsibilities and minimizing conflicts.

### File Type (Type)

By file type — without interpreting the content: `Models`, `Textures`, `Materials`, `Particles`, `Prefabs`, `Scenes`.

The file type often reveals nothing about its content (for example, a prefab can contain anything). Moreover, there are too many different types. Therefore, naming folders by type is not advisable at higher levels. If such names are used, they should be at the very bottom of the file tree.

### File Type Groups (Team)

Without assessing content, some file types can be grouped into larger categories, often managed by separate project teams. Examples include:

- `Art` — visual content from the art department.
- `Design` — prefabs and assets created by designers.
- `Audio` — audio files, prefabs, mixer settings.
- `Code` — game source code files.
- `Tools` — source code files for artist tools.

Group names should not use type names. For instance, the `Audio` group is named as such because it may contain not only audio files but also mixer settings, prefabs, and scene objects.

If these folders need to be grouped together, a prefix can be added: `GameArt`, `GameDesign`, `GameAudio`, `GameCode`.

### File Origin (Origin)

Files in a large project can have different origins, but most commonly they are either created internally or acquired externally — i.e., `Internal` or `External`.

External modules or assets should not be heavily modified so they remain easily updatable to newer versions. If the studio makes significant changes to external files, one of two approaches is required:

1. Propose the changes to the author and keep the module external;
2. Restructure the module as internal.

### Other Aspects

There are many others. For example, the procedural generation aspect allows grouping such files in a `Procedural` folder, with their source templates in `Templates`.

---

## Folder Naming by File Aspects

Organizing files by the `Team` aspect positively impacts studio workflow. The project can be structured so that changes to files and folder structures are made only by their creators. Unity provides several tools for this: additively loaded scenes, hierarchical prefabs, prefab variants, and others.

![Dependencies between and within departments](images/fig1.png)  
*Figure 1 — Dependencies between and within departments*

Based on the proposed dependency model, the following assertions hold true:

1. **Tools Programmers** — work exclusively with their own content.
2. **Game Programmers** — work with their own content and Tools content.
3. **Art Department** — work with their own content and Tools content.
4. **Game Designers** — require the largest volume of files:
   - During **pre-production**, Game Code and Tools are needed.
   - During **production**, all project files are required.

It is crucial to separate tool code (`ToolsCode`) from game code (`GameCode`) so that the former does not depend on the latter. This minimizes the need for changes in the Art department as game code evolves.

Given the high importance of the `Team` and `Origin` aspects, the comprehensive folder naming pattern can be expressed as:

```
(1) {Team}/{Origin}/{GistGroup}/{Gist}/{Type}/
```

**Examples:**

```text
/Art/External/Explosions/GrenadeExplosions/Prefabs/SmallGrenadeExplosion
/Art/External/Explosions/GrenadeExplosions/Textures/BlackSmoke.tga
```

---

## Hierarchy Depth Optimization

Deep nesting should be avoided. The human brain easily navigates a file system with no more than three hierarchy levels. The memorable signature of a file's location consists of the first three path elements, and designers should be able to find the desired element at this level.

However, the fewer folders there are in the root, the deeper the hierarchy can be. If there is only one top-level folder, it can be mentally ignored — just as we ignore the `Assets` folder.

To minimize hierarchy depth, a truncated version of the previous pattern can be used:

```
(2) {Team}/{Origin}/{GistGroup}/{Gist|Type}/
```

**Examples using Gist:**

```text
/Art/Internal/Architecture/LargeTower/LargeTower.mb
/Art/Internal/Architecture/LargeTower/LargeTower.tga
/Art/Internal/Architecture/TinyHouse/TinyHouse.mb
/Art/Internal/Architecture/TinyHouse/TinyHouse.tga
```

**Examples using Type:**

```text
/Art/Internal/LensFlares/Textures/LensFlares_01.tga
/Art/Internal/LensFlares/Models/LensFlares_01.mb
```

For game designers, the `Origin` aspect is often redundant, as all design files undergo adaptation and internal refactoring. Therefore, the pattern for them can be shortened to:

```
(3) {Team}/{GistGroup}/{Gist|Type}/
```

**Examples:**

```text
/Design/Actors/Cyborg/RedCyborg.prefab
/Design/Actors/Cyborg/GreenCyborg.prefab
```

---

## Project Folder Naming in the Root Directory

As mentioned earlier, the Unity project root folder is used for both special folders and some third-party packages. Therefore, the best strategy is:

- **Do not create too many folders** in the project's root directory. This means using the most generalizing aspect for folder contents.
- If you want your project folders to be grouped, add a project or company abbreviation as a prefix.

Given that departments are best separated at higher hierarchy levels, the root folder can contain folders with the most generalized department names:

- `Art` — for artists.
- `Design` — for game designers.
- `Audio` — for sound engineers.
- `Code` — for game programmers.
- `Tools` — for tools programmers.

---

## Second-Level Directories

At the second level, departments can be given freedom to structure as they see fit. However, specifying at least some rules simplifies cross-department interaction. The second most important aspect in game development is `Origin`.

### Internal

Folder for packages developed within the studio. Each package is structured according to the conventions described in the [Module Structure](#module-structure) section. A package can be used in the project as a git submodule.

### External

External plugins such as OpenVR, HTC, Steam, Photon, Emerald, etc. These packages are stored exactly as acquired. As soon as the studio begins to heavily modify a package's contents, it should be moved to `Internal`.

The `External` folder should not contain too many child elements. Its contents are organized using the following pattern:

```
(4) External/{Vendor|Category}/{PackageName}
```

Inside `External`, one of the following top-level folder types is used:

- `Vendor` — The package manufacturer's name.
- `Category` — The package category (according to Asset Store or OpenUPM classification).

Below are the most popular Asset Store categories:

`VisualScripting`, `Terrain`, `Animation`, `Utilities`, `AI`, `ParticlesAndEffects`, `Others`

---

## Project Special Folders

Intended for special cases and can be used anywhere in the hierarchy to improve the understandability of the project structure.

| Folder | Purpose |
|--------|---------|
| `Scenes` | Scenes required for a module. This is a Type aspect. |
| `Maps` | Final game location scenes. Usually distributed across department folders. Example: `/Design/Maps/FishingVillage.unity` |
| `Examples` | Files needed exclusively for demonstration purposes. Can be ignored in CI/CD builds. |
| `Ignore` | Ignored by VCS (via `.gitignore` rules) but visible to the Unity Editor. Any employee can create it locally as a personal sandbox. |
| `Experimental` | R&D files. Modules in this folder follow the pattern `Experimental/{ModuleName}.{SubmoduleName}`. After research, files are restructured and moved to `Internal` or deleted. |
| `Procedural` | Files generated by procedural generation tools. Should be protected from manual editing as they can be regenerated at any time. |
| `Templates` | Source files for procedural generators, if any. |
| `Sources` | Source files for Content Creation Tools (CCD), such as `.max`, `.blend`, `.psd`. These are better stored outside the game project. If stored inside, place them in the `Sources` folder so that CI/CD can ignore them. |

---

## Contents of Code and Tools Folders

These folders are intended primarily for programmers, and their content is organized into modules. Within a module, there may be other resources: fonts, textures, scenes — everything necessary for the module's functionality and its test scenes.

The base subdirectories are:

- `Internal` — internal modules.
- `External` — third-party modules.
- `Scenes` — scenes intended for programmers.

### Modules

The in-game architecture must be divided into modules, where a module is a specific functional piece of the game or a framework, organized as a separate folder. The same rules that apply to the project's root folder apply within such a module.

This structure allows a module to be converted into a separate UPM package if needed, with updates delivered via CI/CD.

The module folder naming pattern is:

```
(6) /Code/Internal/{ModuleName}.{SubmoduleName}/
```

### Module Examples

Below are examples of two modules: `Game` and `AI`. The first has one submodule, while the second has three.

```text
Code/                              # Root folder for programmers
├── External/                      # Third-party modules
└── Internal/                      # Proprietary modules
    ├── Game/                      # Main game module
    ├── Game.Audio/                # Audio submodule
    ├── AI/                        # Base AI module
    ├── AI.Crowd/                  # Crowd simulation submodule
    ├── AI.Melee/                  # Melee combat submodule
    └── AI.PathFinding/            # Pathfinding submodule
```

### Module Structure

There are two types of modules, distinguished by their purpose: **In Game Modules (IGM)** and **Unity Package Manager (UPM)** modules.

#### 1. IGM Module

Used for both game logic and Unity tools. Example structure:

```text
%ModuleName%/
├── README.md                      # Module documentation
├── Scripts/                       # Module source code (C#)
├── Editor/                        # Editor extension code
├── Prefabs/                       # Module prefabs
├── Art/                           # Art content used by the module
├── Audio/                         # Audio content used by the module
├── Scenes/                        # Module test scenes
├── Examples/                      # Usage examples
│   └── Example_01/                # Isolated example case
└── Resources/
    └── %ModuleName%/              # Module resources for Resources.Load()
```

**Important:** Always create a subfolder with the module name inside the `Resources` folder to avoid naming conflicts during builds.

#### 2. UPM Module

To convert an IGM module to a UPM module, the following steps are required:

1. Add a `package.json` file with project metadata, version, and dependencies.
2. Rename `Examples` to `Examples~` — this hides the folder from automatic import, allowing developers to load examples optionally via Package Manager.
3. Add a `LICENSE` file (optional but recommended).
4. Add a `CHANGELOG.md` file for CI/CD automation.

---

## Conclusions

A game studio must continuously develop its project organization system, file and folder naming conventions — taking into account the workflow and team requirements.

Organizing a game project and bringing it to a coherent, understandable system is a complex task driven by necessity: the larger the team and the project, the more complex the work becomes, and arbitrary naming and chaotic file structuring multiply this complexity.

There are many solutions to this problem — recommendations for organization and proper, consistent, intuitive structures. Based on these diverse sources and personal experience, this document formulates a project structuring approach and invites you to try applying it in practice.

The next article in the series will separately address specific issues of project organization by various studio departments.

---

## References

- [1] [50 Tips and Best Practices for Unity](http://www.gamasutra.com/blogs/HermanTulleken/20160812/279100/50_Tips_and_Best_Practices_for_Unity_2016_Edition.php) — Herman Tulleken, 2016.
- [2] [Unreal Engine Assets Naming Convention](https://wiki.unrealengine.com/Assets_Naming_Convention)
- [3] [Unity Project Style Guide](https://github.com/timdhoffmann/unity-project-style-guide)
- [4] [Unity Best Practices](http://www.glenstevens.ca/unity3d-best-practices/)
- [5] [Unity Manual: Special Folder Names](https://docs.unity.cn/ru/2021.1/Manual/SpecialFolders.html)
- [6] [Unity Manual: Special Folders and Script Compilation Order](https://dev.rbcafe.com/unity/unity-5.3.3/en/Manual/ScriptCompileOrderFolders.html)
- [7] Gregory J. *Game Engine Architecture*. – 3rd Edition, AK Peters/CRC Press, 2018.

