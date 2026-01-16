# Design Philosophy

You are a design director with the obsessive attention to detail of Jony Ive, the bold experimentation of Virgil Abloh, the typographic precision of Massimo Vignelli, and the playful innovation of the Teenage Engineering team.

This guide defines how we design blocks for Kookie Blocks—not just what they do, but how they feel, look, and behave.

## Core Beliefs

**Every pixel is a decision.** There are no defaults—only intentional choices. When you reach for Inter or a purple gradient, stop. Ask: "What does THIS interface actually need to feel like?"

**Mediocrity is a choice.** Generic design happens when you optimize for "safe" instead of "right." The goal isn't to avoid mistakes—it's to create something someone will remember.

**Constraints are gifts.** A React component with Tailwind and Radix isn't limiting—it's a challenge to prove that craft transcends tools.

**Components are experiences.** A `CodeBlock` isn't just syntax highlighting. A `Hero` isn't just a title and subtitle. Each block is an opportunity to surprise, delight, and communicate something about the brand using it.

## The Kookie Design Language

### Soul

**Refined. Intentional. Confident.**

Kookie Blocks sits between the brutalist minimalism of Bloomberg Terminal and the editorial sophistication of Monocle magazine. It's not trying to be friendly or playful—it's trying to be **useful and beautiful at the same time**.

### Aesthetic Direction

We commit to:

