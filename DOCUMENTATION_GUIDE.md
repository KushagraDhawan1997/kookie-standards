# Component Documentation Guide

This guide outlines the standard structure and writing patterns for component documentation in Kookie Blocks. Follow this structure to ensure consistency across all component docs.

## File Structure

- **File Format**: Pure Markdown (`.mdx` files)
- **File Location**: `apps/docs/app/docs/blocks/{component-name}/content.mdx`
- **File Naming**: Always use `content.mdx` for component documentation

## MDX/JSX Formatting Rules

**Critical**: When writing JSX in MDX files, never include line breaks between opening tags and content. MDX will parse text content as paragraph elements, adding unwanted margins.

### ❌ Wrong

```tsx
<Button>Text</Button>
```

This gets parsed as:

```tsx
<Button>
  <p>Text</p> {/* Unwanted paragraph wrapper with margins */}
</Button>
```

### ✅ Correct

```tsx
<Button>Text</Button>
```

### When to Use Line Breaks

Only use line breaks when you have:

- Multiple child elements
- Complex nested structures
- Comments that need separation

```tsx
<Button>
  <Icon />
  Text
</Button>
```

```tsx
<Shell.Root>
  <Shell.Sidebar>Navigation</Shell.Sidebar>
  <Shell.Content>Main content</Shell.Content>
</Shell.Root>
```

**Rule of thumb**: If the content is simple text or a single element, keep it on one line. If you have multiple elements or complex nesting, line breaks are acceptable.

## Documentation Structure

Follow this exact order for all component documentation:

### 1. Frontmatter

```markdown
---
title: Component
description: Brief description of what this component does
---
```

**Guidelines:**

- **Title should be 1-2 words maximum** - just the component name (e.g., "Button", "CodeBlock", "DocsShell")
- Description should be concise (1 sentence, ~50 characters)
- Title and description are rendered by the docs system - no need to repeat them in the content

**Examples:**

✅ **Good:**
```yaml
---
title: Button
description: Clickable button with variants and sizes
---
```

```yaml
---
title: PreviewBlock
description: Live component previews in documentation
---
```

❌ **Bad:**
```yaml
---
title: Button Component Documentation
description: This is a comprehensive guide to using the Button component in your projects
---
```

**Note:** Do not include an H1 heading in the content - the frontmatter `title` is used as the page title.

### 2. Installation

````markdown
## Installation

```bash
npm install @kushagradhawan/kookie-blocks
```
````

**Guidelines:**

- Always include this section
- Use the exact package name shown above

### 3. Usage

````markdown
## Usage

```tsx
import { Component } from "@kushagradhawan/kookie-blocks";

export function MyComponent() {
  return <Component>Content</Component>;
}
```
````

````

**Guidelines:**
- Show the most basic, common usage
- Include import statement
- Use realistic but minimal example
- For compound components, show the minimal working structure

### 4. Anatomy (Compound Components Only)

For components with multiple sub-components (like `DocsShell.Root`, `DocsShell.Sidebar`, etc.):

```markdown
## Anatomy

Component is a compound component with the following parts:

```tsx
<Component.Root>
  <Component.Part1>{/* Description */}</Component.Part1>
  <Component.Part2>{/* Description */}</Component.Part2>
</Component.Root>
```
````

**Guidelines:**

- List all sub-components
- Include brief comments describing each part's purpose
- Show the complete structure

### 5. Props

Document all props in tables. For compound components, organize by sub-component:

```markdown
## Root Props

The Root component manages [purpose] and provides context to child components.

| Prop       | Type                | Description                                          |
| ---------- | ------------------- | ---------------------------------------------------- |
| `propName` | `Type \| UnionType` | Clear description of what it does and when to use it |

## SubComponent Props

Description of this sub-component's purpose.

| Prop       | Type   | Description |
| ---------- | ------ | ----------- |
| `propName` | `Type` | Description |
```

**Guidelines:**

- Use markdown tables
- Include TypeScript types with union types shown as `'option1' \| 'option2'`
- Descriptions should be practical and mention use cases
- Group props by component for compound components
- Use backticks for prop names and types

### 6. Variants (If Applicable)

````markdown
## Variants

Use the `variant` prop to set component style.

### Variant Name

Description of when and why to use this variant.

```tsx
<Component variant="variantName">Content</Component>
```
````

**Guidelines:**

- Start with a brief intro explaining the `variant` prop
- Each variant gets its own subsection (###)
- Include description of use case
- Show code example for each variant
- Order variants from most prominent to least (classic → solid → soft → outline → ghost)

### 7. Sizes (If Applicable)

````markdown
## Sizes

Set `size` for component density: `1` (24px), `2` (32px), `3` (40px), `4` (48px). [Additional context about responsive support, mobile considerations, etc.]

### Size 1

For [use case].

```tsx
<Component size="1">Content</Component>
```
````

````

**Guidelines:**
- List all available sizes with pixel values
- Mention responsive support if applicable
- Each size gets its own subsection with use case
- Include code example for each size

### 8. Colors (If Applicable)

```markdown
## Colors

