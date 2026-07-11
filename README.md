# Storekeeper — Merchant Point of Sale (MPoS)

> A roleplay Point-of-Sale register for World of Warcraft merchants.  
> Manage products, orders, staff, and treasury — all from a single in-game window.

--- 

## Overview

Storekeeper turns WoW into a fully functional merchant experience for roleplay servers and communities. Whether you run a candle shop, an apothecary, or a travelling market stall, Storekeeper gives you everything you need to manage your business in character.

It works for solo merchants as well as full teams — owners can bring on managers and employees, sync product catalogues across all staff in real time, track orders, manage treasury, and review a complete activity log.

---

## Features

### Register
The main point-of-sale screen. Products are organised into collapsible categories. Click a product to add it to the cart, then choose how to process the sale.

- **Finalize** — logs a sale instantly to the Activity Log and updates the treasury. Ideal for quick walk-up sales.
- **Confirm & Log** — creates a full tracked order with buyer IC and OOC name, assignable to a staff member with an ETA and payment type.
- Payment types: Paid in Full, Paid as a Favour, Pay After Delivery.
- Out-of-stock products are dimmed and unclickable in the register grid.

### Products
- Add, edit, and delete products with name, description, price, icon, category, rarity, and stock.
- **Rarity** — assign Poor through Legendary rarity; the product name is tinted with the matching WoW rarity colour throughout the UI.
- **Stock** — toggle limited stock per product; quantity decrements automatically on each sale and can be adjusted from the Shop(s) Overview tab.
- **Category colours** — assign a custom colour to each category; the header buttons in the register grid reflect it and it syncs to all staff.
- **Icon picker** — 32,874 icons covering all expansions through The War Within and Midnight, with a runtime supplement that surfaces additional icons from your spellbook, mount journal, and pet journal at login.

### Orders
- Full order tracking with buyer name (IC and OOC), assigned staff member, ETA, and payment status.
- Unclaimed orders show only a Claim button; status controls appear once claimed.
- ETA expiry — claimed orders with a passed ETA automatically release back to unclaimed.
- Order list syncs to all online staff in real time.

### Shop(s) Overview & Treasury
- Manage multiple shops from a single panel.
- **Treasury** — tracks total earnings per shop in copper, displayed as gold, silver, and copper with deposit and withdraw controls for owners.
- Treasury auto-updates when a Paid in Full sale is finalised or a paid order is marked complete.
- **Stock overview** — lists all limited-stock products with +/− adjustment buttons (permission-gated).
- **Request Sync** button for when you need to force a manual data refresh from the owner.

### Management
- Invite staff by character name and assign roles: Owner, Manager, Employee.
- Reorder staff priority with Move Up / Move Down.
- **Granular permissions** — owners configure what managers and employees are allowed to do: edit products, apply discounts, finalise as a favour, adjust stock, add employees.
- Staff discounts — set a percentage discount per staff member.

### Activity Log
- Every shop event is logged: sales, orders, treasury deposits and withdrawals, stock changes, staff changes, and system events.
- Filter tabs narrow the log to: Treasury, Orders, Products, Staff, System.

### Network & Sync
- Staff receive automatic sync when coming online — products, orders, category colours, and treasury are all kept in sync.
- Update notifications — if a colleague online is running a newer version of Storekeeper, you receive a one-time chat notification with a download link.
- Manual sync available from the Shop(s) Overview tab if data seems out of date.

---

## Installation

1. Download the latest release from [CurseForge](https://www.curseforge.com/wow/addons/storekeeper).
2. Extract the `Storekeeper` folder into your WoW addons directory:
   ```
   World of Warcraft/_retail_/Interface/AddOns/Storekeeper/
   ```
3. Log in and type `/sk` to open the register.

---

## Slash Commands

| Command | Action |
|---|---|
| `/sk` | Open the register |
| `/sk manager` | Open the manager window |
| `/sk sync` | Request a full sync from the shop owner |

---

## Optional Dependencies

Storekeeper works standalone but integrates with the following addons when present:

- **Total RP 3** — character names are pulled from your TRP3 profile for receipts and the seller label in the register.

---

## Compatibility

| Game Version | Status |
|---|---|
| The War Within (11.x) | Supported |
| Dragonflight (10.x) | Supported |
| Classic / Era | Not supported |

---

## Changelog

Storekeeper - Changelog

## v1.3.8.1

### Added
- **Production-cost economics.** New per-product `restockCost` field and a per-shop `trackProductionCost` toggle (owner-gated, default false, set in the Stock & Treasury sub-tab).
  - `DB:ApplyRestockCost(shopName, product, unitsAdded)` deducts `restockCost * units` from the treasury on stock increases only; clamps at 0 (matching `AddTreasury`), returns `cost, paid, short` so the UI can warn on shortfall. Logged to the activity log under the `treasury` category.
  - Product form gains a Restock Cost g/s/c row; `SaveProduct` reads it into `fields.restockCost`, `LoadProduct` populates it, `ClearForm` resets it. Base form height 280 → 308 and the variants panel anchor shifted down to make room; product-list scrollframe bottom anchor adjusted to match.
  - Sync: `restockCost` appended as field 12 in `SerialiseProduct`/`DeserialiseProduct` (variants stays field 11) — backwards-compatible in both directions; older peers ignore field 12, newer clients default it to nil. `AddProductFromTable` carries it. The toggle is appended as field 9 in `SHOP_INFO`; `SyncShop` refactored into reusable `NET:SendShopInfo` plus `NET:BroadcastShopInfo` for live owner→staff propagation on toggle. Receiver guards against older peers omitting the field.
  - `DB:MigrateShop` defaults `trackProductionCost` to false.
- **Tutorial.** Added Restock Cost and Stock & Treasury economics steps to the main `STEPS` walkthrough; the latter sets `SK.Manager._peopleSubTab = "inventory"` in `setup` before switching tabs.

### Fixed
- **Product list scroll reset on save.** `SaveProduct` now captures `M.fProductSF:GetVerticalScroll()` before the rebuild and restores it via `C_Timer.After(0, ...)`, clamped to the new `GetVerticalScrollRange`, so editing an item no longer snaps the list to the top.
- **Stock tab scroll reset on +/-.** The Stock +/- handlers no longer call `M:SwitchTab("people")` (a full tab rebuild). They now update the row's stock cell in place via a local `RefreshStockCell` closure (GetProducts returns live references; `SetStock` mutates them), preserving scroll position and eliminating flicker. The treasury FontString is updated in place when a restock charge applies.

---

## Contributing & Bug Reports

Found a bug or have a feature request? Open an issue on GitHub or use the **What's New** button inside Storekeeper to find the beta tester sign-up form.

If you want to contribute, please open an issue first to discuss what you'd like to change.

---

## License

All rights reserved. You may not redistribute, modify, or republish this addon or any part of it without explicit written permission from the author.
