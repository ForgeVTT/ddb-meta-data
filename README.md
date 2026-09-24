# D&D Beyond metadata

This repository contains maintained Foundry VTT metadata for D&D Beyond sources and the assembled per-source directories consumed by The Forge's D&D Beyond converter. It supplements the D&D Beyond database with prepared scenes, journal-note links, roll-table instructions, extra assets, source-page redirects, and contributor attribution.

For the complete integration flow and maintainer runbook, see the monolith's [D&D Beyond documentation](../../../docs/dndbeyond/README.md).

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

## Contributing scene adjustments

A DDB Scene Config export can preserve scene alignment and scale, walls and doors, lighting, notes, D&D Beyond monster tokens, drawings, tiles, and supported module flags.

To create an export with DDB Importer:

1. Open the browser developer console in Foundry and run:

   ```js
   game.settings.set("ddb-importer", "allow-scene-download", true);
   ```

2. Right-click the scene in the scene navigation and choose **DDB Scene Config**.
3. Check the exported JSON for secrets and unrelated world data, then submit it through the [scene metadata form](https://forms.gle/NvyRWdUxi9Dho4As9).

Report missing scenes, pins, parsing errors, or unclear numbered handouts through the repository's [issue tracker](https://github.com/ForgeVTT/ddb-meta-data/issues). Current scene-support status is published at <https://docs.ddb.mrprimate.co.uk/status.html>.

Scene and table files retain D&D Beyond identifiers in `flags.ddb` so the assembler and converter can reconnect them to source content. Follow a recent file for the current shape; do not rely on a copied field list because Foundry scene schemas and exporter output evolve.

## Assemble and validate

Use the monolith workflow rather than running this repository in isolation. The canonical commands, prerequisites, cache layout, and review checklist are in [Operations and troubleshooting](../../../docs/dndbeyond/operations.md#refresh-sources-and-metadata).

Assembly writes `modules/<slug>/module.json` and `README.md`, creates newline-delimited NeDB packs where source metadata exists, and copies `content/assets/<slug>/` into the generated module. A full assembly pass also updates contributors and pack declarations and cleans descriptions across the generated tree. The Forge converter currently consumes the manifest, README, scene pack, and referenced assets; assembled table and folder packs are not merged into generated user packages.

The runtime contract is the assembled source directory:

```text
DNDBCONVERTER_METADATA_ROOTDIR/<slug>/module.json
```

`DNDBCONVERTER_METADATA_ROOTDIR` must therefore point to this repository's `modules/` directory, not to `content/scene_info/`.

## Fan content

Scene adjustments and walling data are released as unofficial Fan Content permitted under the Fan Content Policy. Not approved or endorsed by Wizards. Portions of the materials used are property of Wizards of the Coast. © Wizards of the Coast LLC.
