# KillaDome Store Configuration Guide

## Overview

The KillaDome Store Tab now features a dynamic inventory system that automatically updates when new guns or attachments are added. This guide explains how to configure and extend the store.

## Architecture

The store uses a centralized registry system located in the `StoreAPI` class:

```
StoreAPI
├── _storeInventory (List<StoreItem>) - Master inventory list
├── _cachedGunSkins (List<StoreItem>) - Cached gun skins for performance
└── _cachedAttachments (List<StoreItem>) - Cached attachments for performance
```

## Adding New Gun Skins

To add a new gun skin to the store, locate the `InitializeStoreInventory()` method in the `StoreAPI` class (around line 2815) and add a new line:

### Syntax
```csharp
AddGunSkin("Display Name", "weaponType", "itemId", "imageId", cost);
```

### Parameters
- **Display Name** (string): The name shown to players in the store UI
- **weaponType** (string): Internal weapon identifier that matches the weapon system
  - Examples: `"ak47"`, `"m249"`, `"pistol"`, `"lr300"`, `"mp5"`, `"thompson"`
- **itemId** (string): Unique identifier used for purchase tracking and ownership
  - This should be unique across all items in the store
  - Convention: `"skin_weapontype_skinname"` (e.g., `"skin_ak47_gold"`)
- **imageId** (string): Image identifier for the ImageLibrary plugin integration
  - Used to display item preview images in the store
  - Must match the image registered in ImageLibrary
- **cost** (int): Token price for the item

### Examples

```csharp
// Add a gold AK-47 skin
AddGunSkin("AK-47 Gold Edition", "ak47", "skin_ak47_gold", "ak47_gold_preview", 750);

// Add a chrome LR-300 skin
AddGunSkin("LR-300 Chrome", "lr300", "skin_lr300_chrome", "lr300_chrome_preview", 600);

// Add a tactical MP5 skin
AddGunSkin("MP5 Tactical Black", "mp5", "skin_mp5_tactical", "mp5_tactical_preview", 500);

// Add a unique Rust+ skin
AddGunSkin("AK-47 Neon", "ak47", "3102802323", "ak47_neon", 500);
```

### Current Gun Skins (as of this version)
```csharp
AddGunSkin("AK-47 Neon Skin", "ak47", "3102802323", "ak47_neon", 500);
AddGunSkin("AK-47 Classic Skin", "ak47", "skin_ak47_classic", "ak47_classic", 400);
AddGunSkin("M249 Chrome", "m249", "skin_m249_chrome", "m249_chrome", 450);
AddGunSkin("Pistol Black", "pistol", "skin_pistol_black", "pistol_black", 300);
```

## Adding New Attachments

To add a new attachment, use the `AddAttachment()` method in the same location:

### Syntax
```csharp
AddAttachment("Display Name", "itemId", "imageId", cost, isAlterEgo);
```

### Parameters
- **Display Name** (string): The name shown to players in the store UI
- **itemId** (string): Rust item short name or custom identifier
  - For Rust items: Use the actual item short name (e.g., `"weapon.mod.silencer"`)
  - For Alter-Ego variants: Append `_AE` to the base item ID
- **imageId** (string): Image identifier for ImageLibrary preview
- **cost** (int): Token price for the item
- **isAlterEgo** (bool): `true` for Alter-Ego variants, `false` for normal versions
  - Alter-Ego items appear with a ⚡ icon and orange text

### Examples

```csharp
// Add a normal scope
AddAttachment("4x Scope", "weapon.mod.4x.scope", "scope_4x_preview", 350, false);

// Add an Alter-Ego scope (enhanced stats)
AddAttachment("4x Scope AE", "weapon.mod.4x.scope_AE", "scope_4x_ae_preview", 750, true);

// Add a normal grip
AddAttachment("Vertical Grip", "weapon.mod.verticalgrip", "grip_vertical", 300, false);

// Add an Alter-Ego grip
AddAttachment("Vertical Grip AE", "weapon.mod.verticalgrip_AE", "grip_vertical_ae", 700, true);
```

### Current Attachments (as of this version)

**Scopes:**
```csharp
AddAttachment("Small Scope", "weapon.mod.small.scope", "small_scope", 250, false);
AddAttachment("Small Scope AE", "weapon.mod.small.scope_AE", "small_scope_ae", 600, true);
AddAttachment("8x Scope", "weapon.mod.8x.scope", "8x_scope", 400, false);
AddAttachment("8x Scope AE", "weapon.mod.8x.scope_AE", "8x_scope_ae", 800, true);
```

