# Torre Mind VLMS - Flow Analysis & Screen Mapping

**Document Purpose:** Map business flows to existing screens in torremind.pen and identify gaps.

**Last Updated:** 2026-03-26

---

## Flow A — Pre-Registered Visitor (Invited Guest)

**Business Description:** Gold-standard experience for scheduled meetings. Tenant invites via WhatsApp, visitor receives QR code, arrives and checks in seamlessly.

### Flow Steps & Screen Mapping:

| Step | Description | Screen Type | Screen Name | Screen ID | Status |
|------|-------------|-------------|-------------|-----------|--------|
| 1 | Tenant initiates invitation via WhatsApp | WhatsApp | Case: Tenant Registers a Guest | `2ZfZA` | ✅ Exists |
| 2 | System sends visitor WhatsApp with QR code | WhatsApp | Case: Pre-Registered Visitor QR Invitation | `Dts7e` | ✅ Exists |
| **3a** | **Path A: Visitor shows QR to lobby staff** | **Lobby Kiosk** | **Lobby - QR Scan** | **`fBy7S`** | **✅ Exists** |
| **3b-1** | **Path B: Visitor scans printed QR poster at entrance** | **Physical Poster** | **Printed QR Banner - Entrance** | **`P4AxW`** | **✅ Exists** |
| **3b-2** | **Path B: Mobile web page opens asking for phone number** | **Mobile Web** | **Mobile: Phone Capture Screen** | **`vZ2UY`** | **✅ Exists** |
| **3b-3** | **Path B: WhatsApp opens with check-in confirmation button** | **WhatsApp** | **Pre-Registered Entrance QR - Tap to Confirm** | **`fPemZ`** | **✅ Exists** |
| 4 | System confirms scan via WhatsApp | WhatsApp | Case: Pre-Registered QR Scan Confirmation | `UY36a` | ✅ Exists |
| 5 | Staff dashboard updates with visitor | Dashboard | Staff Dashboard | `CWDOp` | ✅ Exists |
| 6 | Badge/pass confirmation shown | Lobby Kiosk | Lobby - Badge Confirmation | `Z7pa7` | ✅ Exists |
| 7 | Host receives WhatsApp notification | WhatsApp | Case: Host Approval for Walk-In Visitor | `huHVL` | ⚠️ Reusable |

**Status:** ✅ **COMPLETE** - All required screens exist including both check-in paths

**Notes:**
- **Two check-in paths available:**
  - **Path A:** Visitor shows QR code to lobby staff at kiosk (step 3a)
  - **Path B:** Visitor scans printed QR poster at entrance (steps 3b-1 → 3b-2 → 3b-3):
    1. Scans entrance QR poster with phone camera
    2. Mobile web page opens asking for phone number verification
    3. WhatsApp opens with personalized check-in confirmation button
- Both paths converge at step 4 (WhatsApp confirmation receipt)
- Host approval WhatsApp screen exists but is named for walk-in flow (can be reused)
- Path B enables contactless check-in without staff interaction

---

## Flow B — Walk-In / Unannounced Visitor

**Business Description:** Handles visitors arriving without prior arrangement. Staff manually registers and notifies host.

### Flow Steps & Screen Mapping:

| Step | Description | Screen Type | Screen Name | Screen ID | Status |
|------|-------------|-------------|-------------|-----------|--------|
| 1 | Visitor arrives at lobby | Lobby Kiosk | Lobby - Welcome | `2lIF7` | ✅ Exists |
| 2 | Staff opens check-in interface | Dashboard | Check-In Form | `3d2hE` | ✅ Exists |
| 3 | Staff searches for returning visitor OR enters new visitor info | Dashboard | Check-In Form (with search) | `3d2hE` | ✅ Exists |
| 4 | Staff selects visitor type (method selection) | Lobby Kiosk | Lobby - Method Selection | `FJEZG` | ✅ Exists |
| 5 | Staff enters visitor details if new | Lobby Kiosk | Lobby - Visitor Info Entry | `hR4jr` | ✅ Exists |
| 6 | Staff selects department/tenant to notify | Dashboard | Tenant Directory (for lookup) | `93YQ4` | ✅ Exists |
| 7 | System sends WhatsApp to host | WhatsApp | Case: Host Approval for Walk-In Visitor | `huHVL` | ✅ Exists |
| 8 | Staff sees response on dashboard | Dashboard | Staff Dashboard | `CWDOp` | ✅ Exists |
| 9 | Host doesn't respond - escalation | WhatsApp | Case: Host No-Response Escalation | `NElml` | ✅ Exists |
| 10 | Visitor check-in logged | Dashboard | Visitor Log | `zrVM4` | ✅ Exists |

