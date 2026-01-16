# Component Examples Guide

This guide defines how to write strategic, thoughtful component examples for Kookie UI and Kookie Blocks documentation. Examples are not variant showcases—they are real-world patterns developers copy-paste.

## Philosophy

**Examples are promises of what's possible.** Each example should answer: "When would I use this in a real product?"

### Core Principles

1. **Context over catalog** - Don't showcase props in isolation; show them solving real problems
2. **Strategic variant distribution** - Weave variants throughout examples rather than listing them
3. **States in situ** - Loading, disabled, and interactive states belong in realistic scenarios
4. **Hierarchy matters** - Every button pairing should have clear primary/secondary relationships
5. **Copy-paste ready** - Examples should work when dropped into a project

## File Structure

### Location

```
apps/docs/app/docs/{component-name}/
├── page.tsx           # Server component (metadata)
├── page-client.tsx    # Client component (tabs: Documentation/Examples)
├── content.mdx        # Documentation content
└── examples.tsx       # Examples component
```

### Page Client Structure

Update `page-client.tsx` to add the Examples tab:

```tsx
'use client';

import { useState } from 'react';
import { TableOfContents } from "@kushagradhawan/kookie-blocks";
import { Tabs } from "@kushagradhawan/kookie-ui";
import { HugeiconsIcon } from "@hugeicons/react";
import { BookOpen01Icon, EyeIcon } from "@hugeicons/core-free-icons";
import { SiteDocsPage } from "@/components/site-docs-page";
import ContentMDX from "./content.mdx";
import { ComponentExamples } from "./examples";
import type { DocMetadata } from "@/lib/frontmatter";

interface ComponentPageClientProps {
  metadata?: DocMetadata;
}

export default function ComponentPageClient({ metadata }: ComponentPageClientProps) {
  const [activeTab, setActiveTab] = useState<"docs" | "examples">("docs");

  return (
    <SiteDocsPage
      meta={metadata}
      headerTabs={
        <Tabs.Root
          value={activeTab}
          onValueChange={(value) => setActiveTab(value as "docs" | "examples")}
        >
          <Tabs.List>
            <Tabs.Trigger value="docs">
              <HugeiconsIcon icon={BookOpen01Icon} strokeWidth={1.75} />
              Documentation
            </Tabs.Trigger>
            <Tabs.Trigger value="examples">
              <HugeiconsIcon icon={EyeIcon} strokeWidth={1.75} />
              Examples
            </Tabs.Trigger>
          </Tabs.List>
        </Tabs.Root>
      }
      tableOfContents={
        <TableOfContents renderContainer={(content) => content || null} />
      }
    >
      {activeTab === "docs" ? <ContentMDX /> : <ComponentExamples />}
    </SiteDocsPage>
  );
}
```

### Examples Component Structure

```tsx
'use client';

import * as React from 'react';
import { PreviewBlock, CodeBlock, SectionHeader } from '@kushagradhawan/kookie-blocks';
import { Flex, Button, Text, Separator } from '@kushagradhawan/kookie-ui';

export function ComponentExamples() {
  return (
    <Flex direction="column" gap="9">
      {/* Example 1 */}
      <Flex direction="column" gap="4">
        <SectionHeader.Root>
          <SectionHeader.Content>
            <SectionHeader.Title>Example Name</SectionHeader.Title>
            <SectionHeader.Description>
              Clear description of the pattern and when to use it.
            </SectionHeader.Description>
          </SectionHeader.Content>
        </SectionHeader.Root>
        <PreviewBlock background="none" height="8rem">
          {/* Live component preview */}
        </PreviewBlock>
        <CodeBlock
          code={`{/* Code shown to user */}`}
          language="tsx"
          showLineNumbers={true}
          collapsible={false}
        />
      </Flex>

      <Separator size="4" />

      {/* Example 2, etc. */}
    </Flex>
  );
}
```

## Example Anatomy

Every example has three parts:

### 1. Section Header

```tsx
<SectionHeader.Root>
  <SectionHeader.Content>
    <SectionHeader.Title>Form Submission</SectionHeader.Title>
    <SectionHeader.Description>
      The classic variant commands attention for primary actions. Click to see
      the loading state in action.
    </SectionHeader.Description>
  </SectionHeader.Content>
</SectionHeader.Root>
```

