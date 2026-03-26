# Demo System Onboarding - Technical Reference

## Overview

This demo system maps business flows (A-E) to screens in `torremind.pen` and presents them in an interactive HTML interface.

## File Structure

```
demopage/
├── demo-navigation.html          # Interactive split-screen demo
├── FLOW_ANALYSIS.md              # Flow mapping documentation
├── README.md                     # Usage guide
├── screenshots/                  # Exported screen images (24 PNG files)
│   ├── 2ZfZA.png                # WhatsApp screens
│   ├── CWDOp.png                # Dashboard screens
│   ├── fBy7S.png                # Kiosk screens
│   └── ...
└── ONBOARDING.md                 # This file
```

## Design Source

**Original file:** `/home/x/Downloads/torremind.pen`

All screens are designed in this Pencil file and can only be read/modified using the Pencil MCP tools.

## Screen ID Reference

### WhatsApp Flows (375px wide, 812px tall)
| Screen ID | Screen Name | Used In Flow |
|-----------|-------------|--------------|
| `2ZfZA` | Case: Tenant Registers a Guest | Flow A, step 1 |
| `Dts7e` | Case: Pre-Registered Visitor QR Invitation | Flow A, step 2 |
| `P4AxW` | Printed QR Banner - Entrance | Flow A, step 3b-1 (800×400) |
| `vZ2UY` | Mobile: Phone Capture Screen | Flow A, step 3b-2 |
| `fPemZ` | Case: Pre-Registered Entrance QR - Tap to Confirm | Flow A, step 3b-3 |
| `UY36a` | Case: Pre-Registered QR Scan Confirmation | Flow A, step 4; Flow E, steps 2a & 4 |
| `RgJ3N` | Case: Walk-In Visitor Self Check-In | Flow E, steps 1 & 2b |
| `huHVL` | Case: Host Approval for Walk-In Visitor | Flow A, step 7; Flow B, step 7; Flow E, step 3 |
| `NElml` | Case: Host No-Response Escalation | Flow B, step 9 |
| `IoyJK` | Case: Delivery Arrival Notification | (Flow C - not in current demo) |
| `jD5Kl` | Case: Blocklist Match Security Alert | (Edge case - not in main flows) |

### Dashboard Screens (1280px wide, 800px tall)
| Screen ID | Screen Name | Used In Flow |
|-----------|-------------|--------------|
| `CWDOp` | Staff Dashboard | Flow A, step 5; Flow B, step 8; Flow E, step 4 |
| `3d2hE` | Check-In Form | Flow B, step 2 |
| `93YQ4` | Tenant Directory | Flow B, step 6 |
| `zrVM4` | Visitor Log | Flow B, step 10; Exit Flow, step 3 |
| `FB5Er` | Exit - Search & Checkout | Exit Flow, step 2 |
| `EPAKj` | Admin - Login | (System screen - not in flows) |

### Lobby Kiosk Screens (1280px wide, 800px tall)
| Screen ID | Screen Name | Used In Flow |
|-----------|-------------|--------------|
| `2lIF7` | Lobby - Welcome | Flow B, step 1 |
| `FJEZG` | Lobby - Method Selection | Flow B, step 4 |
| `hR4jr` | Lobby - Visitor Info Entry | Flow B, step 5 |
| `Z7pa7` | Lobby - Badge Confirmation | Flow A, step 6 |
| `nE6UY` | Lobby - Department Selection | (Optional - not in main flows) |
| `SJIyt` | Lobby - Motive Selection | (Optional - not in main flows) |
| `gy67C` | Lobby - Signature | (Optional - not in main flows) |
| `fBy7S` | Lobby - QR Scan | Flow A, step 3 |
| `4YA2p` | Lobby - Frequent Visitor | Flow B, step 3 |
| `A1SRI` | Exit - Welcome | Exit Flow, step 1 |

## How to Modify Screens

### Step 1: Open the Pencil file

Use Pencil MCP tools to access torremind.pen:

```javascript
// Get current editor state
mcp__pencil__get_editor_state({ include_schema: false })

// If not open, open the file
mcp__pencil__open_document({ filePathOrTemplate: "/home/x/Downloads/torremind.pen" })
```