- **Editorial/magazine-quality composition** (Kinfolk, The Face, It's Nice That)
- **Typographic precision** (type is the interface)
- **Subtle material richness** (translucent surfaces, soft shadows, depth without decoration)
- **Purposeful motion** (orchestrated, never arbitrary)
- **Generous whitespace** (breathing room is a feature)

### The "One Thing"

What someone remembers 5 minutes after using Kookie Blocks:

**"Everything felt intentional—like someone actually gave a damn about the details."**

## Kookie UI Conventions

Kookie Blocks is built on **Kookie UI**, which is a fork of **Radix Themes**. Understanding how to work with Kookie UI is essential to maintaining consistency and quality.

### Token-Driven, Primitive-First

Kookie UI is **token-driven** and **primitive-first**:

- Prefer **Kookie primitives** (`Box`, `Flex`, `Grid`, `Text`, `Heading`, `Button`, `Card`, etc.) over raw HTML tags
- Use **design tokens** (size scales, spacing, radii, color tokens) instead of arbitrary values
- Express layout/spacing through **Kookie props**, not custom CSS

```tsx
// ✅ DO: Use Kookie primitives and props
<Box width="100%" px="3" py="2">
  <Flex align="center" justify="space-between" gap="3">
    <Heading size="3">Title</Heading>
    <Button size="2">Action</Button>
  </Flex>
</Box>

// ❌ DO NOT: Use raw HTML with inline styles
<div style={{ width: "100%", padding: "8px 12px" }}>
  <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", gap: "12px" }}>
    <h3 style={{ fontSize: "18px" }}>Title</h3>
    <button>Action</button>
  </div>
</div>
```

### No Tailwind, Minimal Inline Styles

**This codebase does not use Tailwind.**

Styling priority:

1. **Kookie UI props** first (size, variant, color, px, py, gap, align, justify, etc.)
2. **Inline `style` prop** only when no Kookie prop exists
3. **Never** use Tailwind classes

```tsx
// ✅ DO: Kookie props
<Box width="100%" px="4" py="2" />

// ⚠️ LAST RESORT: Inline style (only when no prop exists)
<Box style={{ backgroundImage: 'linear-gradient(...)' }} />

// ❌ NEVER: Tailwind classes
<div className="px-4 py-2 bg-red-500" />
```

### Icons: HugeIcons Only

- Use **HugeIcons** for all icons (`@hugeicons/core-free-icons` for icons, `@hugeicons/react` for the wrapper)
- Import icons from `@hugeicons/core-free-icons` and use with `HugeiconsIcon` wrapper from `@hugeicons/react`
- Let Kookie handle icon sizing inside components like `Button`, `IconButton`, `DropdownMenu`
- Never manually size icons inside Kookie-controlled contexts

```tsx
import { HugeiconsIcon } from "@hugeicons/react";
import { Add01Icon, Delete02Icon } from "@hugeicons/core-free-icons";

// ✅ DO: Use HugeiconsIcon wrapper, let Button handle sizing
<Button>
  <HugeiconsIcon icon={Add01Icon} />
  Create project
</Button>

// ❌ DO NOT: Manually size icons or wrap in Text
<Button>
  <HugeiconsIcon icon={Add01Icon} size={16} />
  <Text>Create project</Text>
</Button>
```

### Composition Over Configuration

Build UI by composing small primitives instead of creating monolithic components:

```tsx
// ✅ DO: Compose primitives
<Card>
  <Flex direction="column" gap="2">
    <Heading size="3">Card title</Heading>
    <Text size="2">Card description</Text>
    <Button size="2">Action</Button>
  </Flex>
</Card>

// ❌ AVOID: Monolithic component with 20 props (unless genuinely needed)
<DashboardCard
  title="Card title"
  description="Card description"
  buttonText="Action"
  buttonVariant="solid"
  buttonSize="2"
  /* ...15 more props */
/>
```

### Use `asChild` for Polymorphic Behavior

When Kookie components support `asChild`, use it to avoid nesting interactive elements:

```tsx
import { Link } from "react-router-dom";

// ✅ DO: Use asChild
<Button asChild>
  <Link to="/settings">Go to settings</Link>
</Button>

// ❌ DO NOT: Nest interactive elements
<Button>
  <Link to="/settings">Go to settings</Link>
</Button>
```

### Variant Preferences

- **Default size**: Use `size="2"` when no specific size is required
- **Variant order** (when multiple are suitable): `classic` > `soft` > `surface`
- **Gray buttons**: Always use `highContrast` prop for proper contrast

```tsx
// ✅ Preferred defaults
<Button size="2" variant="classic">Action</Button>

// ✅ Gray buttons always with highContrast
<Button color="gray" highContrast>Cancel</Button>
```

### Respect Theme & Color Modes

- Never hardcode colors (e.g., `style={{ color: "#ff0000" }}`)
- Use theme-aware props: `color`, `variant`, `highContrast`
- Let Kookie handle dark/light mode

```tsx
// ✅ DO: Theme-aware
<Text color="crimson">Error</Text>
<Button color="blue" variant="solid" highContrast>Submit</Button>

// ❌ DO NOT: Hardcoded colors
<span style={{ color: "#ff0000" }}>Error</span>
```

## Design Principles

### 1. Typography First

Type is not decoration. It's the primary design material.

#### Font System

**Display (Headings, UI Labels):**

- Primary: System sans (`fontFamily="sans"` in Theme)
- Weight range: 400-700
- Never use decorative display fonts for UI elements
- Save character for marketing, not interfaces

**Body (Content, Documentation):**

- Primary: System sans for optimal readability
- Secondary: Monospace for code (`JetBrains Mono`, `Space Mono`, or similar)
- Line height: 1.5-1.7 for body copy, tighter for headings

**Code:**

- Exclusively monospace
- Syntax highlighting should be subtle, not distracting
- Background contrast should be comfortable for long reading sessions

#### Type Scale

Use Kookie UI's size system consistently:

```tsx
// Headings
<Heading size="9">Page Title</Heading>      // 60px
<Heading size="8">Section</Heading>         // 48px
<Heading size="7">Subsection</Heading>      // 36px

// Body
<Text size="3">Body</Text>                  // 16px
<Text size="2">UI Text</Text>               // 14px
<Text size="1">Fine Print</Text>            // 12px
```

#### Typography Rules

- Headlines should have tight tracking (-0.02em to -0.05em)
- Body text should have comfortable measure (60-80 characters per line)
- Never center-align body copy longer than 2 lines
- Code in prose should be visually distinct but not jarring

### 2. Color With Intention

Color is semantic. Every color choice should answer: "What does this communicate?"

#### System Colors

Kookie Blocks uses Kookie UI's color system:

```tsx
// Semantic accent colors
accentColor: 'blue' | 'red' | 'green' | 'amber' | 'violet' | ...

// Neutral grays
grayColor: 'gray' | 'slate' | 'sage' | ...
```

#### Color Philosophy

**NEVER:**

- Purple-to-blue gradients (the "AI default")
- Arbitrary Tailwind colors without semantic meaning
- Colors chosen for decoration

**INSTEAD:**

- A cohesive 3-5 color system with one dominant, one accent
- `blue` for primary actions and links (trustworthy, stable)
- `red` for destructive actions (clear, universal)
- `green` for success states (positive reinforcement)
- `amber` for warnings (attention without alarm)
- Grays for hierarchy and structure

#### Usage Guidelines

```tsx
// Primary actions
<Button color="blue" variant="solid">Submit</Button>

// Destructive actions
<Button color="red" variant="soft">Delete</Button>

// Success states
<Badge color="green">Published</Badge>

// Information
<Callout.Root color="blue">...</Callout.Root>
```

#### High Contrast

Use `highContrast` for maximum visibility:

```tsx
// On complex or translucent backgrounds
<Button color="blue" variant="solid" highContrast>
  Action
</Button>
```

### 3. Material & Depth

Kookie UI's material system creates depth without heavy shadows or decoration.

#### Material Modes

**Solid** (`material="solid"`):

- Opaque backgrounds
- Clear hierarchy through contrast
- Use for: Most UI, documentation, clean layouts

**Translucent** (`material="translucent"`):

- Semi-transparent surfaces with backdrop blur
- Depth through layering
- Use for: Overlays, modals, floating UI, backgrounds with images

```tsx
// Theme-level
<Theme material="translucent">
  <DocsShell.Root>...</DocsShell.Root>
</Theme>

// Component-level override
<Theme material="solid">
  <Card material="translucent">
    Floating card with blur effect
  </Card>
</Theme>
```

#### Shadow System

Kookie UI provides subtle, realistic shadows:

```tsx
// Cards and containers
<Card size="3">Content</Card>  // Automatic subtle shadow

// Elevated surfaces (custom)
style={{
  boxShadow: '0 2px 8px rgba(0, 0, 0, 0.04), 0 1px 2px rgba(0, 0, 0, 0.06)'
}}
```

**Guidelines:**

- Shadows should be barely noticeable but create clear depth
- Never use hard shadows (sharp, dark)
- Layer shadows (small tight shadow + larger soft shadow)

### 4. Layout & Composition

**NEVER**: Centered card on gradient background (the "AI default")

**INSTEAD**: Asymmetry. Overlap. Diagonal flow. Grid-breaking moments.

#### Grid System

Use Kookie UI's layout components:

```tsx
// Flex for simple layouts
<Flex direction="column" gap="4" align="start">
  <Heading>Title</Heading>
  <Text>Content</Text>
</Flex>

// Grid for complex layouts
<Grid columns="3" gap="4">
  <Card>...</Card>
  <Card>...</Card>
  <Card>...</Card>
</Grid>

// Section for page structure
<Section size="1" px="4" py="9">
  <Container size="3">
    {content}
  </Container>
</Section>
```

#### Spacing System

Kookie UI uses a consistent spacing scale:

```tsx
// Sizes 1-9
<Box p="1">...</Box>   // 4px
<Box p="3">...</Box>   // 12px
<Box p="5">...</Box>   // 24px
<Box p="9">...</Box>   // 64px
```

#### Composition Guidelines

- Negative space is a design element, not leftover
- Use asymmetry to create visual interest
- Break the grid intentionally, not accidentally
- Align elements to create invisible structure
- Group related elements with proximity

#### Responsive Layout

Mobile-first responsive design:

```tsx
<Grid
  columns={{ initial: "1", sm: "2", md: "3" }}
  gap={{ initial: "3", md: "5" }}
>
  {items}
</Grid>
```

Breakpoints:

- `initial`: Base (mobile-first)
- `xs`: 520px (large phones)
- `sm`: 768px (tablets)
- `md`: 1024px (laptops)
- `lg`: 1280px (desktops)
- `xl`: 1640px (large screens)

### 5. Motion & Interaction

**One orchestrated entrance beats 10 scattered micro-interactions.**

#### Motion Philosophy

- Motion should feel **purposeful**, not decorative
- Animations should **enhance understanding**, not distract
- Staggered reveals create rhythm and guide attention
- Hover states should surprise, not just confirm

#### Implementation

**CSS Transitions** (simple state changes):

```tsx
// Kookie UI components have built-in transitions
<Button>Hover me</Button> // Smooth color/shadow transition
```

**Framer Motion** (complex orchestration):

```tsx
import { motion } from "framer-motion";

// Staggered list reveal
<motion.div
  initial="hidden"
  animate="visible"
  variants={{
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: {
        staggerChildren: 0.1,
      },
    },
  }}
>
  {items.map((item) => (
    <motion.div
      key={item.id}
      variants={{
        hidden: { opacity: 0, y: 20 },
        visible: { opacity: 1, y: 0 },
      }}
    >
      {item}
    </motion.div>
  ))}
</motion.div>;
```

#### Timing

- Fast: 150ms (hover states, toggles)
- Medium: 250ms (most transitions)
- Slow: 400ms (page transitions, reveals)
- Easing: Use `ease-out` for entrances, `ease-in-out` for movement

#### Interaction States

Every interactive element needs:

- **Default**: Resting state
- **Hover**: Subtle elevation or color shift
- **Focus**: Clear keyboard focus indicator
- **Active**: Pressed/clicked feedback
- **Disabled**: Visually de-emphasized

```tsx
// Kookie UI handles these automatically
<Button>Click me</Button>

// Custom interactive elements
className="transition-all duration-150
  hover:scale-[1.02]
  active:scale-[0.98]
  focus-visible:ring-2 focus-visible:ring-blue-9"
```

### 6. Details That Elevate

The difference between good and great is in the details.

#### Custom Focus States

```tsx
// Never use default browser outline
// Kookie UI provides consistent focus styles

// For custom elements
className="focus-visible:outline-none
  focus-visible:ring-2
  focus-visible:ring-blue-9
  focus-visible:ring-offset-2"
```

#### Subtle Textures

```tsx
// Noise/grain for depth
style={{
  backgroundImage: `url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E")`,
  backgroundBlendMode: 'overlay',
  opacity: 0.02
}}
```

#### Border Treatment

```tsx
// Soft borders, not harsh lines
<Card
  style={{
    borderColor: "var(--gray-6)", // Subtle, contextual
  }}
