A brief cheatsheet on how to properly add and integrate new items into STALKER, some documentation of what modules are affected and the relevant root files and sections for writing patches via DLTX. This document assumes a pre-existing familiarity with DLTX and how section and record overrides work.

> [!NOTE]
> This document is primarily written for the overall structure of the GAMMA mod pack and not all parts of it may be fully applicable to other packs or your custom set-up. The majority of it should apply to all Open X-Ray based games (e.g. Anomaly).

## Core Modules

Files and associated paths of the base game.

### NPC Loot Numbers (`death_generic`)

DLTX: `configs/items/settings/mod_death_generic_[name].ltx`

Root module for all NPC loot, includes child modules `death_items` and `death_outfits`. Contains `item_count` section for minimum and maximum number of items dropped by each item identifier. If an item is dropped, it is dropped in the given quantities.

### NPC Loot Chances (`death_items`)

DLTX: `configs/items/settings/mod_death_generic_[name].ltx`

Child module for all NPC loot and their chances for each faction and experience level. Contains `base` and `base_experienced` as foundation sections that all other faction-specific sections inherit from.

### Animal Loot (`mutant_loot`)

DLTX: `configs/items/settings/mod_mutant_loot_[name].ltx`

Special module that defines potential loot found when harvesting mutants. Includes basic drop chance sections `misc_high` and `misc_low` with the format `item, amount, chance`.

### Stashes (`treasure_manager`)

DLTX: `configs/items/settings/mod_treasure_manager_[name].ltx`

Module for loot found in stashes. Contains various settings on categories and chances of items being found. Section `possible_items` defines a set of all items to be potentially spawned as loot.

### Disassembly (`parts`)

DLTX: `configs/items/settings/mod_parts_[name].ltx`

Module to define what components any item can be disassembled into. Separated into `settings` for shared values, `weapon_spare_parts` for the default item to receive on weapon disassembly, `con_parts_list` for parts with a simulated condition, and `nor_parts_list` for parts without a simulated condition. Contains key/value pairs for the item to be disassembled and its components, e.g. `book = prt_i_paper`.

### Repair (`items_repair`)

DLTX: `configs/items/items/mod_base_[name].ltx`

Module for items used for repairs, including the helper items that can optionally be used for a repair. Adding new miscellaneous items that may be used to help repair items or if existing items are repurposed to no longer be suitable to help in repairs, sections in this module need to be modified. The key `repair_parts_sections` defines the items that can be used to augment a repair. Note that even with DLTX, two mods cannot modify the item list in a single key.

### Crafting (`craft`)

DLTX: `configs/items/settings/mod_craft_[name].ltx`

Module for crafting recipes; defines both if the added or modified item can be crafted or if is used as a component to craft something else. Contains multiple numerically named sections from `[1]` to `[8]` corresponding to Anomaly recipe books (often all unlocked by default in a new game).

### Trade Profiles (`trade_*`)

DLTX: `configs/items/trade/mod_trade_[character_name]_[name].ltx`

Multiple modules that define stock, buying, and selling profiles for trader NPCs. The module `trade_presets` defines what items traders buy from the player indexed by category (e.g. `pistols`, `rifles`, `outfits`, `drinks`, `food`, `trash`, etc). The module also defines generial trader classes (e.g. `supplier`, `merchant`, `trasher`, `barman`, `medic`, `scientist`) that inherit item categories. However, the module is only included by specific trader profiles and thus cannot be modified with DLTX -- the individual trader profiles must be modified instead.

### Dynamic Item Spawns (`dynamic_item_spawn`)

DLTX: `configs/items/settings/mod_dynamic_item_spawn_[name].ltx`

Module to allow items to be spawned dynamically in the world as loot to be picked up from predefined positions. Defines number of uses for limited-use items (section `possible_uses`) and categories for the kind of loot found. Items may appear in more than one section at a time (e.g. `kit` and `medical`).

### Encyclopedia (`encyclopedia`)

DLTX: `configs/plugins/mod_encyclopedia_[name].ltx`

Core module that defines all sections used by the in-game PDA encyclopedia. Usually not modified directly, defer to plugins documented below for regular use cases.

### Encyclopedia Articles (`articles_*`)

DLTX: `configs/plugins/encyclopedia_custom/articles/mod_importer.ltx`

Plugin helper module that handles assignments of PDA encyclopedia categories to article keys. The engine assumes an article to exist by identifier alone, there is no dedicated section defined for it. An assigned article `article_example` is expected to have a localized string `article_example` with its title and `article_example_text` with its long-form contents.

May be able to define override with new plugin using low sort order, e.g. `articles/articles_zzz_[name].ltx`.

### Encyclopedia Categories (`categories_*`)

DLTX: `configs/plugins/encyclopedia_custom/categories/mod_importer.ltx`

Plugin helper module that manages PDA encyclopedia categories that articles can get assigned to. Acts as a top-level organisational structure to articles in the player's PDA.

### Debug Spawner (`spawner_blacklist`)

DLTX: `configs/plugins/mod_spawner_blacklist_[name].ltx`

Optional, when either repurposing an existing (previously blacklisted) item or adding a new item that should be blacklisted from the spawner. Allows specifying items that should not automatically appear in the Anomaly debug item spawner. The section of item identifiers is `ignore_sections`.

### Prefetching (`fetch_list`)

DLTX: `configs/items/settings/mod_fetch_list_[name].ltx`

Defines items to be prefetched by the engine. Contains sections for item categories. The section for miscellaneous items as used by this mod is `junk_x`. Records are item identifier and a boolean value to define if it should be prefetched, e.g. `bedspread = true`.

## Mod-Associated Modules

Files and associated paths of common mods.

### Grok Stashes (`grok_treasure_manager`)

DLTX: `configs/items/settings/mod_grok_treasure_manager_[name].ltx`

Module uses the same structure as `treasure_manager` but for Grok's modified stash system. The section `possible_items` defines a set of all items to be potentially spawned as loot.

### Group Icon Overlay (`base_groups`) (Maid's Icons)

DLTX: `configs/custom_icon_layers/groups/mod_base_groups_[name].ltx` (alternatively can create any LTX file as sibling as the base module includes all `group_*` files in the directory)

Module that defines groups for icon augmentation by category (Maid's DII mod). The section name for valuable miscellaneous items as used by this mod is `valuable_group`.

### Effects Icon Overlay (`base_properties`) (Maid's Icons)

DLTX: `configs/custom_icon_layers/properties/properties/mod_base_properties_[name].ltx` (alternatively can create any LTX file as sibling as the base module includes all `property_*` files in the directory)

Module that defines groups for icon augmentation by properties (Maid's DII mod), intended for items like consumables, upgrades, and artefacts.