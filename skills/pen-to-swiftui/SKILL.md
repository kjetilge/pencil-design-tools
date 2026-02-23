---
name: swiftui
description: Converts Pencil.dev (.pen) designs to SwiftUI code. Reads .pen file structure and generates native SwiftUI Views with proper layout, styling, and component hierarchy. Use when you have a .pen mockup and need SwiftUI implementation.
---

# Pencil to SwiftUI Converter

Converts .pen design files to native SwiftUI code with accurate layout and styling.

## When to Use This Skill

Activate when user:
- Has a .pen mockup and wants SwiftUI code
- Says "convert to SwiftUI", "generate Swift", "implement in iOS"
- Needs to go from design to code for an iOS/macOS app
- Wants to sync a Pencil design to a Swift project

## Core Mapping: .pen → SwiftUI

### Layout Containers

| .pen | SwiftUI |
|------|---------|
| `frame` + `layout: "vertical"` | `VStack(spacing: gap)` |
| `frame` + `layout: "horizontal"` | `HStack(spacing: gap)` |
| `frame` + `layout: "none"` | `ZStack` |
| `group` | `Group` |

### Sizing

| .pen | SwiftUI |
|------|---------|
| `fill_container` | `.frame(maxWidth: .infinity)` or `maxHeight: .infinity` |
| `fit_content` | (default SwiftUI behavior) |
| `width: 100` | `.frame(width: 100)` |
| `height: 200` | `.frame(height: 200)` |

### Alignment

| .pen | SwiftUI |
|------|---------|
| `justifyContent: "start"` | `alignment: .leading` (HStack) / `.top` (VStack) |
| `justifyContent: "center"` | `alignment: .center` |
| `justifyContent: "end"` | `alignment: .trailing` (HStack) / `.bottom` (VStack) |
| `justifyContent: "space_between"` | `Spacer()` between items |
| `alignItems: "start"` | Cross-axis alignment |
| `alignItems: "center"` | Cross-axis center |

### Basic Elements

| .pen | SwiftUI |
|------|---------|
| `text` | `Text("content")` |
| `rectangle` | `Rectangle()` or `RoundedRectangle(cornerRadius:)` |
| `ellipse` | `Circle()` or `Ellipse()` |
| `line` | `Divider()` or custom `Path` |
| `path` | `Path { }` with geometry |

### Icons

| .pen | SwiftUI |
|------|---------|
| `icon_font` (lucide/feather) | `Image(systemName: "sf.symbol.name")` |
| Custom mapping needed | See SF Symbols mapping table below |

### Styling

| .pen | SwiftUI |
|------|---------|
| `fill: "#RRGGBB"` | `.background(Color(hex: "RRGGBB"))` |
| `fill: "$variable"` | `.background(Color.theme.variableName)` |
| `stroke` | `.overlay(RoundedRectangle().stroke())` |
| `cornerRadius: 8` | `.clipShape(RoundedRectangle(cornerRadius: 8))` |
| `padding: 16` | `.padding(16)` |
| `padding: [8, 16]` | `.padding(.vertical, 8).padding(.horizontal, 16)` |
| `opacity: 0.5` | `.opacity(0.5)` |
| `effect: shadow` | `.shadow(color:radius:x:y:)` |

### Text Styling

| .pen | SwiftUI |
|------|---------|
| `fontSize: 24` | `.font(.system(size: 24))` |
| `fontWeight: "bold"` | `.fontWeight(.bold)` |
| `fontFamily` | `.font(.custom("FontName", size:))` |
| `textAlign: "center"` | `.multilineTextAlignment(.center)` |
| `fill` on text | `.foregroundColor(Color(...))` |

### Components (ref)

| .pen | SwiftUI |
|------|---------|
| `ref` to reusable component | Custom `View` struct |
| `descendants` overrides | View parameters/bindings |

## Mandatory Workflow

### 1. Read the Design

```
mcp__pencil__get_editor_state() - Get current .pen file
mcp__pencil__batch_get({nodeIds: ["targetFrameId"], readDepth: 10}) - Full structure
mcp__pencil__get_variables() - Get design tokens/colors
```

### 2. Analyze Structure

- Identify reusable components (nodes with `reusable: true`)
- Map component hierarchy to View hierarchy
- Extract color/spacing variables for theme

### 3. Generate SwiftUI Code

#### Theme/Colors First

```swift
// Theme.swift
import SwiftUI

extension Color {
    static let theme = ColorTheme()
}

struct ColorTheme {
    let primary = Color(hex: "007AFF")
    let background = Color(hex: "F5F5F5")
    let textPrimary = Color(hex: "000000")
    // ... from .pen variables
}
```