### Step 2: Read the screen you want to modify

```javascript
// Read a specific screen by ID with its children
mcp__pencil__batch_get({
  filePath: "/home/x/Downloads/torremind.pen",
  nodeIds: ["CWDOp"],  // Example: Staff Dashboard
  readDepth: 3         // Adjust depth based on how deep you need to go
})
```

### Step 3: Modify the screen

Use `mcp__pencil__batch_design` with operation syntax:

```javascript
// Example: Update text content
U("CWDOp/someTextNodeId", { content: "New text here" })

// Example: Change colors
U("CWDOp/someFrameId", { fill: "#0EA5E9" })

// Example: Add a new element
newBtn=I("CWDOp", { type: "frame", width: 200, height: 50, fill: "#10B981" })
```

**Important:** Follow the operation syntax in the Pencil MCP tool description:
- `I(parent, {...})` - Insert new node
- `U(nodeId, {...})` - Update existing node
- `C(nodeId, parent, {...})` - Copy node
- `R(nodeId, {...})` - Replace node
- `D(nodeId)` - Delete node
- `M(nodeId, parent, index)` - Move node

### Step 4: Re-export the modified screen

```javascript
mcp__pencil__export_nodes({
  filePath: "/home/x/Downloads/torremind.pen",
  outputDir: "/home/x/sources/mind_VLMS/demopage/screenshots",
  nodeIds: ["CWDOp"],  // The screen you modified
  format: "png",
  scale: 2
})
```

This will overwrite the existing PNG in `screenshots/` and the demo will automatically show the updated version.

### Step 5: Verify in demo

Open `demo-navigation.html` and click the flow step that uses that screen. The new version should appear.

## How to Add a New Screen

### Step 1: Design the screen in torremind.pen

Use `mcp__pencil__batch_design` to create the new screen:

```javascript
// Example: Create new delivery form screen
deliveryForm=I(document, {
  type: "frame",
  name: "Delivery Entry Form",
  width: 1280,
  height: 800,
  fill: { /* gradient or color */ }
})

// Add child elements
header=I(deliveryForm, { type: "text", content: "Package Delivery", fontSize: 24 })
// ... continue building the screen
```

Note the screen ID returned (e.g., `newScreenId123`).

### Step 2: Export the new screen

```javascript
mcp__pencil__export_nodes({
  filePath: "/home/x/Downloads/torremind.pen",
  outputDir: "/home/x/sources/mind_VLMS/demopage/screenshots",
  nodeIds: ["newScreenId123"],
  format: "png",
  scale: 2
})
```

### Step 3: Add to demo-navigation.html

Find the appropriate flow section and add a new step:

```html
<div class="screen-step" onclick="showScreen('newScreenId123', 'Delivery Entry Form', 'dashboard', this)">
    <span class="step-number">2</span>
    <div class="step-content">
        <div class="step-title">Enter Package Details</div>
        <div class="step-description">Staff logs courier and tracking info</div>
    </div>
    <div class="step-meta">
        <span class="screen-type type-dashboard">Dashboard</span>
        <span class="status-icon">✓</span>
    </div>
</div>
```

### Step 4: Update FLOW_ANALYSIS.md

Add the new screen to the appropriate flow table and update the status.

## How to Add a New Flow

### Step 1: Design all screens for the flow

Create the screens in torremind.pen using the Pencil MCP tools.

### Step 2: Export all screens

```javascript
mcp__pencil__export_nodes({
  filePath: "/home/x/Downloads/torremind.pen",
  outputDir: "/home/x/sources/mind_VLMS/demopage/screenshots",
  nodeIds: ["screen1", "screen2", "screen3"],
  format: "png",
  scale: 2
})
```

### Step 3: Add flow section to demo-navigation.html

Copy an existing flow section and modify:

```html
<!-- Flow F -->
<div class="flow-section">
    <div class="flow-header" onclick="toggleFlow('flowF')">
        <span class="flow-badge">FLOW F</span>
        <h2 class="flow-title">New Flow Name</h2>
        <span class="status-badge status-complete">✓</span>
        <button class="expand-toggle" id="toggleF">▼</button>
    </div>
    <p class="flow-description">
        Description of what this flow does...
    </p>
    <div class="screen-sequence" id="flowF">
        <!-- Add screen steps here -->
    </div>
</div>
```

