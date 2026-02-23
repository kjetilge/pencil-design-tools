# Pencil Design Tools

AI-powered design tools for Claude Code and Claude Cowork.

**Supports all Apple platforms:** iOS, macOS, iPadOS, watchOS, tvOS, visionOS

---

## Tutorial: Building a macOS App Mockup

This tutorial shows how to iteratively build a professional macOS app mockup using natural language commands in Claude Cowork.

### Example: Video Encoder App

We'll build a video encoder app with sidebar navigation, queue list, and encoding progress.

---

### Step 1: Create the Window Shell

```
/pencil-design-tools:mockup macOS window with title "Video Encoder" - just the basic window frame with traffic lights
```

**Result:** A clean macOS window with:
- Traffic light buttons (red, yellow, green)
- Centered window title
- Empty content area ready for layout

```
┌─────────────────────────────────────────────┐
│ ● ● ●         Video Encoder                 │
├─────────────────────────────────────────────┤
│                                             │
│                                             │
│           Empty content area                │
│                                             │
│                                             │
└─────────────────────────────────────────────┘
```

---

### Step 2: Add Sidebar Navigation

```
/pencil-design-tools:mockup Add sidebar with navigation: Queue (selected), Completed, Settings
```

**Result:** Sidebar with macOS-style navigation:
- Section header "Library"
- Selected item with blue background
- Secondary items in standard text

```
┌─────────────────────────────────────────────┐
│ ● ● ●         Video Encoder                 │
├──────────┬──────────────────────────────────┤
│ Library  │                                  │
│ ┌──────┐ │                                  │
│ │Queue │ │        Content area              │
│ └──────┘ │                                  │
│ Completed│                                  │
│ Settings │                                  │
└──────────┴──────────────────────────────────┘
```

---

### Step 3: Add Content List

```
/pencil-design-tools:mockup Add encoding queue with two video items: interview_final.mp4 (1.2GB, encoding at 60%) and product_demo.mov (890MB, queued)
```

**Result:** Content area with video list:
- Header with item count badge
- Video cards with thumbnail, filename, size
- Progress indicator for active encoding
- Status badge for queued items

```
┌─────────────────────────────────────────────┐
│ ● ● ●         Video Encoder                 │
├──────────┬──────────────────────────────────┤
│ Library  │ Encoding Queue          2 items  │
│ ┌──────┐ │ ┌─────────────────────────────┐  │
│ │Queue │ │ │ ▶ interview_final.mp4  60%  │  │
│ └──────┘ │ │   1.2 GB • H.264 → HEVC ━━━ │  │
│ Completed│ └─────────────────────────────┘  │
│ Settings │ ┌─────────────────────────────┐  │
│          │ │   product_demo.mov   Queued │  │
│          │ │   890 MB • ProRes → HEVC    │  │
│          │ └─────────────────────────────┘  │
└──────────┴──────────────────────────────────┘
```

---

### Step 4: Add Toolbar Actions

```
/pencil-design-tools:mockup Add toolbar with + button and "Start All" primary button
```

**Result:** Complete app with toolbar:
- Add button (+) for importing videos
- Primary action button "Start All"
- Polished, production-ready mockup

```
┌─────────────────────────────────────────────┐
│ ● ● ●      Video Encoder      [+] [Start All]│
├──────────┬──────────────────────────────────┤
│ Library  │ Encoding Queue          2 items  │
│ ┌──────┐ │ ┌─────────────────────────────┐  │
│ │Queue │ │ │ ▶ interview_final.mp4  60%  │  │
│ └──────┘ │ │   1.2 GB • H.264 → HEVC ━━━ │  │
│ Completed│ └─────────────────────────────┘  │
│ Settings │ ┌─────────────────────────────┐  │
│          │ │   product_demo.mov   Queued │  │
│          │ │   890 MB • ProRes → HEVC    │  │
│          │ └─────────────────────────────┘  │
└──────────┴──────────────────────────────────┘
```

---

### Step 5: Validate & Generate Code

```
/pencil-design-tools:ux-review
```

**UX Review output:**
- ✅ Click targets meet 24pt minimum (macOS)
- ✅ Sidebar width appropriate (180pt)
- ✅ Visual hierarchy clear
- ✅ Primary action prominently placed

```
/pencil-design-tools:swiftui
```

**Generates SwiftUI code:**

```swift
struct VideoEncoderView: View {
    @State private var selectedNav = "Queue"

    var body: some View {
        NavigationSplitView {
            List(selection: $selectedNav) {
                Section("Library") {
                    Label("Queue", systemImage: "list.bullet")
                    Label("Completed", systemImage: "checkmark.circle")
                    Label("Settings", systemImage: "gear")
                }
            }
        } detail: {
            EncodingQueueView()
        }
        .toolbar {
            ToolbarItem(placement: .primaryAction) {
                Button("Start All") { }
            }
            ToolbarItem {
                Button(action: {}) {
                    Image(systemName: "plus")
                }
            }
        }
    }
}
```

---

## Available Skills

| Skill | Command | Description |
|-------|---------|-------------|
| **mockup** | `/pencil-design-tools:mockup` | Create UI mockups from natural language |
| **swiftui** | `/pencil-design-tools:swiftui` | Convert Pencil designs to SwiftUI |
| **ux-review** | `/pencil-design-tools:ux-review` | Validate against Apple HIG |
| **swift** | `/pencil-design-tools:swift` | Direct SwiftUI generation (no mockup) |
| **design** | `/pencil-design-tools:design` | Combined mockup + SwiftUI workflow |

---

## Example Commands

### macOS

```
/pencil-design-tools:mockup macOS settings window with sidebar navigation
/pencil-design-tools:mockup Document-based app with toolbar and inspector
/pencil-design-tools:mockup Menu bar app with dropdown panel
```

### iOS

```
/pencil-design-tools:mockup iOS login screen with social auth
/pencil-design-tools:mockup Tab bar app with 4 sections
/pencil-design-tools:mockup Settings page with grouped rows
```

### visionOS

```
/pencil-design-tools:mockup Immersive media player with ornament controls
/pencil-design-tools:mockup Floating window with glass material
```

---

## Installation

### From GitHub

```bash
git clone https://github.com/kjetilge/pencil-design-tools.git ~/Developer/claude-plugins/pencil-design-tools
```

### Add to Claude Code

```bash
claude --plugin-dir ~/Developer/claude-plugins/pencil-design-tools
```

### Or symlink for Cowork

```bash
ln -sf ~/Developer/claude-plugins/pencil-design-tools ~/.claude/plugins/pencil-design-tools
```

---

## Requirements

- **Pencil.dev** app installed and running
- **Pencil MCP** configured in Claude settings

### MCP Configuration

```json
{
  "mcpServers": {
    "pencil": {
      "command": "npx",
      "args": ["-y", "@anthropic/pencil-mcp"]
    }
  }
}
```

---

## .pen → SwiftUI Mapping

| Pencil | SwiftUI |
|--------|---------|
| `frame` + `layout: "vertical"` | `VStack` |
| `frame` + `layout: "horizontal"` | `HStack` |
| `fill_container` | `.frame(maxWidth: .infinity)` |
| `cornerRadius` | `.clipShape(RoundedRectangle)` |
| `padding` | `.padding()` |
| `gap` | `spacing:` |

---

## License

MIT

## Author

**Kjetil Geirbo** · kjetil@geirbo.no · [@kjetilge](https://github.com/kjetilge)
