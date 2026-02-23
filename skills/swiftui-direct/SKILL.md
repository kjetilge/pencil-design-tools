---
name: swift
description: Generates SwiftUI code directly from natural language descriptions without creating visual mockups first. Perfect for rapid prototyping when you already know what you want. Supports iOS, macOS, iPadOS, watchOS, tvOS, and visionOS.
---

# SwiftUI Direct Generator

Generates production-ready SwiftUI code directly from natural language descriptions, bypassing the visual mockup step.

## When to Use This Skill

Use when:
- You know exactly what you want and don't need to visualize first
- Rapid prototyping of components
- Generating boilerplate SwiftUI views
- You'll iterate in Xcode with live preview instead

**Use `/pencil-design-tools:mockup` instead when:**
- You need visual mockups for spec documents
- You want to iterate visually before coding
- Stakeholders need to review designs

## Usage

```
/pencil-design-tools:swift [description]
/pencil-design-tools:swift [description] --platform [ios|macos|ipados|watchos|tvos|visionos]
```

## Examples

```
/pencil-design-tools:swift Login form with email and password
/pencil-design-tools:swift macOS settings window with sidebar
/pencil-design-tools:swift Video card with thumbnail, title, and duration badge
/pencil-design-tools:swift Encoding progress view with queue list --platform macos
```

## Output Structure

### Single Component
```swift
// VideoCard.swift
struct VideoCard: View { ... }

#Preview {
    VideoCard(...)
}
```

### Screen with Multiple Components
```swift
// LoginView.swift
struct LoginView: View { ... }

// MARK: - Subviews
private struct EmailField: View { ... }
private struct PasswordField: View { ... }
private struct LoginButton: View { ... }

#Preview {
    LoginView()
}
```

## Platform Defaults

### iOS (Default)
```swift
struct ContentView: View {
    var body: some View {
        NavigationStack {
            // Content
        }
    }
}
```

### macOS
```swift
struct ContentView: View {
    var body: some View {
        NavigationSplitView {
            // Sidebar
        } detail: {
            // Detail
        }
    }
}
```

### watchOS
```swift
struct ContentView: View {
    var body: some View {
        ScrollView {
            // Compact, glanceable content
        }
    }
}
```

### visionOS
```swift
struct ContentView: View {
    var body: some View {
        NavigationStack {
            // Spatial content
        }
    }
}
```

## Code Quality Standards

### Always Include:
1. **Preview macro** for Xcode canvas
2. **MARK comments** for organization
3. **Extracted subviews** for complex layouts
4. **Proper accessibility** labels
5. **SF Symbols** for icons

### Patterns to Follow:
```swift
// 1. State at the top
@State private var email = ""
@State private var isLoading = false

// 2. Body with clear structure
var body: some View {
    VStack(spacing: 24) {
        header
        content
        actions
    }
    .padding()
}

// 3. Computed properties for subviews
private var header: some View {
    Text("Title")
        .font(.largeTitle)
}
```

## Component Library Patterns

### Button Styles
```swift
// Primary
Button("Sign In") { }
    .buttonStyle(.borderedProminent)

// Secondary
Button("Cancel") { }
    .buttonStyle(.bordered)

// Destructive
Button("Delete", role: .destructive) { }
```

### Input Fields
```swift
// Text input
TextField("Email", text: $email)
    .textFieldStyle(.roundedBorder)
    .textContentType(.emailAddress)
    .keyboardType(.emailAddress)

// Secure input
SecureField("Password", text: $password)
    .textFieldStyle(.roundedBorder)
    .textContentType(.password)
```

### Lists
```swift
List {
    ForEach(items) { item in
        ItemRow(item: item)
    }
    .onDelete(perform: delete)
}
.listStyle(.insetGrouped) // iOS
.listStyle(.sidebar)      // macOS
```

### Navigation (macOS)
```swift
NavigationSplitView {
    List(selection: $selection) {
        ForEach(items) { item in
            NavigationLink(value: item) {
                Label(item.title, systemImage: item.icon)
            }
        }
    }
    .navigationSplitViewColumnWidth(min: 200, ideal: 250)
} detail: {
    DetailView(item: selection)
}
```

## macOS-Specific Patterns

### Toolbar
```swift
.toolbar {
    ToolbarItem(placement: .primaryAction) {
        Button(action: add) {
            Label("Add", systemImage: "plus")
        }
    }
    ToolbarItem(placement: .destructiveAction) {
        Button(action: delete) {
            Label("Delete", systemImage: "trash")
        }
    }
}
```

### Settings Window
```swift
struct SettingsView: View {
    var body: some View {
        TabView {
            GeneralSettings()
                .tabItem {
                    Label("General", systemImage: "gearshape")
                }

            EncodingSettings()
                .tabItem {
                    Label("Encoding", systemImage: "film")
                }
        }
        .frame(width: 500, height: 400)
    }
}
```

### Menu Bar Commands
```swift
.commands {
    CommandGroup(replacing: .newItem) {
        Button("New Video") { }
            .keyboardShortcut("n")
    }
    CommandMenu("Encoding") {
        Button("Start Encoding") { }
            .keyboardShortcut("e")
    }
}
```

## Async/Loading Patterns

```swift
struct AsyncContentView: View {
    @State private var isLoading = false
    @State private var items: [Item] = []

    var body: some View {
        Group {
            if isLoading {
                ProgressView()
            } else if items.isEmpty {
                ContentUnavailableView(
                    "No Videos",
                    systemImage: "film",
                    description: Text("Add videos to get started")
                )
            } else {
                List(items) { item in
                    ItemRow(item: item)
                }
            }
        }
        .task {
            await loadItems()
        }
    }
}
```

## File Saving Convention

Generated code is saved to:
```
Sources/Views/[ViewName].swift
Sources/Components/[ComponentName].swift
```

Or in current directory if no Sources folder exists.

## Integration with Pencil Workflow

If you want to visualize the generated code:
1. Generate with `/pencil-design-tools:swift`
2. Run in Xcode with Live Preview
3. If changes needed, iterate in code OR
4. Create mockup with `/pencil-design-tools:mockup` for spec documentation