**Underbarrel:**
```csharp
AddAttachment("Holo Sight", "weapon.mod.holosight", "holo_sight", 300, false);
AddAttachment("Holo Sight AE", "weapon.mod.holosight_AE", "holo_sight_ae", 700, true);
AddAttachment("Laser Sight", "weapon.mod.lasersight", "laser_sight", 250, false);
AddAttachment("Laser Sight AE", "weapon.mod.lasersight_AE", "laser_sight_ae", 650, true);
```

**Silencers/Muzzle:**
```csharp
AddAttachment("Soda Can Silencer", "weapon.mod.sodacansilencer", "sodacan_silencer", 150, false);
AddAttachment("Soda Can Silencer AE", "weapon.mod.sodacansilencer_AE", "sodacan_silencer_ae", 550, true);
AddAttachment("Oil Filter Silencer", "weapon.mod.oilfiltersilencer", "oilfilter_silencer", 200, false);
AddAttachment("Oil Filter Silencer AE", "weapon.mod.oilfiltersilencer_AE", "oilfilter_silencer_ae", 600, true);
AddAttachment("Silencer", "weapon.mod.silencer", "silencer", 400, false);
AddAttachment("Silencer AE", "weapon.mod.silencer_AE", "silencer_ae", 800, true);
AddAttachment("Muzzle Brake", "weapon.mod.muzzlebrake", "muzzle_brake", 300, false);
AddAttachment("Muzzle Brake AE", "weapon.mod.muzzlebrake_AE", "muzzle_brake_ae", 700, true);
AddAttachment("Muzzle Boost", "weapon.mod.muzzleboost", "muzzle_boost", 350, false);
AddAttachment("Muzzle Boost AE", "weapon.mod.muzzleboost_AE", "muzzle_boost_ae", 750, true);
```

## Weapon Types

Supported weapon types for gun skins (these match the loadout system):
- `"ak47"` - AK-47 Assault Rifle
- `"m249"` - M249 Light Machine Gun
- `"pistol"` - Semi-Automatic Pistol
- Additional weapons can be added by extending the weapon mapping in `GiveWeapon()` method

## ImageLibrary Integration

The store uses the ImageLibrary plugin for displaying item preview images. To add images:

1. Install the ImageLibrary plugin on your server
2. Register your images with ImageLibrary using the imageId you specified
3. The store will automatically display the images when available

Example ImageLibrary registration:
```csharp
ImageLibrary.Call("AddImage", "https://url-to-image.png", "ak47_gold_preview");
```

## Performance Notes

The store system uses caching for optimal performance:
- Item lists are filtered and cached during initialization
- No repeated LINQ queries on every UI render
- Suitable for hundreds of items without performance impact

## Pricing Guidelines

Suggested pricing tiers based on item rarity/power:
- **Basic items**: 100-250 tokens
- **Standard items**: 250-400 tokens
- **Premium items**: 400-600 tokens
- **Rare items**: 600-800 tokens
- **Ultra-rare items**: 800-1000+ tokens
- **Alter-Ego items**: 2-3x the normal variant cost

## Troubleshooting

### Items not appearing in store
1. Check that the item was added in `InitializeStoreInventory()`
2. Verify the syntax matches the examples
3. Ensure the StoreAPI is initialized in the plugin's Init() method
4. Check server logs for any errors during initialization

### Images not displaying
1. Verify ImageLibrary plugin is loaded
2. Check that images are registered with correct imageId
3. Ensure image URLs are accessible from the server

### Items purchasable but not applying
1. Verify the itemId matches what's used in the loadout system
2. Check that weapon types are correctly mapped
3. Review the purchase command handler for any errors

## Data Structure Reference

### StoreItem Class
```csharp
public class StoreItem
{
    public string Name { get; set; }           // Display name in UI
    public StoreItemType ItemType { get; set; } // GunSkin or Attachment
    public string WeaponType { get; set; }      // For gun skins only
    public string Id { get; set; }              // Unique item identifier
    public string ImageId { get; set; }         // ImageLibrary reference
    public int Cost { get; set; }               // Token price
    public bool IsAlterEgo { get; set; }        // Enhanced variant flag
}
```

### StoreItemType Enum
```csharp
public enum StoreItemType
{
    GunSkin,      // Weapon skins/camos
    Attachment    // Weapon attachments and mods
}
```

## Migration from Hardcoded Arrays

If upgrading from an older version with hardcoded arrays:
1. The old arrays in `ShowStoreTab()` have been removed
2. All items are now in `InitializeStoreInventory()`
3. Existing player purchases are preserved (uses same itemId system)
4. No database migration required

## Future Extensions

The dynamic system supports easy extension:
- New weapon categories (melee, throwables, etc.)
- Bundle/package items
- Limited-time offers
- Seasonal items
- VIP-exclusive items
- Achievement-unlocked items

To add new categories, extend the `StoreItemType` enum and add corresponding methods to `StoreAPI`.
