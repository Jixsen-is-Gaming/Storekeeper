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
- **Storekeeper: Extended** — a companion addon that adds additional features. Currently in closed beta.

---

## Compatibility

| Game Version | Status |
|---|---|
| The War Within (11.x) | Supported |
| Dragonflight (10.x) | Supported |
| Classic / Era | Not supported |

---

## Changelog

### v1.3.2
**Bugs Fixed**
- Management tab: Move Up and Move Down buttons were too narrow, causing text to overflow outside the button borders.
- Products tab: Category and Price column headers were misaligned with the data rows beneath them.
- Shop(s) Overview: the Delete Shop and Leave This Shop buttons were clipped by the bottom border of the panel.

**Added**
- Icon picker: updated from 20,073 icons (BfA 8.0 era) to 32,874 icons, covering all content through The War Within and Midnight.
- Icon picker: runtime supplement — spellbook, mount journal, and pet journal are scanned at login to surface any icons not yet in the static list.
- Shop(s) Overview: manual Request Sync button added alongside Delete/Leave.

**Changed**
- Icon picker: now always opens Storekeeper's own built-in picker. Previously it would open TRP3's icon browser when TRP3 was loaded, causing confusion.
- What's New: moved from a delayed sidebar tab to a permanent button in the title bar, left of the close button. No more delay on first open.
- Sync button removed from the title bar — sync runs automatically on login.
- Removed dead TRP3 icon browser integration code that was no longer reachable.

### v1.3.1
- Granular staff permissions (edit products, apply discounts, adjust stock, add employees).
- Staff discounts per staff member.
- Category colours synced to all staff.
- Treasury auto-updates on Paid in Full sales and completed orders.
- Withdraw All button.
- ETA expiry — overdue claimed orders auto-release to unclaimed.
- Activity Log category filter tabs.
- Update notifications when a colleague is on a newer version.
- Stock overview with +/− adjustment buttons in Shop(s) Overview.

---

## Contributing & Bug Reports

Found a bug or have a feature request? Open an issue on GitHub or use the **What's New** button inside Storekeeper to find the beta tester sign-up form.

If you want to contribute, please open an issue first to discuss what you'd like to change.

---

## License

All rights reserved. You may not redistribute, modify, or republish this addon or any part of it without explicit written permission from the author.
