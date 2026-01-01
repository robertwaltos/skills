---
name: frontend-design
description: |
  Create distinctive, production-grade frontend interfaces with exceptional design quality that rivals top design agencies. Embodies expertise of a Senior UI/UX Designer with 12+ years at studios like Pentagram, IDEO, or Apple.
  Use when: user requests web components, landing pages, dashboards, React/Vue components, HTML/CSS layouts, web UI styling, single-page applications, or any interactive web interface.
  Do NOT use when: user needs static print design (use canvas-design) or complex multi-component artifacts (use web-artifacts-builder).
  Outputs: .html, .jsx, .tsx, .css, .vue files with production-ready code.
  Keywords: website, landing page, dashboard, React, Vue, HTML, CSS, UI, UX, web design, component, interface, frontend, responsive
license: Complete terms in LICENSE.txt
---

# Frontend Design Skill

**Expertise Level**: Senior UI/UX Designer / Creative Director (12+ years equivalent)

This skill embodies the methodology of world-class digital design studios—producing interfaces that would be at home in Awwwards galleries or Apple keynotes. Every output avoids generic "AI slop" aesthetics through deliberate, distinctive creative choices.

---

## Related Skills

- **[canvas-design](../canvas-design)**: For static visual art/posters (non-interactive)
- **[web-artifacts-builder](../web-artifacts-builder)**: For complex multi-component React artifacts with routing and state management
- **[theme-factory](../theme-factory)**: Apply consistent themes to frontend work

---

## Design Thinking Process

Before writing any code, complete this design thinking phase:

### 1. Context Analysis

- **Purpose**: What problem does this interface solve? Who uses it?
- **Audience**: Technical users? Consumers? Enterprise? Creative professionals?
- **Platform**: Desktop-first? Mobile-first? Both?
- **Brand context**: Existing brand to match? Greenfield opportunity?

### 2. Aesthetic Direction Selection

Pick a **BOLD** direction and commit fully. Mediocrity comes from hedging:

| Direction | Characteristics | Best For |
|-----------|-----------------|----------|
| **Brutally Minimal** | Stark white space, sharp typography, essential elements only | Portfolio, luxury, editorial |
| **Maximalist Chaos** | Dense information, layered elements, controlled overwhelming | Creative agencies, music, fashion |
| **Retro-Futuristic** | Vintage computing meets sci-fi, CRT effects, terminal aesthetics | Tech products, developer tools |
| **Organic/Natural** | Soft curves, earthy colors, flowing shapes | Wellness, sustainability, lifestyle |
| **Luxury/Refined** | Premium materials, subtle animations, exquisite typography | High-end products, finance |
| **Playful/Toy-like** | Bold primaries, bouncy animations, rounded forms | Children's products, casual apps |
| **Editorial/Magazine** | Strong grid, dramatic typography, editorial rhythm | Content platforms, news, blogs |
| **Brutalist/Raw** | Exposed structure, stark contrasts, anti-design | Art, avant-garde, statement pieces |
| **Art Deco/Geometric** | Symmetry, gold accents, ornamental geometry | Events, luxury venues, heritage brands |
| **Industrial/Utilitarian** | Functional beauty, exposed mechanics, tool-like | SaaS, productivity, B2B |
| **Neo-Memphis** | Bold shapes, clashing colors, 80s revival | Creative tools, youth brands |
| **Swiss/International** | Grid perfection, Helvetica heritage, systematic | Corporate, information design |

### 3. The Differentiation Question

Answer: **What single thing will someone remember after 5 seconds?**

This becomes the design's focal point—everything else supports it.

---

## Implementation Framework

### Typography Excellence

**Font Selection Principles:**

```
NEVER USE:
- Inter, Roboto, Arial, system-ui (generic)
- Space Grotesk (overused in AI outputs)
- Any "safe" default choice

ALWAYS SEEK:
- Distinctive display fonts with character
- Refined body fonts with excellent readability
- Unexpected pairings that create tension
```

**Recommended Font Sources:**
- Google Fonts (curated selections only)
- Font Squirrel
- Adobe Fonts
- Commercial foundries for premium work

**Pairing Framework:**
| Display Font Type | Body Font Type | Effect |
|-------------------|----------------|--------|
| Geometric Sans | Humanist Serif | Modern sophistication |
| Slab Serif | Geometric Sans | Industrial elegance |
| Script/Display | Clean Sans | Playful professionalism |
| Condensed Bold | Wide Regular | Dynamic tension |
| Monospace | Serif | Technical editorial |

### Color System Architecture

**The 60-30-10 Rule with CSS Variables:**

```css
:root {
  /* 60% - Dominant (backgrounds, large areas) */
  --color-primary: #...;
  --color-surface: #...;
  
  /* 30% - Secondary (cards, sections) */
  --color-secondary: #...;
  --color-muted: #...;
  
  /* 10% - Accent (CTAs, highlights) */
  --color-accent: #...;
  --color-accent-hover: #...;
  
  /* Semantic */
  --color-text: #...;
  --color-text-muted: #...;
}
```

**Color Palettes to AVOID:**
- Purple gradients on white (the "AI default")
- Blue + Orange (overdone)
- Generic Material Design palettes
- Any palette that feels "template-y"

