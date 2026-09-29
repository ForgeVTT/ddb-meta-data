# D&D Beyond metadata

This repository is a fork of [MrPrimateddb-meta-data](https://github.com/MrPrimate/ddb-meta-data) and contains metadata associated with books on D&D Beyond.
It is used by [The Forge's D&D Beyond converter](https://github.com/ForgeVTT/theforge/blob/master/docs/dndbeyond/README.md) to align maps, place tokens, and set up D&D Beyond content in generated Foundry VTT packages.

## Repository layout

| Path                             | Purpose                                                                      |
| -------------------------------- | ---------------------------------------------------------------------------- |
| `content/scene_info/<slug>/`     | Maintained Foundry scene exports.                                            |
| `content/note_info/<slug>.json`  | Rules for linking scene notes to D&D Beyond journal content.                 |
| `content/table_info/<slug>.json` | RollTable extraction/linking instructions.                                   |
| `content/assets/<slug>/`         | Metadata-owned files, usually replacement tiles.                             |
| `content/contributors.json`      | Per-source contributor attribution.                                          |
| `redirect.json`                  | Confirmed D&D Beyond source URLs; `null` means no usable page was confirmed. |
| `modules/<slug>/`                | Generated manifest, README, NeDB packs, and copied assets.                   |

`content/meta.json`, `content/status.json`, and `content/versions.json` describe the maintained dataset. The legacy `content/journal_info/` directory is not read by the current assembler.

The `modules/` tree is generated but versioned. Do not fix generated files without making the corresponding change in `content/` or the assembly tools.

## Current Scene Support

You can see the current state of scene support on the [DDB Importer documentation site](https://docs.ddb.mrprimate.co.uk/status.html).

If you wish to help improve scene walls and lighting, see [MrPrimate/ddb-meta-data README.md#contribution](https://github.com/MrPrimate/ddb-meta-data/blob/main/README.md#contribution).

## Assemble and validate

Use the monolith workflow rather than running this repository in isolation. The canonical commands, prerequisites, cache layout, and review checklist are in [Operations and troubleshooting](https://github.com/ForgeVTT/theforge/blob/master/docs/dndbeyond/operations.md#refresh-sources-and-metadata).

Assembly writes `modules/<slug>/module.json` and `README.md`, creates newline-delimited NeDB packs where source metadata exists, and copies `content/assets/<slug>/` into the generated module. A full assembly pass also updates contributors and pack declarations and cleans descriptions across the generated tree. The Forge converter currently consumes the manifest, README, scene pack, and referenced assets; assembled table and folder packs are not merged into generated user packages.

The runtime contract is the assembled source directory:

```text
DNDBCONVERTER_METADATA_ROOTDIR/<slug>/module.json
```

`DNDBCONVERTER_METADATA_ROOTDIR` must therefore point to this repository's `modules/` directory, not to `content/scene_info/`.

## Flags

### Actors

- `monsterId` - The ID of the associated DDB monster

### Tables

- `ddbId` - D&D Beyond record ID
- `cobaltId` - D&D Beyond Cobalt content ID
- `parentId` - ID of the parent content record
- `slug` - D&D Beyond content slug
- `tagIdFirst` - The ID of the start tag
- `contentChunkId` - The table's content chunk ID
- `sceneName` - Name of the D&D Beyond section containing the table

### Scenes

- `bookCode` - e.g. `lmop`, `cos`
- `ddbId` - D&D Beyond record ID
- `cobaltId` - D&D Beyond Cobalt content ID
- `parentId` - ID of the parent content record
- `contentChunkId` - The scene's content chunk ID
- `foundryVersion` - Foundry version used to export the scene
- `versions` - Versioning data
  - `ddbMetaData` - Versioning data for the metadata specifically
    - `name` - Name of scene
    - `bookCode` - e.g. `lmop`, `cos`
    - `contentChunkId` - The map's content chunk ID
    - `lastUpdate` - Last metadata version the scene was updated in
    - `notes` - Version of notes
    - `tokens` - Version of tokens
    - `walls` - Version of walls
    - `lights` - Version of lights
    - `drawings` - Version of drawings
    - `foundry` - Foundry version used for the metadata update
- `noteInfos` - Optionally contains data used for splitting scene notes
  - `ddbId` - D&D Beyond record ID
  - `cobaltId` - D&D Beyond Cobalt content ID
  - `parentId` - ID of the parent content record
  - `splitTag` - Tag to split on
  - `slug` - Slug of the scene note
  - `tagIdFirst` - The ID of the start tag
  - `contentChunkIdStart` - Content chunk ID of the tag to start parsing at
  - `tagIdLast` - The ID of the stop tag
  - `contentChunkIdStop` - Content chunk ID of the tag to stop parsing at
  - `sceneName` - Name of the scene

## Fan Content

The scene adjustments and walling data are released as unofficial Fan Content permitted under the Fan Content Policy. Not approved or endorsed by Wizards. Portions of the materials used are property of Wizards of the Coast. © Wizards of the Coast LLC.
