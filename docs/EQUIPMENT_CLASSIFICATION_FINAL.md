# Equipment Classification Final

## Method
Items classified by `type` field from client binary:
- 1=Weapon, 4=Armor, 6=Shield, 7=Helmet, 9=Accessory, 11=Boots, 14=Cape
- 0,2=Consumable, 3=Material, 5=Quest, 8=Scroll, 10=Ammo, 15=Skillbook
- 16=Potion, 17=Food, 18=CraftMat, 19=Currency, 20+=Misc

## Results
- Total items: 16,318
- Equipment items: ~3,265 (types 1,4,6,7,9,11,14)
- Consumables (Potion+Food): ~1,270
- Materials: ~233

## Equipment Progression
- Entries: 135 (10-level bands per slot)
- Confidence: CLIENT_FACT (from binary type field)