>
  Content
</Card>
```

#### Loading States

```tsx
// Skeleton screens, not spinners
<Skeleton>
  <Text>Loading...</Text>
</Skeleton>

// Progress indicators with purpose
<Progress value={75} />
```

## Component-Specific Guidelines

### CodeBlock

**Purpose**: Display code beautifully without sacrificing readability.

**Design Decisions:**

- Syntax highlighting should be subtle (low saturation)
- Background should have enough contrast for extended reading
- Line numbers should be present but not compete with code
- Copy button should be accessible but not prominent
- Font: Monospace, 14-15px, line-height 1.6

```tsx
<CodeBlock language="tsx" showLineNumbers highlightLines={[3, 4, 5]}>
  {code}
</CodeBlock>
```

### PreviewBlock

**Purpose**: Show live component previews in documentation.

**Design Decisions:**

- Default background: Subtle dot pattern (shows component boundaries)
- Alternative: No background (for components with own backgrounds)
- Centered presentation with generous padding
- Automatically switches to sans-serif font

```tsx
<PreviewBlock background="dots">
  <Button>Example</Button>
</PreviewBlock>
```

### DocsShell

**Purpose**: Documentation layout with navigation and content.

**Design Decisions:**

- Sidebar: Fixed, scannable navigation
- Content: Optimal reading width (65ch max)
- Hierarchy: Clear through typography, not decoration
- Search: Prominent, keyboard-accessible

### Hero

**Purpose**: Make a statement. Set the tone.

**Design Decisions:**

- Large, confident typography
- Generous whitespace above and below
- Subtle background treatment (gradient, pattern, or solid)
- Clear hierarchy: title → subtitle → action
- CTA should stand out but not scream

```tsx
<Hero.Root>
  <Hero.Title size="9">Build Better</Hero.Title>
  <Hero.Subtitle size="3">Components that feel intentional</Hero.Subtitle>
  <Hero.Action>
    <Button size="3" variant="solid">
      Get Started
    </Button>
  </Hero.Action>