**Color Palettes that WORK:**
- Monochromatic with single bold accent
- Earth tones with unexpected neon accent
- Dark mode with luminous highlights
- Desaturated palette with saturated accent
- Analogous colors with complementary accent

### Motion & Animation

**Hierarchy of Impact:**
1. **Page load orchestration** (highest impact) - Staggered reveals
2. **Scroll-triggered animations** - Content entering viewport
3. **Hover states** - Interactive feedback
4. **Micro-interactions** (lowest impact) - Button clicks, toggles

**CSS Animation Principles:**

```css
/* Timing functions that feel premium */
--ease-out-expo: cubic-bezier(0.19, 1, 0.22, 1);
--ease-in-out-cubic: cubic-bezier(0.65, 0, 0.35, 1);
--spring: cubic-bezier(0.175, 0.885, 0.32, 1.275);

/* Duration guidelines */
/* Micro (buttons, toggles): 150-200ms */
/* Standard (modals, reveals): 300-400ms */
/* Dramatic (page transitions): 500-800ms */
```

**Stagger Pattern:**

```css
.item { animation: fadeInUp 0.6s var(--ease-out-expo) backwards; }
.item:nth-child(1) { animation-delay: 0.1s; }
.item:nth-child(2) { animation-delay: 0.2s; }
.item:nth-child(3) { animation-delay: 0.3s; }
```

### Spatial Composition

**Layout Techniques for Distinctive Design:**

| Technique | Effect | CSS Approach |
|-----------|--------|--------------|
| **Asymmetric Grid** | Dynamic tension | CSS Grid with unequal columns |
| **Overlap** | Depth and interest | Negative margins, absolute positioning |
| **Diagonal Flow** | Energy and movement | Transform: skew, clip-path |
| **Grid Breaking** | Emphasis and surprise | Elements escaping containers |
| **Generous Whitespace** | Luxury and breathing room | Increased padding/margins |
| **Controlled Density** | Information-rich energy | Tight spacing, small multiples |

### Background & Texture

**Beyond Solid Colors:**

```css
/* Gradient meshes */
background: radial-gradient(at 40% 20%, hsla(28,100%,74%,1) 0px, transparent 50%),
            radial-gradient(at 80% 0%, hsla(189,100%,56%,1) 0px, transparent 50%),
            radial-gradient(at 0% 50%, hsla(355,100%,93%,1) 0px, transparent 50%);

/* Noise texture overlay */
background-image: url("data:image/svg+xml,..."); /* inline SVG noise */

/* Grain effect */
filter: contrast(1.1) brightness(1.02);

/* Glassmorphism */
backdrop-filter: blur(10px);
background: rgba(255, 255, 255, 0.1);
```

---

## Quality Checklist

Before delivering any frontend work:

- [ ] **Aesthetic direction is clear and consistent** - Not hedged or generic
- [ ] **Typography is distinctive** - No Inter, Roboto, or safe defaults
- [ ] **Color palette is cohesive** - CSS variables, 60-30-10 rule
- [ ] **Animations are purposeful** - High-impact orchestration over scattered effects
- [ ] **Layout breaks expectations** - Not standard template patterns
- [ ] **Responsive behavior is considered** - Works across breakpoints
- [ ] **Code is production-quality** - Clean, semantic, accessible
- [ ] **One memorable element exists** - The thing people will remember

---

## Anti-Patterns (Never Do These)

❌ **Generic Hero Patterns**: Centered headline + subheadline + two buttons + stock illustration
❌ **Card Grid Monotony**: Equal-sized cards in perfect rows
❌ **Default Shadows**: `box-shadow: 0 4px 6px rgba(0,0,0,0.1)`
❌ **Template Navbars**: Logo left, links center, CTA right
❌ **Purple Gradients**: The calling card of AI-generated design
❌ **Rounded Everything**: `border-radius: 8px` on every element
❌ **Safe Color Choices**: Blue primary, gray secondary
❌ **Illustration Libraries**: Generic undraw/humaaans style illustrations

---

## Expert Techniques

### The "One Thing" Principle

Every interface should have ONE distinctive element that makes it memorable:
- An unusual navigation pattern
- A dramatic typographic treatment
- An unexpected color choice
- A signature animation
- A creative interaction pattern

Design that "one thing" first, then build everything else to support it.

### Optical Alignment

Mathematical alignment ≠ visual alignment. Adjust elements so they *appear* aligned:
- Text often needs slight leftward adjustment to align with geometric shapes
- Triangles/arrows need to extend past the "edge" to appear centered
- Rounded corners create optical insets

### Creating Depth Without Shadows

- **Color value shifts**: Slightly darker/lighter variants suggest depth
- **Border definition**: Subtle 1px borders define edges
- **Gradient overlays**: Linear gradients suggest light direction
- **Layered backgrounds**: Multiple background layers create depth

### Responsive Typography

```css
/* Fluid typography that scales beautifully */
font-size: clamp(1rem, 0.5rem + 2vw, 2rem);

/* Variable font axis manipulation */
font-variation-settings: 'wght' var(--font-weight), 'wdth' var(--font-width);
```

---

*This skill embodies 12+ years of UI/UX design expertise at world-class studios. Every output should feel hand-crafted by a senior designer who obsesses over every pixel, every animation timing, every typographic detail.*
