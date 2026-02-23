---
name: ux-review
description: Validates UI mockups against UX best practices and Apple Human Interface Guidelines. Checks accessibility, touch targets, cognitive load, and platform conventions. Use after creating mockups or on existing designs to ensure quality UX.
---

# UX Review Skill

Validates mockups and designs against UX best practices, accessibility standards, and Apple Human Interface Guidelines.

## When to Use This Skill

Activate when:
- After creating a mockup with `/pencil-design-tools:mockup`
- Reviewing existing designs in .pen files
- Validating UX before finalizing PRD
- Checking accessibility compliance

## Validation Categories

### 1. Touch/Click Targets (Accessibility)

**Apple HIG Requirements:**
- iOS: Minimum 44x44 pt
- macOS: Minimum 24x24 pt (28x28 recommended)
- watchOS: Minimum 38x38 pt

**Check:**
```
- Are all interactive elements large enough?
- Is there adequate spacing between targets?
- Can users with motor impairments use this?
```

### 2. Visual Hierarchy

**Check:**
- Primary action is visually prominent
- Clear distinction between primary/secondary/tertiary actions
- Logical reading order (F-pattern or Z-pattern)
- Important info above the fold

**Red flags:**
- Multiple competing CTAs
- Buried primary action
- Visual clutter

### 3. Cognitive Load

**Check:**
- Max 5-7 items in navigation/lists without grouping
- Clear labels (no jargon)
- Progressive disclosure for complex tasks
- Consistent patterns across screens

**Red flags:**
- Too many choices at once
- Unclear iconography without labels
- Mixed interaction patterns

### 4. Accessibility (WCAG)

**Check:**
- Color contrast ratio (4.5:1 for text, 3:1 for large text)
- Not relying solely on color to convey info
- Text size (min 11pt iOS, 10pt macOS)
- Dynamic Type support consideration

**Red flags:**
- Low contrast text
- Color-only indicators
- Tiny text

### 5. Platform Conventions (Apple HIG)

**iOS:**
- Tab bar at bottom (max 5 items)
- Navigation bar at top
- Swipe gestures for navigation
- Pull to refresh for lists

**macOS:**
- Menu bar for app-wide commands
- Sidebar for navigation
- Toolbar for contextual actions
- Keyboard shortcuts for power users

**Red flags:**
- Bottom navigation on macOS
- Hamburger menu on iOS (use tab bar)
- Non-standard gestures

### 6. Error Prevention & Recovery

**Check:**
- Destructive actions require confirmation
- Undo available where possible
- Clear error messages with recovery path
- Input validation with helpful hints

**Red flags:**
- No confirmation for delete
- Vague error messages
- No way to recover from mistakes

### 7. Loading & Empty States

**Check:**
- Loading indicators for async operations
- Skeleton screens for content loading
- Helpful empty states with actions
- Offline state handling

**Red flags:**
- No loading feedback
- Blank empty states
- No offline consideration

## Review Output Format

After reviewing a mockup, output:

```markdown
## UX Review: [Screen Name]

### ✅ Passes
- Touch targets meet minimum size (44pt)
- Clear visual hierarchy with prominent CTA
- Follows macOS sidebar navigation pattern

### ⚠️ Warnings
- **Cognitive load:** 8 items in navigation, consider grouping
- **Contrast:** Subtitle text (#999) may be low contrast on white

### ❌ Issues
- **Touch target:** Settings icon is 32x32, needs 44x44 minimum
- **Error state:** No visible error handling for failed upload

### 💡 Recommendations
1. Increase settings icon to 44x44pt
2. Group navigation items into categories
3. Add error state mockup for upload failure
4. Consider adding keyboard shortcuts for power users
```

## Integration with Mockup Workflow

Can be called automatically after mockup creation:

```
/pencil-design-tools:mockup Login screen with email and password
/pencil-design-tools:ux-review
```

Or on existing design:

```
/pencil-design-tools:ux-review [nodeId or current selection]
```

## Platform-Specific Checklists

### iOS Checklist
- [ ] Touch targets ≥ 44pt
- [ ] Tab bar for main navigation (≤5 items)
- [ ] Safe area respected
- [ ] Dynamic Type support
- [ ] Dark mode consideration

### macOS Checklist
- [ ] Click targets ≥ 24pt
- [ ] Sidebar for navigation
- [ ] Menu bar integration
- [ ] Keyboard shortcuts
- [ ] Window resizing behavior

### iPadOS Checklist
- [ ] Supports both orientations
- [ ] Pointer/trackpad support
- [ ] Multitasking consideration
- [ ] Keyboard shortcuts

### watchOS Checklist
- [ ] Glanceable information
- [ ] Minimal interaction required
- [ ] Digital Crown support
- [ ] Complications if relevant

## References

- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [Nielsen's 10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/)
