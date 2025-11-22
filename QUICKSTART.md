# Quick Start: Adding New Guns to KillaDome Store

## ⚡ TL;DR - How to Add a New Gun Skin

**Location:** `KillaDome.cs` → Line ~2815 → `InitializeStoreInventory()` method

**Add ONE line:**
```csharp
AddGunSkin("Display Name", "weaponType", "itemId", "imageId", cost);
```

**Example - Add a new LR-300 skin:**
```csharp
AddGunSkin("LR-300 Gold Edition", "lr300", "skin_lr300_gold", "lr300_gold", 750);
```

**Result:** ✅ Gun automatically appears in Store Tab!

---

## Step-by-Step Example

### Before (Old System - DON'T USE):
Had to edit hardcoded array in ShowStoreTab() method around line 2124:
```csharp
var gunSkins = new[]
{
    new { Name = "AK-47 Neon Skin", Cost = 500, Id = "3102802323", ImageId = "ak47_neon" },
    new { Name = "AK-47 Classic Skin", Cost = 400, Id = "skin_ak47_neon", ImageId = "ak47_classic" }
    // Had to add new item here in array, modify UI rendering code, test, etc.
};
```
❌ Problem: Requires modifying UI code, error-prone, not scalable

### After (New System - USE THIS):
Just add to registry in InitializeStoreInventory():
```csharp
private void InitializeStoreInventory()
{
    _storeInventory = new List<StoreItem>();
    
    // Add Gun Skins - automatically populated from gun registry
    AddGunSkin("AK-47 Neon Skin", "ak47", "3102802323", "ak47_neon", 500);
    AddGunSkin("AK-47 Classic Skin", "ak47", "skin_ak47_classic", "ak47_classic", 400);
    AddGunSkin("M249 Chrome", "m249", "skin_m249_chrome", "m249_chrome", 450);
    AddGunSkin("Pistol Black", "pistol", "skin_pistol_black", "pistol_black", 300);
    
    // ADD YOUR NEW GUNS HERE (example):
    AddGunSkin("LR-300 Gold Edition", "lr300", "skin_lr300_gold", "lr300_gold", 750);
    AddGunSkin("Thompson Dragon", "thompson", "skin_thompson_dragon", "thompson_dragon", 650);
    AddGunSkin("MP5 Tactical", "mp5", "skin_mp5_tactical", "mp5_tactical", 550);
    
    // ... rest of method
}
```
✅ Solution: One line per gun, automatic store updates, scalable, clean

---

## Real-World Examples

### Example 1: Add Multiple AK-47 Skins
```csharp
AddGunSkin("AK-47 Gold Plated", "ak47", "skin_ak47_gold", "ak47_gold", 800);
AddGunSkin("AK-47 Snow Camo", "ak47", "skin_ak47_snow", "ak47_snow", 600);
AddGunSkin("AK-47 Desert Ops", "ak47", "skin_ak47_desert", "ak47_desert", 550);
AddGunSkin("AK-47 Urban Elite", "ak47", "skin_ak47_urban", "ak47_urban", 700);
```

### Example 2: Add New Weapon Type (LR-300)
```csharp
AddGunSkin("LR-300 Standard", "lr300", "skin_lr300_standard", "lr300_standard", 400);
AddGunSkin("LR-300 Chrome", "lr300", "skin_lr300_chrome", "lr300_chrome", 500);
AddGunSkin("LR-300 Gold", "lr300", "skin_lr300_gold", "lr300_gold", 750);
```

### Example 3: Add Premium Skins
```csharp
AddGunSkin("M249 Diamond", "m249", "skin_m249_diamond", "m249_diamond", 1200);
AddGunSkin("AK-47 Dragon Lore", "ak47", "skin_ak47_dragonlore", "ak47_dragonlore", 1500);
```

---

## Parameter Guide

```csharp
AddGunSkin("Display Name", "weaponType", "itemId", "imageId", cost);
           ↑              ↑             ↑         ↑          ↑
           │              │             │         │          │
           │              │             │         │          └─ Token price (int)
           │              │             │         └─ Image name in ImageLibrary
           │              │             └─ Unique ID for purchase tracking
           │              └─ Weapon type: ak47, m249, pistol, lr300, mp5, etc.
           └─ Name shown in Store UI
```

### Parameter Details:

1. **Display Name**: What players see in the store
   - Use clear, descriptive names
   - Examples: "AK-47 Gold", "M249 Chrome", "Desert Eagle Black"

2. **weaponType**: Internal weapon identifier
   - Common types: `"ak47"`, `"m249"`, `"pistol"`, `"lr300"`, `"mp5"`, `"thompson"`
   - Must match weapon system (see GiveWeapon method for supported types)
   - Case-sensitive, use lowercase