**Guidelines:**
- Title: 2-3 words naming the pattern (not "Solid Variant" but "Form Submission")
- Description: Explain *why* this pattern, which variants are used, and any interactivity

### 2. Preview Block

```tsx
<PreviewBlock background="none" height="8rem">
  <Flex gap="2" align="center">
    <Button variant="soft" size="2" color="gray" highContrast>
      Cancel
    </Button>
    <Button variant="classic" size="2" highContrast>
      Save Changes
    </Button>
  </Flex>
</PreviewBlock>
```

**Guidelines:**
- Use `background="none"` for most examples
- Set appropriate `height` for the content
- Use real, minimal UI—not lorem ipsum

### 3. Code Block

```tsx
<CodeBlock
  code={`<Flex gap="2" align="center">
  <Button
    variant="soft"
    size="2"
    color="gray"
    highContrast
  >
    Cancel
  </Button>
  <Button
    variant="classic"
    size="2"
    highContrast
  >
    Save Changes
  </Button>
</Flex>`}
  language="tsx"
  showLineNumbers={true}
  collapsible={false}
/>
```

**Guidelines:**
- Code must match the preview exactly
- Use multi-line formatting when components have 3+ props
- Include only the essential code (no wrapper functions unless needed)

## Code Formatting

> **IMPORTANT:** Always use multi-line formatting for props. This prevents horizontal scrolling in code blocks and improves readability. Single-line props are acceptable only for components with 1-2 short props.

### Multi-Line Props

When a component has 3 or more props, split across lines:

```tsx
// ✅ Good: Props on separate lines
<Button
  variant="classic"
  size="2"
  highContrast
  loading={isSubmitting}
  onClick={handleSubmit}
>
  Save Changes
</Button>

// ❌ Avoid: Long single line
<Button variant="classic" size="2" highContrast loading={isSubmitting} onClick={handleSubmit}>Save Changes</Button>
```

### Flex Containers

Split Flex props when there are many:

```tsx
// ✅ Good
<Flex
  direction="column"
  align="center"
  justify="center"
  gap="4"
  py="5"
  style={{ textAlign: 'center' }}
>
  {/* content */}
</Flex>

// ❌ Avoid
<Flex direction="column" align="center" justify="center" gap="4" py="5" style={{ textAlign: 'center' }}>
```

## Strategic Variant Usage

### Don't Create Variant Catalogs

```tsx
// ❌ Avoid: Listing variants without context
<Button variant="solid">Solid</Button>
<Button variant="soft">Soft</Button>
<Button variant="outline">Outline</Button>
```

### Do Show Variants in Context

```tsx
// ✅ Good: Variants solving a real problem
<Flex gap="2" align="center">
  <Button variant="soft" size="2" color="gray" highContrast>
    Cancel
  </Button>
  <Button variant="classic" size="2" highContrast>
    Save Changes
  </Button>
</Flex>
```

### Variant Distribution

Spread variants across examples strategically:

| Example | Primary Variant | Secondary Variant | Why |
|---------|-----------------|-------------------|-----|
| Form Submission | `classic` | `soft` | Classic commands attention |
| Destructive | `solid` (red) | `outline` | Red signals danger, outline for escape |
| Card CTA | `solid` | `ghost` | Solid inside classic card for hierarchy |
| Toolbar | `soft` | - | Subtle fills for grouped actions |
| Empty State | `surface` | - | Gentle elevation for CTAs |
| Compact | `soft` (size 1) | `ghost` (size 1) | Dense interfaces |

## Showing States

### Loading State

Show loading in a realistic interaction context:

```tsx
export function ComponentExamples() {
  const [isSubmitting, setIsSubmitting] = React.useState(false);

  const handleSubmit = () => {
    setIsSubmitting(true);
    setTimeout(() => setIsSubmitting(false), 2000);
  };

  return (
    <Button
      variant="classic"
      size="2"
      highContrast
      loading={isSubmitting}
      onClick={handleSubmit}
    >
      {isSubmitting ? 'Saving...' : 'Save Changes'}
    </Button>
  );
}
```

### Disabled with Tooltip