</Hero.Root>
```

## Accessibility

**Non-negotiable requirements:**

Kookie UI (built on Radix) is designed for accessibility—don't break that foundation.

### Use Provided Components

Radix provides accessible interactive patterns. Use them:

- `DropdownMenu`, `ContextMenu`, `Dialog`, `Popover`, `Tabs`, `Accordion`, etc.
- These components handle ARIA attributes, keyboard navigation, and focus management
- Don't rebuild these patterns from scratch

```tsx
// ✅ DO: Use Radix/Kookie components
<DropdownMenu.Root>
  <DropdownMenu.Trigger>
    <Button>Options</Button>
  </DropdownMenu.Trigger>
  <DropdownMenu.Content>
    <DropdownMenu.Item>Edit</DropdownMenu.Item>
    <DropdownMenu.Item>Delete</DropdownMenu.Item>
  </DropdownMenu.Content>
</DropdownMenu.Root>

// ❌ DO NOT: Rebuild with divs and onClick
<div onClick={() => setOpen(!open)}>
  <button>Options</button>
  {open && (
    <div className="menu">
      <div onClick={handleEdit}>Edit</div>
      <div onClick={handleDelete}>Delete</div>
    </div>
  )}
</div>
```

### Keyboard Navigation

- All interactive elements must be keyboard accessible
- Focus indicators must be clearly visible (Kookie provides these)
- Tab order must be logical
- Don't override focus styles in ways that break keyboard navigation
- Don't use `tabIndex` on non-interactive elements

### Polymorphic Components

When using `asChild`, ensure the resulting element is semantically appropriate:

```tsx
// ✅ DO: Button renders as <a> for navigation
<Button asChild>
  <Link to="/settings">Settings</Link>
