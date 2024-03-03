A brief cheatsheet on how to properly add and integrate new items into STALKER, some documentation of what modules are affected and the relevant root files and sections for writing patches via DLTX.

### `death_generic`

DLTX: `configs/items/settings/mod_death_generic_[name]`

Root module for all NPC loot, includes child modules `death_items` and `death_outfits`. Contains `item_count` section for minimum and maximum number of items dropped by each item identifier. If an item is dropped, it is dropped in the given quantities.

### `death_items`

DLTX: `configs/items/settings/mod_death_generic_[name]`

Child module for all NPC loot and their chances for each faction and experience level. Contains `base` and `base_experienced` as foundation sections that all other faction-specific sections inherit from.

### `mutant_loot`

DLTX: `configs/items/settings/mod_mutant_loot_[name]`

Special module that defines potential loot found when harvesting mutants. Includes basic drop chance sections `misc_high` and `misc_low` with the format `item, amount, chance`.

### `treasure_manager`

DLTX: `configs/items/settings/mod_treasure_manager_[name]`

Module for loot found in stashes. Contains various settings on categories and chances of items being found. Section `possible_items` defines a set of all items to be potentially spawned as loot.

### `grok_treasure_manager`

DLTX: `configs/items/settings/mod_grok_treasure_manager_[name]`

Module uses the same structure as `treasure_manager` but for Grok's modified stash system. Section `possible_items` defines a set of all items to be potentially spawned as loot.

### `parts`

DLTX: `configs/items/settings/mod_parts_[name]`

Module to define what components any item can be disassembled into. Separated into `settings` for shared values, `weapon_spare_parts` for the default item to receive on weapon disassembly, `con_parts_list` for parts with a simulated condition, and `nor_parts_list` for parts without a simulated condition. Contains key/value pairs for the item to be disassembled and its components, e.g. `book = prt_i_paper`.

### `configs/items/items/items_repair`

DLTX: `mod_base_[name]`

Module for items used for repairs, including the helper items that can optionally be used for a repair. Adding new miscellaneous items that may be used to help repair items or if existing items are repurposed to no longer be suitable to help in repairs, sections in this module need to be modified. The key `repair_parts_sections` defines the items that can be used to augment a repair. Note that even with DLTX, two mods cannot modify the item list in a single key.

### `trade_*`

DLTX: `configs/items/trade/mod_trade_[character_name]_[name]`

Multiple modules that define stock, buying, and selling profiles for trader NPCs. The module `trade_presets` defines what items traders buy from the player indexed by category (e.g. `pistols`, `rifles`, `outfits`, `drinks`, `food`, `trash`, etc). The module also defines generial trader classes (e.g. `supplier`, `merchant`, `trasher`, `barman`, `medic`, `scientist`) that inherit item categories. However, the module is only included by specific trader profiles and thus cannot be modified with DLTX -- the individual trader profiles must be modified instead.

### `base_groups` (Maid's Icons)

DLTX: `configs/custom_icon_layers/groups/mod_base_groups_[name]` (alternatively can create any LTX file as sibling as the base module includes all `group_*` files in the directory)

Module that defines groups for icon augmentation by category (Maid's DII mod). The section name for valuable miscellaneous items is `valuable_group`.

### `base_properties` (Maid's Icons)

DLTX: `configs/custom_icon_layers/properties/properties/mod_base_properties_[name]` (alternatively can create any LTX file as sibling as the base module includes all `property_*` files in the directory)

Module that defines groups for icon augmentation by properties (Maid's DII mod), intended for items like consumables, upgrades, and artefacts.

### `spawner_blacklist`

DLTX: `configs/plugins/mod_spawner_blacklist_[name]`

Optional, when either repurposing an existing (previously blacklisted) item or adding a new item that should be blacklisted from the spawner. Allows specifying items that should not automatically appear in the Anomaly debug item spawner. The section of item identifiers is `ignore_sections`.

### `fetch_list`

DLTX: `configs/items/settings/mod_fetch_list_[name]`

Defines items to be prefetched by the engine. Contains sections for item categories. The section for miscellaneous items is `junk_x`. Records are item identifier and a boolean value to define if it should be prefetched, e.g. `bedspread = true`.