Disabled buttons should explain why:

```tsx
<Button
  variant="soft"
  size="2"
  color="gray"
  highContrast
  disabled={selectedCount === 0}
  tooltip={selectedCount === 0 ? 'Select items first' : undefined}
>
  Export
</Button>
```

### Interactive State Toggles

Let users toggle states to understand behavior:

```tsx
const [selectedCount, setSelectedCount] = React.useState(0);

const toggleSelection = () => {
  setSelectedCount((prev) => (prev === 0 ? 3 : 0));
};

<Button onClick={toggleSelection}>
  {selectedCount > 0 ? `${selectedCount} selected` : 'Select items'}
</Button>
```

## Hierarchy in Button Pairs

Every button pairing needs clear visual hierarchy:

### Classic Card Rule

Inside a `variant="classic"` Card, use `variant="solid"` for the button—not classic inside classic:

```tsx
// ✅ Good: solid inside classic card
<Card variant="classic" size="2">
  <Button variant="solid" size="2">
    Upgrade Now
  </Button>
  <Button variant="ghost" size="2" color="gray">
    Compare Plans
  </Button>
</Card>

// ❌ Avoid: classic inside classic
<Card variant="classic">
  <Button variant="classic">Upgrade</Button>
</Card>
```

### Primary/Secondary Patterns

| Context | Primary | Secondary |
|---------|---------|-----------|
| Form actions | `classic` or `solid` | `soft` or `outline` |
| Destructive | `solid` (red) | `outline` (gray) |
| Card CTA | `solid` | `ghost` |
| Marketing | `classic` | `soft` |

## Size Demonstrations

### Compact Interfaces

Show `size="1"` in a realistic dense toolbar:

```tsx
<Flex gap="1" align="center">
  <Button variant="soft" size="1" color="gray" highContrast>
    <HugeiconsIcon icon={FilterIcon} strokeWidth={1.75} />
    Filter
  </Button>
  <Button variant="soft" size="1" color="gray" highContrast>
    <HugeiconsIcon icon={SortingAZ01Icon} strokeWidth={1.75} />
    Sort
  </Button>
</Flex>
```

### Default Size

Use `size="2"` for most examples—it's the standard:

```tsx
<Button variant="classic" size="2" highContrast>
  Save Changes
</Button>
```

## Translucent Material

For translucent examples, use PreviewBlock with custom background:

```tsx
<PreviewBlock
  height="12rem"
  appearance="dark"
  showThemeToggle={false}
  variant="ghost"
  background={{
    backgroundColor: 'hsl(220, 20%, 10%)',
    backgroundImage:
      'radial-gradient(circle at 30% 20%, hsl(280, 60%, 25%) 0%, transparent 50%), radial-gradient(circle at 70% 80%, hsl(200, 60%, 20%) 0%, transparent 50%)',
    borderRadius: 'var(--radius-3)',
  }}
>
  <Button variant="soft" size="2" highContrast material="translucent">
    Watch Demo
  </Button>
</PreviewBlock>
```

**Guidelines:**
- Use `appearance="dark"` for dark backgrounds
- Use `showThemeToggle={false}` to prevent theme switching
- Use `variant="ghost"` on PreviewBlock for minimal chrome
- Use `soft` variant with `highContrast` for best translucent readability

## Content Hierarchy

When examples include text content (like cards or empty states), use proper Kookie typography:

```tsx
<Flex direction="column" gap="1">
  <Heading size="4" weight="medium">
    Pro Plan
  </Heading>
  <Text size="2" color="gray">
    Unlimited projects and collaborators
  </Text>
</Flex>
```

**Guidelines:**
- Use `Heading` for titles, not `Text` with `weight="medium"`
- Use `Text size="2" color="gray"` for descriptions
- Use `gap="1"` between title and description
- Use `gap="6"` between content sections and actions

## Card Variants in Examples

Use `variant="classic"` for Cards that provide UI context in examples:

```tsx
// ✅ Good: classic variant for example context
<Card variant="classic" size="2">
  <Flex justify="between" align="center" p="2">
    <Text size="2" weight="medium">Dashboard</Text>
    <IconButton variant="soft" size="2" color="gray" highContrast>
      <HugeiconsIcon icon={SettingsIcon} />
    </IconButton>
  </Flex>
</Card>

// ❌ Avoid: surface variant (less visual definition)
<Card variant="surface" size="2">
  ...
</Card>
```

