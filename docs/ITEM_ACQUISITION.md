# Item Acquisition

## Sources
- SHOP: Available from sellers (shop_items)
- QUEST: Quest reward candidate (PROBABLE)
- QUEST_REQUIRED: Required for quest (PROBABLE)
- CRAFT: Crafted item (craft.edt)
- MIX: Mixed item (mix.edt)

## Rules
- Shop items: CLIENT_FACT from seller.edt
- Quest items: PROBABLE from raw consequence fields
- Craft/Mix: BINARY_CONFIRMED from binary recipes