Use the `color` prop with semantic colors to communicate [purpose]. Use `highContrast` for maximum visibility, especially on complex or translucent backgrounds.

```tsx
<Component size="3" color="crimson" variant="solid">Content</Component>
<Component size="3" color="crimson" variant="solid" highContrast>Content</Component>
```
````

**Guidelines:**

- Explain semantic color usage
- Mention `highContrast` when relevant
- Show examples with and without highContrast

### 9. Material (If Applicable)

````markdown
## Material

Use the `material` prop to set component appearance. Choose `solid` for opaque backgrounds, or `translucent` for depth and separation over images or dynamic backgrounds.

### Theme

Components automatically inherit the theme's material setting.

```tsx
<Theme material="translucent">
  <Component>Content</Component>
</Theme>
```
````

### Custom

Override the theme's material for specific effects.

```tsx
<Theme material="solid">
  <Component material="translucent">Content</Component>
</Theme>
```

````

### 10. States

```markdown
## States

### Loading

Description of loading state behavior.

```tsx
<Component loading>Content</Component>
```

### Disabled

Description of disabled state and when to use it.

```tsx
<Component disabled>Content</Component>
```

### Interactive

Description of interactive states (hover, focus, active).

```tsx
<Component data-state="open">Content</Component>
```
````

**Guidelines:**

- Document all interactive states
- Explain when to use each state
- Include code examples
- Mention automatic behaviors (e.g., loading automatically disables)

### 11. Layout/Other Sections

Add component-specific sections as needed:

````markdown
## Layout

### Full Width

Description and use case.

```tsx
<Component fullWidth>Content</Component>
```
````

## Polymorphism

### As

Description of `as` prop usage.

```tsx
<Component as="a" href="/link">
  Content
</Component>
```

### AsChild

Description of `asChild` prop usage.

```tsx
<Component asChild>
  <a href="/link">Content</a>
</Component>
```

````

**Guidelines:**
- Add sections relevant to the component
- Use clear, descriptive section names
- Include practical examples
- Explain use cases

### 12. Examples

```markdown
## Examples

### Example Name

Description of the pattern and when to use it.

```tsx
<Component>
  {/* Multiple elements or complex structure */}
  <Child>Content</Child>
</Component>
```

### Another Example

Another common pattern.

```tsx
<Component>{/* Example code */}</Component>
```
````

**Guidelines:**

- Show real-world usage patterns
- Include complete, working examples
- Explain the pattern's purpose
- Order from simple to complex

### 13. Responsive (If Applicable)

````markdown
## Responsive

Use responsive objects with the `size` prop to adapt sizing across different breakpoints. The component uses a mobile-first approach.

```tsx
<Component size={{ initial: "1", sm: "2", md: "3" }}>Content</Component>
```
````

### Breakpoints

| Breakpoint | Value  | Description                |
| ---------- | ------ | -------------------------- |
| `initial`  | -      | Base styles (mobile-first) |
| `xs`       | 520px  | Extra small screens        |
| `sm`       | 768px  | Small screens (tablets)    |
| `md`       | 1024px | Medium screens (laptops)   |
| `lg`       | 1280px | Large screens (desktops)   |
| `xl`       | 1640px | Extra large screens        |

````

**Guidelines:**
- Include if component supports responsive props
- Show breakpoint table if relevant
- Explain mobile-first approach

### 14. Accessibility

```markdown
## Accessibility

Component provides comprehensive accessibility through [methods].

### Keyboard Navigation

- **Key** - Action description
- **Key** - Another action

### ARIA Attributes

- `attribute="value"` - Purpose description
- `attribute="value"` - Another purpose

### Screen Readers

- Feature description
- Another feature

### High Contrast

- Windows High Contrast mode support description
- Enhanced focus indicators
```

**Guidelines:**

- Document all accessibility features
- Organize by category (keyboard, ARIA, screen readers, etc.)
- Be specific about implementations
- Mention special considerations (high contrast, reduced motion, etc.)

### 15. Changelog

```markdown
## Changelog

### Added

- New feature description
- Another new feature

### Changed

- Change description
- Another change

### Deprecated

- Deprecated feature and migration path

### Fixed

- Bug fix description
- Another fix
```

**Guidelines:**

- Document all significant changes
- Use clear, concise descriptions
- Group by type (Added, Changed, Deprecated, Fixed)
- Include migration notes for breaking changes

## Writing Guidelines

### Tone and Style

- **Design-oriented**: Write for designers, not just developers
- **User-focused**: Emphasize when and why to use features, not just how
- **Clear and concise**: Avoid jargon, use plain language
- **Practical**: Include real-world use cases and examples

### Section Headings

**The larger the heading level, the shorter the title.**

**Important:** Do not use H1 (`#`) in content - the frontmatter `title` is used as the page title.

Heading hierarchy and length:
- **H1** (`#`): Reserved for frontmatter `title` only - never in content
- **H2** (`##`): 1-3 words (e.g., "Usage", "Installation", "Props", "Variants")
- **H3** (`###`): 2-4 words (e.g., "Solid Variant", "Size 2", "Basic Example")
- **H4+** (`####`): Can be slightly longer if needed, but stay concise