The `classic` variant provides better visual definition and shadow, making it clear where the component boundaries are.

## UI Context for Components

Consider providing realistic UI context when it helps clarify where/how a component is used:

```tsx
// ✅ Good: Icon buttons in a realistic toolbar context
<Card variant="classic" size="2">
  <Flex justify="between" align="center" p="2">
    <Text size="2" weight="medium">Dashboard</Text>
    <Flex gap="1">
      <IconButton variant="soft" size="2" color="gray" highContrast tooltip="Search">
        <HugeiconsIcon icon={SearchIcon} />
      </IconButton>
      <IconButton variant="soft" size="2" color="gray" highContrast tooltip="Settings">
        <HugeiconsIcon icon={SettingsIcon} />
      </IconButton>
    </Flex>
  </Flex>
</Card>

// ❌ Avoid: Floating icon buttons without context
<Flex gap="1">
  <IconButton variant="soft" size="2">...</IconButton>
  <IconButton variant="soft" size="2">...</IconButton>
</Flex>
```

**Guidelines:**
- UI context is discretionary—use your judgment on when it adds value
- Toolbar/table controls benefit from context (shows where they live)
- Simple toggles (like buttons) can stand alone
- When using context, prefer skeleton/minimal content to keep focus on the showcased component
- Use generic labels like "Dashboard", "24 items" rather than distracting content

## Props Over Inline Styles

Use component props instead of inline styles when available (Kookie rules):

```tsx
// ✅ Good: Using Flex props
<Flex position="absolute" top="2" right="2">
  <IconButton variant="ghost" size="2">
    <HugeiconsIcon icon={CloseIcon} />
  </IconButton>
</Flex>

// ❌ Avoid: Inline styles when props exist
<Flex style={{ position: 'absolute', top: 'var(--space-2)', right: 'var(--space-2)' }}>
  ...
</Flex>
```

## Icon Button Accessibility

Icon buttons require `aria-label` for screen readers:

```tsx
// ✅ Good: aria-label describes the action
<IconButton
  variant="soft"
  size="2"
  color="gray"
  highContrast
  aria-label="Search"
  tooltip="Search"
>
  <HugeiconsIcon icon={SearchIcon} strokeWidth={1.75} />
</IconButton>

// ❌ Avoid: Missing aria-label
<IconButton variant="soft" size="2">
  <HugeiconsIcon icon={SearchIcon} />
</IconButton>
```

**Guidelines:**
- Always include `aria-label` on icon buttons
- Use `tooltip` for visual hover feedback (matches aria-label typically)
- For toggle states, update aria-label dynamically: `aria-label={liked ? 'Unlike' : 'Like'}`

## Toggle Components

Toggle buttons use `pressed` and `onPressedChange` for state:

```tsx
const [liked, setLiked] = React.useState(false);

<ToggleIconButton
  variant="ghost"
  size="2"
  color="crimson"
  pressed={liked}
  onPressedChange={setLiked}
  aria-label={liked ? 'Unlike' : 'Like'}
  tooltip={liked ? 'Unlike' : 'Like'}
>
  <HugeiconsIcon icon={FavouriteIcon} strokeWidth={1.75} />
</ToggleIconButton>
```

**Guidelines:**
- Include state declaration in CodeBlock so users understand the pattern
- Update aria-label and tooltip based on pressed state
- Choose colors that match the action (crimson for like, amber for favorite, etc.)

## Gray Buttons

Always use `highContrast` on gray buttons:

```tsx
// ✅ Good
<Button variant="soft" size="2" color="gray" highContrast>
  Cancel
</Button>

// ❌ Avoid: Low contrast gray
<Button variant="soft" size="2" color="gray">
  Cancel
</Button>
```

## Spacing

### Between Buttons

Use `gap="2"` for most button pairs:

```tsx
<Flex gap="2" align="center">
  <Button variant="soft" size="2" color="gray" highContrast>
    Cancel
  </Button>
  <Button variant="classic" size="2" highContrast>
    Save
  </Button>
</Flex>
```

