# Pencil Design Tools

AI-powered design tools for Claude Code and Claude Cowork.

## Features

### `/pencil-design-tools:mockup`
Create UI mockups from natural language descriptions using Pencil.dev.

```
/pencil-design-tools:mockup Login screen with email, password, and social login
```

**Output:**
- Visual mockup in Pencil.dev
- Screenshot saved to `docs/mockups/`
- Markdown snippet for spec documents

### `/pencil-design-tools:swiftui`
Convert Pencil designs to SwiftUI code.

```
/pencil-design-tools:swiftui
```

**Output:**
- SwiftUI View structs
- Color theme from design variables
- Proper layout with VStack/HStack/ZStack

### `/pencil-design-tools:design`
Combined workflow - mockup + optional SwiftUI conversion.

```
/pencil-design-tools:design Settings page for iOS app -> SwiftUI
```

## Installation

### From Local Marketplace

```bash
# Add the marketplace (one time)
/plugin marketplace add ~/Developer/claude-plugins/kjetil-marketplace

# Install the plugin
/plugin install pencil-design-tools@kjetil-plugins
```

### For Development (Iterative Testing)

```bash
# Run Claude Code with the plugin loaded directly
claude --plugin-dir ~/Developer/claude-plugins/pencil-design-tools

# Or in Claude Cowork, add to personal plugins
```

## Requirements

- **Pencil MCP Server** must be configured in Claude Code/Cowork
- Pencil.dev editor for visual design

## Directory Structure

```
pencil-design-tools/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── skills/
│   ├── mockup-generator/
│   │   └── SKILL.md         # /mockup skill
│   └── pen-to-swiftui/
│       └── SKILL.md         # /swiftui skill
├── commands/
│   └── design.md            # /design command
└── README.md
```

## Development Workflow

### Quick Iteration

1. Edit skills in `~/Developer/claude-plugins/pencil-design-tools/skills/`
2. Test with `claude --plugin-dir ~/Developer/claude-plugins/pencil-design-tools`
3. Skills reload automatically on each invocation

### Testing in Cowork

1. Add plugin directory as personal plugin in Cowork settings
2. Reload Cowork
3. Skills appear under plugin namespace

## Mapping Reference

### .pen → SwiftUI

| Pencil | SwiftUI |
|--------|---------|
| `frame` + `vertical` | `VStack` |
| `frame` + `horizontal` | `HStack` |
| `fill_container` | `.frame(maxWidth: .infinity)` |
| `cornerRadius` | `.clipShape(RoundedRectangle)` |
| `padding` | `.padding()` |
| `gap` | `spacing:` parameter |

## License

MIT
