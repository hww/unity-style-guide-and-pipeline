# Part 1 - File Naming in Unity 3D

**Valeria Pudova (hww)**  
💬 Telegram: @core_systems_eng  
✉️ Email: valery.hww@gmail.com  
📅 22.09.2022

![UnityAssetsStructur_SharedFolder](images/title.png)

---

## 📌 Table of Contents

- [Part 1 - File Naming in Unity 3D](#part-1---file-naming-in-unity-3d)
  - [📌 Table of Contents](#-table-of-contents)
  - [Introduction](#introduction)
    - [Two Primary File Naming Styles](#two-primary-file-naming-styles)
      - [1. `PascalCase`](#1-pascalcase)
      - [2. `lower-case`](#2-lower-case)
    - [Style Comparison](#style-comparison)
    - [General Rules](#general-rules)
  - [File Name Prefix](#file-name-prefix)
    - [Type Prefix](#type-prefix)
    - [Target Prefix](#target-prefix)
    - [Sequence Prefix](#sequence-prefix)
    - [Temp Prefix](#temp-prefix)
  - [File Name Suffix](#file-name-suffix)
    - [Target Suffix](#target-suffix)
    - [Version Suffix](#version-suffix)
    - [Sequence Suffix](#sequence-suffix)
    - [Aspect Suffix](#aspect-suffix)
    - [Content Suffix](#content-suffix)
  - [Combining Suffixes](#combining-suffixes)
  - [Unity Reserved Rules](#unity-reserved-rules)
  - [Object Naming Rules](#object-naming-rules)
  - [Conclusions](#conclusions)
  - [References](#references)

---

## Introduction

Source code file naming is thoroughly covered in the official [Microsoft Coding Conventions C#](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions) document. This material proposes effective naming strategies for media content files (assets).

There are two fundamental file naming styles (see table below). You must choose one at the very beginning of the R&D phase and strictly adhere to it from start to project release.

### Two Primary File Naming Styles

#### 1. `PascalCase`

Words are written without spaces, with each new word capitalized. Underscores may be used for logical context separation:

- `MagneticRifle`
- `MagneticRifle_Broken`

Prefixes and suffixes are written in ALL CAPS:

- `SM_RailwayStation`
- `RailwayStation_EM`

#### 2. `lower-case`

Uses only lowercase characters for names, prefixes, and suffixes. Underscores replace spaces:

- `magnetic_rifle`
- `sm_railway_station`
- `railway_station_em`

Hyphens (kebab-case) are an acceptable alternative:

- `magnetic-rifle`
- `sm-railway-station`
- `railway-station-em`

### Style Comparison

**PascalCase** is the most common among Unity developers, as it naturally follows C# coding conventions.

**lower-case** may be preferred if your studio uses a custom low-level asset management and streaming system — this reliably prevents bugs related to filesystem case sensitivity (e.g., ext4 on Linux) when streaming assets from a server.

### General Rules

In both cases, only the Latin alphabet, numbers, and certain special characters are allowed in file names. Besides underscores and hyphens, Unity reserves the `@` symbol for linking animation files with 3D meshes.

**Critical rule:** Never apply your chosen internal style to third-party plugin files (from the Asset Store). Keep their original structure intact, making only cosmetic changes when absolutely necessary. This will save significant time during planned package updates.

**Spaces in file paths are strictly prohibited** — they lead to hard-to-trace system errors.

---

## File Name Prefix

Using special prefixes affects physical file sorting in directories: all files sharing the same prefix are always grouped together. This is the most valuable benefit of this tool. However, prefixes can reduce name readability, so they should be applied systematically across all files in a category without exceptions, allowing the team to become accustomed to the pattern.

### Type Prefix

Within Unity itself, the type prefix offers no practical benefit, but it is indispensable when analyzing the repository through external VCS tools (Git) or the OS file manager.

**Recommended abbreviation standard:**

| Prefix | Meaning |
|--------|---------|
| `SM_`  | Static Mesh |
| `SK_`  | Skeletal Mesh |
| `P_`   | Prefab |
| `PS_`  | Particle System |
| `M_`   | Material |
| `PM_`  | Physical Material |
| `T_`   | Texture |
| `A_`   | Audio Clip |
| `AM_`  | Audio Mixer |
| `S_`   | Scene |
| `SP_`  | Sprite |
| `F_`   | Font |
| `SH_`  | Shader |

### Target Prefix

Used for assets and prefabs to explicitly indicate their runtime behavior context. For scene structuring, for example:

- `SC_FishingVillage` — main base scene that can be launched standalone.
- `SA_FishingVillage` — scene strictly intended for additive loading (`LoadSceneMode.Additive`).
- `TST_FishingVillage` — isolated programmer test scene.
- `TSA_FishingVillage` — test scene for additive subsystem validation.

### Sequence Prefix

A numeric prefix is necessary for ordering objects with different names into a clear execution queue. This is critical for storing dialogue audio files, where character lines must follow a specific order. Always use leading zero padding (typically two digits):

- `01_BootScene`
- `02_SandyBeachScene`
- `03_OldFactoryScene`

**Important:** Given the specific nature of the Sequence prefix, do not combine it with other prefixes to avoid overloading the string.

### Temp Prefix

Any temporary files or sandbox folders are marked with a double underscore (or double hyphen in lower-case style) at the very beginning of the name. This double character allows for instant filtering via global search for bulk cleanup before release:

- `__RailwayStationScene`

---

## File Name Suffix

A suffix adds metadata about the object with minimal impact on basic alphabetical file sorting, making it an extremely convenient tool.

### Target Suffix

Indicates runtime system usage specifics:

- `BootScene_1P` — singleplayer mode configuration.
- `BootScene_2P` — local split-screen multiplayer configuration.
- `BootScene_ADD` — additional additive scene content.

### Version Suffix

Although versioning is typically handled by Git, manual versioning suffixes are invaluable when an artist or designer needs to visually compare multiple iterations of an object directly within a single scene. Use strict numeric progression with leading zero padding:

- `RedStone_V01`
- `RedStone_V02`
- `RedStone_V03`

**Rule:** Never use names like `StoneFinal`, `StoneVersion2`, or `StoneBetter`. In game development, you cannot predict which version will become final — use only numeric indices.

### Sequence Suffix

Used to create arrays of minor variations of the same asset (useful for surface textures and environment details). Indexing should **strictly start from zero** (`_00`) so that the asset name in the directory perfectly matches its programmatic array index in code:

- `RedStone_00` (variant 0)
- `RedStone_01` (variant 1)
- `RedStone_02` (variant 2)

### Aspect Suffix

Used instead of numbers when an object has clearly defined logical or physical states:

- **UI states:** `EnterButton_Active`, `EnterButton_Inactive`
- **Skybox sides:** `JungleSky_Top`, `JungleSky_North`
- **Mesh LOD levels:** `RedCyborg_LOD0`, `RedCyborg_LOD1`

### Content Suffix

Most commonly used in the technical art and texturing pipeline to explicitly indicate material maps:

| Suffix | Description |
|--------|-------------|
| `_RGB` / `_BC` | Albedo / Base Color |
| `_A` | Opacity / Alpha |
| `_BC_A` | Combined: Albedo + Alpha |
| `_R` | Roughness / Smoothness |
| `_MT` | Metallic |
| `_MT_R` | Combined: Metallic + Roughness |
| `_SP` | Specular |
| `_SP_R` | Combined: Specular + Roughness |
| `_EM` | Emission |
| `_N` | Normal Map |
| `_DP` / `_H` | Displacement / Height Map |
| `_AO` | Ambient Occlusion |
| `_FM` | Flow Map |
| `_M` | Universal Shader Mask |

---

## Combining Suffixes

The Content suffix has the highest informational priority and **must always appear at the very end** of the file name. Order indices and version numbers precede it.

**Full file name format:**

`[prefix][name][aspect][sequence|target][version][content]`

**Examples of correct combination:**

- `Grass_00_V01_RGB` — variant 0, iteration 1, color map
- `Grass_00_V01_A` — variant 0, iteration 1, alpha map
- `Sky_North_V01_RGB` — north sky side, iteration 1, color map

**Important:** Combining multiple suffixes reduces readability and memorability of file names. It is advisable to minimize the number of suffixes per name — use no more than three simultaneously.

---

## Unity Reserved Rules

The Unity editor natively ignores and does not import files and folders that:

- Have the name `cvs`
- Start with a dot character `.`
- End with a tilde `~` (used for hiding folders like `Examples~` in UPM packages)
- Have the `.tmp` extension

---

## Object Naming Rules

Naming objects only appears simple at first glance. Developers must carefully select grammatically correct English words that accurately describe the asset's essence:

1. **Minimum words.** Use the fewest words necessary to describe the appearance (typically two): `RedPill`, `BluePill`.

2. **General to specific (left to right).** The most precise descriptor goes on the left, with clarifying details on the right. Write `red_cyborg` instead of `CyborgRed`.

3. **English only.** Never use transliteration (non-English words written in Latin script) — this creates multiple spelling variations (`Yashik`, `Jasik`, `Box`).

4. **Avoid generalizations.** If you're modeling a specific entity, don't name it abstractly as `Bird`. Name it `Flamingo`, `Eagle`, or `Swallow`.

5. **Avoid slang.** Use commonly known academic terms that any new team member or outsourcer will understand.

6. **Antonyms from the dictionary.** For opposing states, use precise antonym pairs. For example, use `Stop` for `Start`, and `End` for `Begin`. Consult an antonym dictionary if you are unsure.

---

## Conclusions

- For Unity developers, `PascalCase` is the recommended choice to minimize architectural risks.
- Never rename or restructure third-party Asset Store asset files.
- The most valuable and effective prefix is the numeric Sequence prefix for ordering.
- Suffixes provide maximum flexibility and informativeness with minimal impact on directory structure.
- For large projects, it is recommended to develop two small automation utilities:
  1. An internal batch renaming/renumbering tool that follows the established conventions.
  2. A CI/CD system validator that checks target project folders for strict compliance with the naming convention before build assembly.

---

## References

- 📄 **[Microsoft Coding Conventions C#](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)**
- 📄 **[50 Tips and Best Practices for Unity by Herman Tulleken (08/12/16)](https://www.gamedeveloper.com/design/50-tips-and-best-practices-for-unity-2016-edition-)**
- 📄 **[Unreal Engine Assets Naming Convention](https://docs.unrealengine.com/4.27/en-US/ProductionPipelines/AssetNaming/)**
- 📄 **[Unity Project Style Guide](https://github.com/timdhoffmann/unity-project-style-guide)**
- 📄 **[Unity Best Practices](https://www.glenstevens.ca/unity3d-best-practices/)**