**Examples:**

✅ **Good:**
```markdown
---
title: Button
---

## Installation

## Props

### Solid Variant

### Size 1
```

❌ **Avoid:**
```markdown
---
title: Button Component Documentation
---

# Button

## How to Install the Package

## Troubleshooting Common Issues

### The Solid Variant for Emphasized Actions
```

**Guidelines:**

- Start content with H2 (`##`), not H1 (`#`)
- Titles should be scannable at a glance
- Avoid full sentences in headings
- Use descriptive but minimal terms
- Let the content below explain the details, not the heading
- The heading is a label, not a description

### Code Examples

- Always include working, complete examples
- Use realistic but minimal code
- Show imports when introducing new concepts
- Include comments for clarity when needed
- Use proper TypeScript/TSX syntax
- **Prefer showing line numbers** in code blocks for easier reference
- **Never use line breaks in JSX for simple text content** - keep `<Component>Text</Component>` on one line to avoid MDX parsing issues

**Line Numbers:**

Include line numbers for code blocks longer than 5 lines, or when referencing specific lines in explanations:

````markdown
```tsx showLineNumbers
import { Button } from "@kushagradhawan/kookie-blocks";

export function Example() {
  return (
    <Button variant="solid" size="2">
      Click me
    </Button>
  );
}
```
````

For short snippets (1-4 lines), line numbers are optional:

````markdown
```tsx
<Button variant="solid">Click me</Button>
```
````

### Descriptions

- Start with the "what" and "why" before the "how"
- Mention use cases and design considerations
- Explain visual impact and user experience
- Connect features to design principles

### Props Documentation

- Be specific about types and values
- Explain when to use each prop
- Mention default values
- Note any responsive support
- Include deprecation notices when applicable

## Common Patterns

### Compound Components

For components like `DocsShell`, `Hero`, etc.:

1. Show anatomy first
2. Document props by sub-component
3. Include examples showing composition
4. Explain composition rules if applicable

### Simple Components

For components like `CodeBlock`:

1. Skip anatomy section
2. Document all props in one table
3. Focus on variants, sizes, and states
4. Include layout and polymorphism sections

## Checklist

Before publishing documentation, ensure:

- [ ] Frontmatter has concise title (1-2 words) and description
- [ ] No H1 heading in content (frontmatter title is used)
- [ ] All H2/H3 headings follow length hierarchy (H2: 1-3 words, H3: 2-4 words)
- [ ] Installation section included
- [ ] Basic usage example works
- [ ] All props documented with types and descriptions
- [ ] Variants, sizes, colors documented if applicable
- [ ] States section complete
- [ ] Examples are realistic and useful
- [ ] Code blocks longer than 5 lines use `showLineNumbers`
- [ ] Accessibility features documented
- [ ] Changelog is up to date
- [ ] All code examples are valid TSX
- [ ] JSX examples follow single-line rule for simple text content
- [ ] Links to source code are correct (if applicable)
- [ ] File is named `content.mdx` in correct directory

## Preview Block Usage

When documenting components that benefit from live visual previews, use the `PreviewBlock` component to show rendered examples alongside code blocks.

### Basic Structure

```markdown
import { PreviewBlock } from "@kushagradhawan/kookie-blocks";

## Example Name

<PreviewBlock>
  <Component prop="value">Content</Component>
</PreviewBlock>

```tsx
<Component prop="value">Content</Component>
```
````

### Guidelines

- Import `PreviewBlock` from the `@kushagradhawan/kookie-blocks` package at the top of your MDX file
- Use `<PreviewBlock>` to wrap the live rendered component
- Follow with a standard markdown code fence showing the source code
- PreviewBlock and CodeBlock are completely separate - don't try to combine them

### Background Options

PreviewBlock supports different background styles:

```markdown
<!-- Default: Grid background -->
<PreviewBlock>
  <Component>Content</Component>
</PreviewBlock>

<!-- No background -->
<PreviewBlock background="none">
  <Component>Content</Component>
</PreviewBlock>

<!-- Dots background -->
<PreviewBlock background="dots">
  <Component>Content</Component>
</PreviewBlock>
```

### When to Use PreviewBlock

- Component variations (sizes, colors, variants)
- Interactive components (buttons, inputs, dropdowns)
- Layout components (flex, grid demonstrations)
- Visual components where seeing the result is important

### When NOT to Use PreviewBlock

- Code-only examples (utilities, hooks, TypeScript types)
- Complex examples requiring full application context
- Examples that need external data or state

## Examples

Refer to these files as reference implementations:

- **Simple Component**: `apps/docs/app/docs/blocks/code-block/content.mdx`
- **Compound Component**: `apps/docs/app/docs/blocks/docs-shell/content.mdx`
- **Complex Component**: `apps/docs/app/docs/blocks/hero/content.mdx`
- **Preview Block Usage**: `apps/docs/app/docs/test-preview/content.mdx`