#### Components as Views

```swift
// ButtonPrimary.swift (from reusable component)
struct ButtonPrimary: View {
    let title: String
    let action: () -> Void

    var body: some View {
        Button(action: action) {
            Text(title)
                .font(.system(size: 16, weight: .semibold))
                .foregroundColor(.white)
                .frame(maxWidth: .infinity)
                .padding(.vertical, 16)
                .background(Color.theme.primary)
                .clipShape(RoundedRectangle(cornerRadius: 12))
        }
    }
}
```

#### Screen as View

```swift
// LoginView.swift (from frame)
struct LoginView: View {
    @State private var email = ""
    @State private var password = ""

    var body: some View {
        VStack(spacing: 24) {
            // Header
            Text("Welcome Back")
                .font(.system(size: 28, weight: .bold))
                .foregroundColor(.theme.textPrimary)

            // Form fields
            VStack(spacing: 16) {
                TextField("Email", text: $email)
                    .textFieldStyle(.roundedBorder)

                SecureField("Password", text: $password)
                    .textFieldStyle(.roundedBorder)
            }

            // Button
            ButtonPrimary(title: "Sign In") {
                // Action
            }

            Spacer()
        }
        .padding(24)
        .background(Color.theme.background)
    }
}
```

### 4. Verify Visually

```
mcp__pencil__get_screenshot(nodeId) - Compare design to implementation
```

## SF Symbols Mapping

Common Lucide/Feather → SF Symbols:

| Lucide/Feather | SF Symbol |
|----------------|-----------|
| `arrow-left` | `chevron.left` |
| `arrow-right` | `chevron.right` |
| `check` | `checkmark` |
| `x` | `xmark` |
| `menu` | `line.3.horizontal` |
| `search` | `magnifyingglass` |
| `settings` | `gearshape` |
| `user` | `person` |
| `home` | `house` |
| `mail` | `envelope` |
| `bell` | `bell` |
| `heart` | `heart` |
| `star` | `star` |
| `plus` | `plus` |
| `minus` | `minus` |
| `edit` | `pencil` |
| `trash` | `trash` |
| `share` | `square.and.arrow.up` |
| `download` | `arrow.down.circle` |
| `upload` | `arrow.up.circle` |
| `camera` | `camera` |
| `image` | `photo` |
| `lock` | `lock` |
| `unlock` | `lock.open` |
| `eye` | `eye` |
| `eye-off` | `eye.slash` |

## Color Hex Extension

Include this helper in your Swift project:

```swift
extension Color {
    init(hex: String) {
        let hex = hex.trimmingCharacters(in: CharacterSet.alphanumerics.inverted)
        var int: UInt64 = 0
        Scanner(string: hex).scanHexInt64(&int)
        let a, r, g, b: UInt64
        switch hex.count {
        case 3:
            (a, r, g, b) = (255, (int >> 8) * 17, (int >> 4 & 0xF) * 17, (int & 0xF) * 17)
        case 6:
            (a, r, g, b) = (255, int >> 16, int >> 8 & 0xFF, int & 0xFF)
        case 8:
            (a, r, g, b) = (int >> 24, int >> 16 & 0xFF, int >> 8 & 0xFF, int & 0xFF)
        default:
            (a, r, g, b) = (1, 1, 1, 0)
        }
        self.init(
            .sRGB,
            red: Double(r) / 255,
            green: Double(g) / 255,
            blue: Double(b) / 255,
            opacity: Double(a) / 255
        )
    }
}
```

## Output Structure

```
Sources/
  Theme/
    ColorTheme.swift      # From .pen variables
    Typography.swift      # Font styles
  Components/
    ButtonPrimary.swift   # From reusable components
    InputField.swift
    Card.swift
  Views/
    LoginView.swift       # From frames/screens
    DashboardView.swift
```

## Best Practices

1. **Extract components first** - Convert reusable nodes to View structs
2. **Use SwiftUI idioms** - Don't force web patterns into SwiftUI
3. **Leverage @State/@Binding** - For interactive components
4. **Use native controls** - Prefer `TextField`, `Button` over custom when possible
5. **Test on device** - Screenshots can't capture interaction feel
6. **Consider dark mode** - Map variables to support both themes

## Limitations

- Complex SVG paths need manual conversion
- Animations are not captured in .pen
- Some gradient types may need adjustment
- Custom fonts need to be bundled separately
