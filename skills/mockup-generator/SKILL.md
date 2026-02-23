---
name: mockup
description: Creates UI mockups from natural language descriptions using Pencil.dev. Saves screenshots to docs/mockups/ and returns markdown references for spec documents. Use during specification work to visualize screens, components, and user flows.
---

# Mockup Generator

Creates professional UI mockups from natural language descriptions and embeds them in specification documents.

## When to Use This Skill

Activate when user:
- Describes a screen, component, or UI flow during spec work
- Says "mockup", "design", "visualize", "sketch", or "wireframe"
- Needs visual documentation for a feature spec
- Wants to see what a described UI looks like

## Mandatory Workflow

### 1. Setup (First Time Only)

```bash
mkdir -p docs/mockups
```

### 2. Analyze Request

- Identify the UI type (screen, component, form, dashboard, etc.)
- Determine platform hints (mobile, desktop, web)
- Extract key elements (buttons, inputs, lists, etc.)

### 3. Get Style Inspiration

```
1. mcp__pencil__get_style_guide_tags() - Get available style tags
2. mcp__pencil__get_style_guide([relevant tags]) - Get design inspiration
3. mcp__pencil__get_guidelines("design-system" or "landing-page")
```

### 4. Create or Open .pen File

```
mcp__pencil__get_editor_state() - Check current state
mcp__pencil__open_document("docs/mockups/specs.pen") - Open/create design file
```

### 5. Design the Mockup

Use `batch_design` to create the UI. Always use the placeholder pattern:

```javascript
// Example: Login screen
container=I(document, {type: "frame", name: "Login Screen", layout: "vertical", width: 375, height: 812, fill: "#FFFFFF", placeholder: true})
header=I(container, {type: "text", content: "Welcome Back", fontSize: 28, fontWeight: "bold", fill: "#000000"})
// ... more components
U(container, {placeholder: false})  // Remove placeholder when done
```

### 6. Take Screenshot & Save

```
1. mcp__pencil__get_screenshot(nodeId) - Capture the design
2. Save screenshot to docs/mockups/{descriptive-name}.png
```

### 7. Return Markdown Reference

Always return a complete markdown snippet for the spec document:

```markdown
### [Screen Name]

![Screen Name](./mockups/screen-name.png)

**Key Elements:**
- Element 1: Description
- Element 2: Description

**Interactions:**
- Action 1 leads to...
- Action 2 triggers...
```

## Design Principles

### Mobile-First Dimensions
- iPhone: 375 x 812 (standard), 390 x 844 (Pro)
- iPad: 820 x 1180
- Desktop: 1440 x 900

### Common Components

| UI Element | .pen Implementation |
|------------|---------------------|
| Button | `frame` with text child, fill, cornerRadius |
| Input Field | `frame` with border stroke, text placeholder |
| Card | `frame` with shadow effect, padding |
| List Item | `frame` with horizontal layout |
| Navigation Bar | `frame` with horizontal layout, fixed height |
| Tab Bar | `frame` at bottom, icon_font children |

### Color Variables

Always use CSS variables for theming:
```javascript
{fill: "$primary"}
{fill: "$background"}
{fill: "$text-primary"}
```

## File Organization

```
docs/
  mockups/
    specs.pen          # Main design file
    login-screen.png   # Exported screenshots
    dashboard.png
    user-flow.png
  SPEC.md              # Specification document with embedded mockups
```

## Example Output

When user says: "Create a mockup for a login screen with email, password, and social login"

Response:

```
Created login screen mockup.

### Login Screen

![Login Screen](./mockups/login-screen.png)

**Key Elements:**
- Email input field with placeholder
- Password input field with show/hide toggle
- "Sign In" primary button
- "Forgot Password?" link
- Divider with "or continue with"
- Social login buttons (Google, Apple)
- "Don't have an account? Sign up" footer link

**Dimensions:** 375 x 812 (iPhone standard)
**Style:** Clean, modern with subtle shadows
```

## Best Practices

1. **Name frames descriptively** - Use clear names like "Login Screen", "Dashboard Header"
2. **Use placeholder flag** - Set `placeholder: true` while designing, remove when done
3. **Group related elements** - Use frames to organize logical sections
4. **Consistent spacing** - Use 8px grid (8, 16, 24, 32, etc.)
5. **Save incrementally** - Take screenshots after each major addition
6. **Include context** - Add notes about interactions and states

## Handling Multiple Screens

For user flows or multi-screen specs:

```javascript
// Find space for new screen
mcp__pencil__find_empty_space_on_canvas({direction: "right", width: 375, height: 812, padding: 50})

// Create screens side by side
screen1=I(document, {type: "frame", name: "Screen 1", x: 0, ...})
screen2=I(document, {type: "frame", name: "Screen 2", x: 425, ...})
```

## Integration with Spec Documents

The mockups integrate seamlessly with markdown specs:

```markdown
# Feature Specification: User Authentication

## Overview
Users can sign in with email/password or social providers.

## UI Mockups

### Login Screen
![Login](./mockups/login-screen.png)

### Registration Screen
![Register](./mockups/register-screen.png)

## User Flow
1. User opens app -> Login Screen
2. New user taps "Sign up" -> Registration Screen
3. ...
```