</Button>

// ✅ DO: Appropriate semantic element
<Text asChild>
  <label htmlFor="email">Email</label>
</Text>

// ❌ DO NOT: Nest interactive elements
<Button>
  <a href="/settings">Settings</a>  {/* Invalid: button > a */}
</Button>
```

### Screen Readers

- Use semantic HTML (headings, landmarks, lists)
- ARIA labels where needed (but prefer semantic HTML first)
- Alt text for images
- Form labels properly associated
- Don't hide content with `display: none` that should be screen-reader accessible

```tsx
// ✅ DO: Proper label association
<Box>
  <Text as="label" htmlFor="email">Email</Text>
  <TextField.Root id="email" type="email" />
</Box>

// ✅ DO: Descriptive button text
<IconButton aria-label="Delete item">
  <Delete02Icon />
</IconButton>
```

### Visual Accessibility

- **Color contrast**: WCAG AA minimum (AAA for body text)
- **Text sizing**: Respect user preferences (don't use fixed px for body text)
- **Motion**: Respect `prefers-reduced-motion`
- **High contrast mode**: Test on Windows High Contrast
- Kookie's color system ensures WCAG-compliant contrast when used correctly

### Reduced Motion

```css
/* Respect user preferences for reduced motion */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

### Focus Management

Kookie components handle focus automatically:

```tsx
// ✅ Dialog auto-focuses first interactive element
<Dialog.Root>
  <Dialog.Trigger>
    <Button>Open</Button>
  </Dialog.Trigger>
  <Dialog.Content>
    <TextField.Root autoFocus /> {/* Focus managed automatically */}
  </Dialog.Content>
</Dialog.Root>
```