### Dense Toolbars

Use `gap="1"` for compact toolbars:

```tsx
<Flex gap="1" align="center">
  <Button variant="soft" size="1" color="gray" highContrast>Filter</Button>
  <Button variant="soft" size="1" color="gray" highContrast>Sort</Button>
</Flex>
```

## Checklist

Before publishing examples:

- [ ] Each example has a real-world use case name (not "Solid Variant")
- [ ] Variants are distributed strategically across examples
- [ ] States (loading, disabled) are shown in context, not isolated
- [ ] Button pairs have clear hierarchy
- [ ] Classic cards don't contain classic buttons
- [ ] Gray buttons use `highContrast`
- [ ] Code formatting uses multi-line for 3+ props
- [ ] PreviewBlock and CodeBlock match exactly
- [ ] Interactive examples work (click to test)
- [ ] Sizes are demonstrated in appropriate contexts
- [ ] Translucent examples have proper dark backgrounds
- [ ] Cards use `variant="classic"` for UI context
- [ ] UI context added where it helps clarify usage (discretionary)
- [ ] Props used over inline styles when available
- [ ] Icon buttons have `aria-label` for accessibility
- [ ] Toggle components include state declaration in CodeBlock

## Block-Level Components

Block components (Hero, Footer, Navbar, SectionHeader) need different example strategies than primitive components (Button, Card).

### Layout Variations

Show different layouts in realistic contexts, not as isolated options:

```tsx
// ✅ Good: Layout in context
<SectionHeader.Root>
  <SectionHeader.Title>Split Layout</SectionHeader.Title>
  <SectionHeader.Description>
    Horizontal layout with content and media side-by-side
  </SectionHeader.Description>
</SectionHeader.Root>

// ❌ Avoid: Isolated layout demo
<SectionHeader.Root>
  <SectionHeader.Title>layout="split"</SectionHeader.Title>
</SectionHeader.Root>
```

### Responsive Props

Demonstrate responsive breakpoint props with clear descriptions:

```tsx
<Hero.Root
  layout={{ initial: "stacked", md: "split" }}
  gap={{ initial: "6", md: "8" }}
  p="6"
>
  <Hero.Title
    align={{ initial: "center", md: "left" }}
    size={{ initial: "7", sm: "8", md: "9" }}
  >
    Launch your online store in minutes.
  </Hero.Title>
</Hero.Root>
```

**Guidelines:**
- Explain the breakpoint behavior in the description
- Show responsive props on related components together
- Use realistic content that benefits from responsive adjustments

### Slot-Based Composition

For components with semantic slots (Navbar, Footer), show how slots work together:

```tsx
<Footer p="8" gap="8">
  <Footer.Main>
    <Footer.Brand gap="5">
      <Avatar fallback="K" size="3" src="/logo.svg" />
      <Footer.Tagline>Company mission statement here.</Footer.Tagline>
      <Footer.Social>{/* social links */}</Footer.Social>
    </Footer.Brand>
    <Footer.Links>
      <Footer.LinkGroup title="Solutions">{/* links */}</Footer.LinkGroup>
      <Footer.LinkGroup title="Company">{/* links */}</Footer.LinkGroup>
    </Footer.Links>
  </Footer.Main>
  <Separator size="4" light />
  <Footer.Bottom>
    <Footer.Legal>{/* copyright */}</Footer.Legal>
  </Footer.Bottom>
</Footer>
```

### Educational Examples

Sometimes showing the architecture helps developers understand:

```tsx
// Navbar slot visualization example
<Navbar.Root position="static">
  <Navbar.Logo>
    <Flex style={slotStyle}>
      <Text size="2" color="gray">Logo Slot</Text>
    </Flex>
  </Navbar.Logo>
  <Navbar.Navigation style={{ flex: 1 }}>
    <Flex style={slotStyle}>
      <Text size="2" color="gray">Navigation Slot (expands)</Text>
    </Flex>
  </Navbar.Navigation>
  <Navbar.Actions>
    <Flex style={slotStyle}>
      <Text size="2" color="gray">Actions Slot</Text>
    </Flex>
  </Navbar.Actions>
</Navbar.Root>
```