**Status:** ✅ **COMPLETE** - All required screens exist

**Notes:**
- Check-In Form (`3d2hE`) serves as the primary staff interface for this flow
- Frequent Visitor screen (`4YA2p`) provides quick lookup for returning visitors

---

## Flow C — Delivery / Package Arrival

**Business Description:** Fast-track flow for high-frequency deliveries.

### Flow Steps & Screen Mapping:

| Step | Description | Screen Type | Screen Name | Screen ID | Status |
|------|-------------|-------------|-------------|-----------|--------|
| 1 | Staff selects delivery mode | Dashboard | Check-In Form (delivery mode) | `3d2hE` | ⚠️ Partial |
| 2 | Staff enters courier info | Dashboard | **MISSING: Delivery Entry Form** | - | ❌ Missing |
| 3 | System sends WhatsApp to recipient | WhatsApp | Case: Delivery Arrival Notification | `IoyJK` | ✅ Exists |
| 4 | Delivery logged with timestamp | Dashboard | Visitor Log (or dedicated delivery log) | `zrVM4` | ⚠️ Partial |
| 5 | Staff marks as collected | Dashboard | **MISSING: Delivery Status Update** | - | ❌ Missing |

**Status:** ⚠️ **PARTIAL** - Missing dedicated delivery interface

**Gaps Identified:**
1. **Delivery Entry Form** - Simplified form optimized for package data (courier, tracking, description)
2. **Delivery Status Dashboard** - Track packages pending pickup vs. collected
3. **Barcode Scanner Interface** - Quick scan shipping labels

**Workaround:** Can use existing Check-In Form with custom visitor type "Delivery"

---

## Flow D — Recurring Vendor or Contractor

**Business Description:** Fast check-in for pre-registered vendors with recurring access.

### Flow Steps & Screen Mapping:

| Step | Description | Screen Type | Screen Name | Screen ID | Status |
|------|-------------|-------------|-------------|-----------|--------|
| 1 | Vendor arrives | Lobby Kiosk | Lobby - Welcome | `2lIF7` | ✅ Exists |
| 2 | Staff searches vendor by name/company | Lobby Kiosk | Lobby - Frequent Visitor | `4YA2p` | ✅ Exists |
| 3 | System auto-fills vendor record | Dashboard | Check-In Form (pre-filled) | `3d2hE` | ⚠️ Partial |
| 4 | Staff confirms service type and area | Dashboard | **MISSING: Vendor Access Control** | - | ❌ Missing |
| 5 | Vendor check-in logged | Dashboard | Visitor Log | `zrVM4` | ✅ Exists |
| 6 | Vendor check-out logged | Dashboard | Exit - Search & Checkout | `FB5Er` | ✅ Exists |

**Status:** ⚠️ **PARTIAL** - Missing vendor-specific fields

**Gaps Identified:**
1. **Vendor Access Control Interface** - Specify floors/zones, work order reference, service type
2. **Recurring Vendor Registry** - Pre-register vendors with company, schedule, clearances
3. **Vendor Badge Type** - Distinct visual badge for contractors vs. visitors

**Workaround:** Can use Frequent Visitor + Check-In Form with manual notes

---

## Flow E — WhatsApp Self-Service Check-In

