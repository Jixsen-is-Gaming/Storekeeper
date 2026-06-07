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

### v1.3.4
**Added**
- Payment details in Finish Transaction — the popup now includes a Gratuity (tip) field and a Customer Paid field when Paid in Full is selected. Change due is calculated live and automatically handled in the treasury.
**Changed**
- The "Receipt / Order" button has been renamed to "Finish Transaction".
**Bug Fixes**
- Switching characters now always loads the correct TRP3 IC name for the character you log in on. Previously the name from the last played character would persist until you pressed "From TRP3" manually.
- The Storekeeper version badge in the TRP3 tooltip now shows for players outside of a group on first hover. A HELLO ping is sent the moment you hover someone, and the badge injects as soon as they reply without needing to re-hover. Fixed a casing mismatch (SK.NET vs SK.Net) that silently prevented the HELLO whisper from ever being sent to players not yet in the known peers list, meaning the badge would never appear for first-time encounters out of group.
- The dropdown when creating or editing a product now caps at 8 visible rows and scrolls with the mousewheel for larger category lists. Fixed the scroll direction being inverted, the frame level ordering causing items to render incorrectly, and clipping not working as intended.
- All addon messages are now queued and sent one at a time rather than in a single burst. This prevents WoW's rate limiter from triggering a disconnect when saving products in shops with multiple staff members or large product lists. Reduced the chunk size from 230 to 200 bytes to give proper headroom for message overhead, especially in shops with longer names.
- The remove (X) button on roster rows now works correctly. It removes the staff member locally, sends a WITHDRAW message to cancel any pending invite popup on their end, and sends a KICK so their client removes the shop immediately. Inviting a player who does not have Storekeeper installed no longer silently adds them to the roster. They receive a plain whisper explaining they need the addon, and the inviter gets a chat message confirming this. The player is not added to the roster until they have the addon and are invited again. A new WITHDRAW network message is sent when a staff member is removed before they have accepted their invite, dismissing the invite popup on their screen.
- When a kicked player logs in and the owner is online, the owner now sends a KICK automatically during the SYNC_PING handshake, cleaning up the shop on their end without any manual action required. When a player receives a SHOP_INFO roster update and they are not listed on it, they now automatically remove the shop from their local data with a clear chat message explaining why.
- SYNC_END, ORDER_END, and LOGS_END now wait for all chunks to arrive before processing the data. Previously, if a single message was dropped by WoW, the sync would process incomplete data and could result in corrupted products, orders, or logs. Duplicate chunk messages are now ignored so that receiving the same chunk twice does not cause a premature sync.
- Category colors were not being sent or received correctly in almost any scenario. BroadcastCategoryColors and SendCategoryColorsTo were reading from a field that was migrated away in v1.3.4 (shop.categories) and always sending an empty payload to staff. The SHOP_INFO and CAT_COLORS receive handlers were writing colors back into the same old field, meaning any colors received were immediately discarded on the next read. All four sites now correctly use shop.categoryColors.
- Added a 10-second cooldown to the Request Sync button to prevent accidental spamming. Orders and category colors are now staggered behind the product sync so they do not arrive on the receiving end before the product list has finished transmitting. The owner now checks whether a SYNC_PING sender is still on the roster before pushing data. Players who have been removed receive a KICK instead.

### v1.3.4
**Added**
- Cart stepper: each item in the cart now has a − button to decrease quantity by one, a quantity counter, a + button to add one more, and a separate × button to remove the line entirely.
- Active shop is now remembered per character. Each alt restores their own last-selected shop on login rather than sharing one account-wide value.
- Seller IC name is now saved per character. On first login it auto-populates from your TRP3 profile if one is set. A From TRP3 button in Settings lets you refresh it at any time.
- A mini-tutorial fires once on first login after updating, pointing out the new TRP3 Profile Text button on the Products tab.

**Bugs Fixed**
- Tooltip version badge not showing on other players — the HELLO whisper reply now uses the full Name-Realm address. Previously the realm suffix was stripped, causing silent delivery failure on connected realms.
- Tooltip version badge (TRP3) — the badge now resolves the hovered player from TRP3's own internal tooltip target instead of the mouseover unit token, which could point to a different player or return nil if the cursor had moved.
- Account-wide shop access — characters on the same account no longer inherit the active shop of a different character at login.
- Remove employee button unclickable — the button was calling an undefined function, causing a silent Lua error before the removal could run. Fixed.
- Manager window spawning off-centre — the window was offset 300px to the right of screen centre on open.
- Seller IC name bleeding across characters — new characters were being seeded with the IC name of whichever character last saved the setting.
- Shop Overview and Staff tabs misaligned — content in these two tabs sat flush against the left edge of the window instead of matching the consistent padding used by all other tabs.
- TOC: numeric CurseForge project ID — X-Curse-Project-ID was set to a text slug instead of the required numeric ID.
- TOC: file path separators — module paths now use forward slashes as required.

### v1.3.3
**Added**
- TRP3 Profile Text: a new button on the Products tab generates ready-to-paste TRP3 markup for your character's About tab, no separate addon required. A settings panel lets you customise shop title size, category title size, product name size, alignment, category colours, and whether out-of-stock products are included.

**Changed**
- Network: A full sync request (/sk sync) now also pushes category colours and category order alongside products and orders.
- Code cleanup: Removed an internal BroadcastToStaff function that was defined but never called. Removed the dead LOGS_DATA send path that had no corresponding receiver.

The previous version mentioned another addon in the makings. A separate one that shows a catalogue. But due to no interest this project has been put on hold. Therefore the TRP3 feature is released.

### v1.3.2
**Bug Fixes**
- Network: Empty log syncs now correctly use the LOGS_START / LOGS_END protocol instead of the unhandled LOGS_DATA command, which was silently dropped by receivers.
- Network: Category colours and category order were not being pushed to staff members whose products were already up to date when they came online. They now always receive colours on login sync.

**Added**
- What's New auto-open: The register now opens automatically to the What's New tab the first time you log in after installing a new version, so you never miss what changed.
- Community links: The What's New tab now has three quick-access buttons — Discord, CurseForge, and GitHub. Click any of them to copy the URL to your clipboard.

**Changed**
- Network: A full sync request (/sk sync) now also pushes category colours and category order alongside products and orders.
- Code cleanup: Removed an internal BroadcastToStaff function that was defined but never called. Removed the dead LOGS_DATA send path that had no corresponding receiver.

### v1.3.1
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

### v1.3.0
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
