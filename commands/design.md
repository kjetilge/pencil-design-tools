---
name: design
description: Quick design workflow - creates mockup and optionally converts to SwiftUI
---

# Design Workflow Command

This command provides a streamlined design workflow:

1. **Analyze** the user's design request
2. **Create** a mockup using Pencil.dev
3. **Save** screenshot to docs/mockups/
4. **Optionally** convert to SwiftUI if requested

## Usage

```
/pencil-design-tools:design [description]
```

## Examples

```
/pencil-design-tools:design Login screen with email and password
/pencil-design-tools:design Dashboard with stats cards and chart
/pencil-design-tools:design Settings page for iOS app -> SwiftUI
```

## Workflow Steps

1. Parse the design description
2. Determine target platform (mobile/desktop/web)
3. Get style inspiration if needed
4. Create mockup in Pencil
5. Take and save screenshot
6. If "-> SwiftUI" or "to Swift" mentioned, also generate SwiftUI code
7. Return markdown snippet for spec document

## Output Format

### For Mockup Only

```markdown
### [Screen Name]

![Screen Name](./mockups/screen-name.png)

**Elements:** [list of key elements]
**Dimensions:** [width x height]
```

### For Mockup + SwiftUI

```markdown
### [Screen Name]

![Screen Name](./mockups/screen-name.png)

**SwiftUI Code:** `Sources/Views/ScreenName.swift`

[Brief code preview]
```