### Step 4: Document in FLOW_ANALYSIS.md

Add a new section following the existing format.

## Common Modifications

### Change screen title or description in demo

Edit `demo-navigation.html`, find the step, and modify:

```html
<div class="step-title">NEW TITLE HERE</div>
<div class="step-description">NEW DESCRIPTION HERE</div>
```

### Change flow order

Cut and paste entire `<div class="flow-section">...</div>` blocks in `demo-navigation.html`.

### Hide a screen from demo (without deleting)

Add `style="display:none"` to the screen-step div:

```html
<div class="screen-step" style="display:none" onclick="...">
```

### Change screen type badge color

Modify the `screen-type` class:
- `type-dashboard` = purple (Dashboard)
- `type-kiosk` = cyan (Kiosk)
- `type-whatsapp` = green (WhatsApp)

### Re-export all screens

If you make bulk changes to torremind.pen:

```javascript
mcp__pencil__export_nodes({
  filePath: "/home/x/Downloads/torremind.pen",
  outputDir: "/home/x/sources/mind_VLMS/demopage/screenshots",
  nodeIds: [
    "CWDOp", "3d2hE", "93YQ4", "zrVM4", "2lIF7", "FJEZG",
    "hR4jr", "Z7pa7", "nE6UY", "SJIyt", "gy67C", "fBy7S",
    "4YA2p", "A1SRI", "FB5Er", "EPAKj", "2ZfZA", "Dts7e",
    "UY36a", "RgJ3N", "huHVL", "NElml", "IoyJK", "jD5Kl"
  ],
  format: "png",
  scale: 2
})
```

## Design System Reference

### Colors (Cyberpunk Theme)
- Primary: `#0EA5E9` (cyan)
- Background gradient: `#0B1628` → `#070B14` → `#050811`
- Text primary: `#E5E5E5`
- Text secondary: `#737373`
- Success: `#10B981`
- Warning: `#FBBf24`
- Error: `#EF4444`

### Typography
- Font family: Inter (dashboard), Outfit (kiosk)
- Monospace: JetBrains Mono (for IDs, code)

### Screen Dimensions
- Dashboard: 1280×800px
- Kiosk: 1280×800px
- WhatsApp: 375×812px (mobile)

## Troubleshooting

### Screenshot not showing in demo
1. Check filename matches screen ID: `screenshots/CWDOp.png`
2. Check file was exported with correct node ID
3. Try hard refresh (Ctrl+Shift+R) in browser

### Screen ID not found in torremind.pen
Use `mcp__pencil__batch_get` to list all screens:

```javascript
mcp__pencil__batch_get({
  filePath: "/home/x/Downloads/torremind.pen",
  patterns: [{ type: "frame" }],
  readDepth: 1,
  searchDepth: 2
})
```

### Modified screen not updating in demo
1. Make sure you re-exported the PNG after modifying
2. Clear browser cache
3. Check the PNG file timestamp: `ls -lh demopage/screenshots/`

### Need to see screen hierarchy
Use `mcp__pencil__snapshot_layout` to understand structure:

```javascript
mcp__pencil__snapshot_layout({
  filePath: "/home/x/Downloads/torremind.pen",
  parentId: "CWDOp",  // Optional: specific screen
  maxDepth: 3
})
```

## Quick Commands Reference

```bash
# List all screenshots
ls -lh demopage/screenshots/

# Check screenshot file sizes
du -h demopage/screenshots/*.png

# Open demo in browser (Linux)
xdg-open demopage/demo-navigation.html

# Count total screens
ls demopage/screenshots/ | wc -l
```

## Notes

- **Never use Read/Write tools on .pen files** - they're encrypted. Always use Pencil MCP tools.
- **Screen IDs are permanent** - they don't change unless you delete and recreate
- **Export at 2x scale** for retina/high-DPI displays
- **PNG format preferred** over JPEG for UI screenshots (no compression artifacts)
- The demo HTML has no dependencies - works offline once screenshots are exported