**Business Description:** Visitor-initiated check-in via WhatsApp (no kiosk hardware required).

### Flow Steps & Screen Mapping:

| Step | Description | Screen Type | Screen Name | Screen ID | Status |
|------|-------------|-------------|-------------|-----------|--------|
| 1 | Visitor scans QR placard at entrance | WhatsApp | Case: Walk-In Visitor Self Check-In | `RgJ3N` | ✅ Exists |
| 2a | Pre-registered: Scan sends auto-confirmation | WhatsApp | Case: Pre-Registered QR Scan Confirmation | `UY36a` | ✅ Exists |
| 2b | Walk-in: WhatsApp bot prompts for info | WhatsApp | Case: Walk-In Visitor Self Check-In | `RgJ3N` | ✅ Exists |
| 3 | System notifies host via WhatsApp | WhatsApp | Case: Host Approval for Walk-In Visitor | `huHVL` | ✅ Exists |
| 4 | Visitor receives confirmation | WhatsApp | Case: Pre-Registered QR Scan Confirmation | `UY36a` | ✅ Exists |
| 5 | Staff dashboard updates in real-time | Dashboard | Staff Dashboard | `CWDOp` | ✅ Exists |

**Status:** ✅ **COMPLETE** - All required screens exist

**Notes:**
- This flow leverages existing WhatsApp screens
- Minimal lobby staff intervention required
- Dashboard provides override capability

---

## Additional Screens (Not Mapped to Flows A-E)

These screens exist but aren't explicitly part of flows A-E:

| Screen Name | Screen ID | Purpose | Used In |
|-------------|-----------|---------|---------|
| Lobby - Welcome | `2lIF7` | Entry point for kiosk | Multiple flows |
| Lobby - Method Selection | `FJEZG` | Choose check-in method | Flow B |
| Lobby - Visitor Info Entry | `hR4jr` | Manual data entry | Flow B |
| Lobby - Department Selection | `nE6UY` | Select which dept to visit | Flow B variation |
| Lobby - Motive Selection | `SJIyt` | Choose visit reason | Flow B variation |
| Lobby - Signature | `gy67C` | Digital signature capture | Optional compliance |
| Exit - Welcome | `A1SRI` | Checkout welcome | All flows (exit) |
| Exit - Search & Checkout | `FB5Er` | Complete visitor checkout | All flows (exit) |
| Admin - Login | `EPAKj` | Staff authentication | System access |
| Case: Blocklist Match Security Alert | `jD5Kl` | Security notification | Edge case |

---

## Summary: Missing Screens by Priority

### Priority 1 (Critical for Flow C)
1. **Delivery Entry Form** - Simplified interface for package logging
2. **Delivery Status Dashboard** - Track pending/collected packages

### Priority 2 (Enhance Flow D)
3. **Vendor Access Control Interface** - Floor/zone permissions, work orders
4. **Recurring Vendor Registry** - Pre-registration management

### Priority 3 (Nice to Have)
5. **Barcode Scanner Overlay** - For shipping labels and visitor QR codes
6. **Real-time Notification Panel** - Staff dashboard alert center
7. **Visitor Photo Capture** - For badge printing

---

## Screen Statistics

- **Total Screens in torremind.pen:** 24
- **Screens Mapped to Flows:** 20
- **Missing Critical Screens:** 2 (Delivery forms)
- **Missing Enhancement Screens:** 5

**Overall Coverage:** ~90% of flows can be demonstrated with existing screens

---

## Recommendations for Demo Presentation

1. **Flow A (Pre-Registered):** Fully demonstrable - best flow to start
2. **Flow E (WhatsApp Self-Service):** Fully demonstrable - showcase innovation
3. **Flow B (Walk-In):** Fully demonstrable - showcase staff workflow
4. **Flow D (Recurring Vendor):** Demonstrable with notes about upcoming features
5. **Flow C (Delivery):** Demonstrate conceptually, mention in-development delivery module

**Suggested Demo Order:** A → E → B → (show exit flow) → D → C