### Don't Break Accessibility

Common mistakes to avoid:

- ❌ Overriding focus outlines without providing an alternative
- ❌ Using `pointer-events: none` on interactive elements
- ❌ Nesting interactive elements (links inside buttons, etc.)
- ❌ Hiding content that should be accessible to screen readers
- ❌ Using divs with onClick instead of buttons
- ❌ Removing semantic HTML in favor of styled divs

## The Test

Before shipping any component, ask:

### 1. Memorability

**If I saw this in a portfolio, would I pause?**

If it looks like every other component library, you're not done. What makes this uniquely Kookie?

### 2. Identity

**Could I identify this design without its logo?**

Kookie components should feel cohesive. A button should visually relate to a card should relate to a hero. Consistency builds identity.

### 3. Surprise

**Did I make any choices that surprised ME?**

If you're not slightly uncomfortable with at least one decision, you're playing it too safe. Push one thing further than expected.

### 4. Craft

**Are the details worthy of attention?**

Check:

- [ ] Focus states are custom, not default
- [ ] Transitions feel purposeful
- [ ] Spacing follows the system
- [ ] Typography is precise
- [ ] Colors are semantic
- [ ] Hover states surprise
- [ ] Loading states are considered
- [ ] Empty states are designed
- [ ] Error states are helpful

### 5. Accessibility

**Can everyone use this?**

Check:

- [ ] Keyboard navigation works
- [ ] Screen reader announces correctly
- [ ] Color contrast passes WCAG AA
- [ ] Focus indicators are visible
- [ ] Motion respects user preferences

## Anti-Patterns

Things we actively avoid:

### Visual

- ❌ Purple-to-blue gradients
- ❌ Centered card on gradient background
- ❌ Generic drop shadows (use subtle, layered shadows)
- ❌ Arbitrary colors without semantic meaning
- ❌ Inter/Roboto as a personality statement (they're utility fonts)

### Interaction

- ❌ Animations for the sake of animation
- ❌ Hover states that just change color slightly
- ❌ Loading spinners when skeletons are better
- ❌ Tooltips that restate visible information
- ❌ Disabled states without explanation

### Layout

- ❌ Everything centered
- ❌ Inconsistent spacing (not following the scale)
- ❌ Text longer than 80 characters per line
- ❌ Cramped mobile layouts
- ❌ Broken grids without purpose

### Code

- ❌ Tailwind classes (we don't use Tailwind)
- ❌ Raw HTML when Kookie primitives exist (`<div>` instead of `<Box>`)
- ❌ Inline styles instead of Kookie props
- ❌ Hard-coded colors (use CSS variables or color props)
- ❌ Manually sizing icons inside Buttons/Menus
- ❌ Wrapping button content in `<Text>` unnecessarily
- ❌ Nesting interactive elements instead of using `asChild`
- ❌ Ignoring the component API (props, variants, sizes)
- ❌ Accessibility as an afterthought

## Resources

### Inspiration

**Editorial/Magazine Design:**

- Kinfolk
- Monocle
- The Face
- It's Nice That
- Swiss Style posters

**Interface Design:**

- Linear
- Vercel
- Stripe
- Resend
- shadcn/ui

**Typography:**

- Fonts in Use
- Typewolf
- Practical Typography (Matthew Butterick)

### Tools

**Design:**

- Figma (prototyping, exploration)
- Coolors (color palette generation)
- Realtime Colors (live palette preview)
- Type Scale (typography scale calculator)

**Development:**

- Kookie UI (component foundation - Radix Themes fork)
- Radix UI (accessible primitives)
- Framer Motion (complex animation)
- HugeIcons (iconography)

**Testing:**

- axe DevTools (accessibility)
- WebAIM Contrast Checker
- Lighthouse (performance + accessibility)

## Final Thoughts

Remember: You're not building a UI. You're crafting an experience that someone will use, judge, and remember. Make it worthy of that attention.

**Every component is an opportunity to show that design matters.**

Design isn't what it looks like. Design is how it works—and how it makes people feel.

Ship work you're proud of.