Use educational examples sparingly—one per component maximum.

## Progressive Complexity

Order examples from simple to full-featured:

1. **Minimal** - Core component with essential props only
2. **Common** - Typical use case with common props
3. **Full-featured** - All slots/features demonstrated
4. **Advanced** - Responsive, interactive, or specialized patterns

Footer examples demonstrate this well:
1. Social Links (minimal)
2. Centered (common pattern)
3. 4-Column (full navigation)
4. Company Mission (everything together)

## Example Naming

### Use-Case Names (Preferred)

Name examples after what they accomplish:

| ✅ Good | ❌ Avoid |
|---------|----------|
| Form Submission | Solid Variant |
| Destructive Confirmation | Red Button |
| Dashboard Navbar | With User Menu |
| Company Mission | Full Footer |
| Compact Toolbar | Size 1 |

### Feature Names (Acceptable)

When demonstrating a specific capability, name the feature:

| ✅ Acceptable | Context |
|---------------|---------|
| Responsive Layout | Showing breakpoint behavior |
| Split Layout | Showing layout="split" |
| Tabs | Showing tab integration |

### Names to Avoid

- "Basic" → Use "Minimal" or a specific use case
- "With X" → Integrate the feature into a use case
- "Large/Small Y" → Show sizes in appropriate contexts

## Dogfooding

When the component being documented can introduce its own examples, use it:

```tsx
// SectionHeader examples use SectionHeader for headers
<Flex direction="column" gap="4">
  <SectionHeader.Root>
    <SectionHeader.Content>
      <SectionHeader.Title>With Tabs</SectionHeader.Title>
      <SectionHeader.Description>
        Tabs provide built-in separator, no need for separator prop
      </SectionHeader.Description>
    </SectionHeader.Content>
  </SectionHeader.Root>
  <PreviewBlock>{/* SectionHeader with tabs */}</PreviewBlock>
  <CodeBlock>{/* code */}</CodeBlock>
</Flex>
```

This demonstrates the component's utility while documenting it.

## Background Patterns

### Gradient Backgrounds

For hero sections or promotional contexts:

```tsx
<PreviewBlock
  height="40rem"
  appearance="dark"
  showThemeToggle={false}
  variant="ghost"
  background={{
    backgroundColor: "hsla(244, 100%, 50%, 1)",
    backgroundImage:
      "radial-gradient(circle at 50% 0%, hsla(319, 0%, 0%, 1) 49%, transparent 102%)",
    backgroundBlendMode: "normal",
    borderRadius: "var(--radius-3)",
  }}
>
  {/* content */}
</PreviewBlock>
```

### Dot Pattern

For documentation or technical contexts:

```tsx
<PreviewBlock height="40rem" background="dots">
  {/* content */}
</PreviewBlock>
```

## Block Component Checklist

Additional items for block-level components:

- [ ] Examples progress from simple to complex
- [ ] Layouts are shown in realistic contexts, not isolation
- [ ] Responsive props are demonstrated with clear breakpoint descriptions
- [ ] Slot composition is clear and documented
- [ ] At most one educational/visualization example
- [ ] Example names describe use cases or features, not props

## Reference Implementations

### Primitive Components
- **Button Examples**: `kookie-ui/apps/docs/app/docs/button/examples.tsx`
- **IconButton Examples**: `kookie-ui/apps/docs/app/docs/icon-button/examples.tsx`
- **ToggleButton Examples**: `kookie-ui/apps/docs/app/docs/toggle-button/examples.tsx`
- **ToggleIconButton Examples**: `kookie-ui/apps/docs/app/docs/toggle-icon-button/examples.tsx`
- **Navbar Examples**: `kookie-ui/apps/docs/app/docs/navbar/examples.tsx`

### Block Components
- **Hero Examples**: `kookie-blocks/apps/docs/app/docs/blocks/hero/examples.tsx`
- **Footer Examples**: `kookie-blocks/apps/docs/app/docs/blocks/footer/examples.tsx`
- **SectionHeader Examples**: `kookie-blocks/apps/docs/app/docs/blocks/section-header/examples.tsx`
- **PreviewBlock Examples**: `kookie-blocks/apps/docs/app/docs/blocks/preview-block/examples.tsx`