3. **itemId**: Unique identifier for this specific skin
   - Must be unique across ALL items in store
   - Suggested format: `"skin_weapontype_skinname"`
   - Examples: `"skin_ak47_gold"`, `"skin_m249_chrome"`
   - Can also use official Rust skin IDs: `"3102802323"` (for real skins)

4. **imageId**: Reference for ImageLibrary plugin
   - Used to display preview image in store
   - Must be registered in ImageLibrary first
   - Examples: `"ak47_gold"`, `"m249_chrome_preview"`

5. **cost**: Token price
   - Must be positive integer
   - Suggested ranges:
     - Basic: 100-300 tokens
     - Standard: 300-500 tokens
     - Premium: 500-800 tokens
     - Rare: 800-1200 tokens
     - Legendary: 1200+ tokens

---

## Testing Your Changes

1. **Save the file** after adding your guns
2. **Reload the plugin** on your server:
   ```
   o.reload KillaDome
   ```
3. **Open the Store Tab** in-game
4. **Verify** your new guns appear with correct:
   - Name
   - Price
   - Preview image (if ImageLibrary configured)
   - Purchase button functionality

---

## Adding Attachments

Similar pattern for attachments:

```csharp
AddAttachment("Display Name", "itemId", "imageId", cost, isAlterEgo);
```

### Normal Attachment Example:
```csharp
AddAttachment("Vertical Grip", "weapon.mod.verticalgrip", "vertical_grip", 300, false);
```

### Alter-Ego Attachment Example:
```csharp
AddAttachment("Vertical Grip AE", "weapon.mod.verticalgrip_AE", "vertical_grip_ae", 700, true);
```

---

## Common Weapon Types

| Weapon Type | Description | Example Skins |
|------------|-------------|---------------|
| `"ak47"` | AK-47 Assault Rifle | Neon, Classic, Gold |
| `"m249"` | M249 Light Machine Gun | Chrome, Diamond |
| `"pistol"` | Semi-Auto Pistol | Black, Silver |
| `"lr300"` | LR-300 Assault Rifle | Gold, Tactical |
| `"mp5"` | MP5 SMG | Black, Urban |
| `"thompson"` | Thompson SMG | Dragon, Vintage |

*Note: Ensure weapon types are supported in the GiveWeapon() method*

---

## Troubleshooting

### Gun not appearing in store?
- ✅ Check syntax: all 5 parameters present, commas correct
- ✅ Verify itemId is unique (not used by another item)
- ✅ Reload plugin after changes
- ✅ Check server console for errors

### Image not showing?
- ✅ Verify ImageLibrary plugin is loaded
- ✅ Register image in ImageLibrary with matching imageId
- ✅ Use lowercase, no spaces in imageId

### Purchase not working?
- ✅ Verify itemId matches exactly
- ✅ Check player has enough tokens
- ✅ Verify weapon type exists in weapon mapping

---

## Complete Working Example

Here's a complete section showing multiple gun additions:

```csharp
// In InitializeStoreInventory() method, around line 2820

// Existing guns (already in file)
AddGunSkin("AK-47 Neon Skin", "ak47", "3102802323", "ak47_neon", 500);
AddGunSkin("AK-47 Classic Skin", "ak47", "skin_ak47_classic", "ak47_classic", 400);
AddGunSkin("M249 Chrome", "m249", "skin_m249_chrome", "m249_chrome", 450);
AddGunSkin("Pistol Black", "pistol", "skin_pistol_black", "pistol_black", 300);

// NEW GUNS - Add these lines:
AddGunSkin("AK-47 Gold Edition", "ak47", "skin_ak47_gold", "ak47_gold", 800);
AddGunSkin("AK-47 Dragon Fire", "ak47", "skin_ak47_dragonfire", "ak47_dragonfire", 1200);
AddGunSkin("M249 Diamond Bling", "m249", "skin_m249_diamond", "m249_diamond", 1500);
AddGunSkin("LR-300 Tactical Black", "lr300", "skin_lr300_tactical", "lr300_tactical", 600);
AddGunSkin("LR-300 Gold Rush", "lr300", "skin_lr300_goldrush", "lr300_goldrush", 900);
AddGunSkin("Thompson Vintage", "thompson", "skin_thompson_vintage", "thompson_vintage", 550);
AddGunSkin("MP5 Urban Ops", "mp5", "skin_mp5_urban", "mp5_urban", 500);
AddGunSkin("Pistol Silver", "pistol", "skin_pistol_silver", "pistol_silver", 350);

// Save, reload, done! All 8 new guns now appear in store ✅
```

---

## Benefits of This System

✅ **Simple**: One line per gun
✅ **Fast**: Add 10 guns in 1 minute
✅ **Automatic**: Store updates immediately
✅ **Safe**: No UI code modification needed
✅ **Scalable**: Can handle 100+ guns without issues
✅ **Maintainable**: All guns in one place

---

For full documentation, see: `STORE_CONFIGURATION.md`
