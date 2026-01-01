---
name: brand-guidelines
description: |
  Apply Anthropic's official brand identity—colors, typography, and visual language—to artifacts. Embodies expertise of a Brand Manager ensuring visual consistency across all touchpoints.
  Use when: user requests Anthropic branding, brand colors, company styling, visual formatting with brand standards, or styling artifacts with official guidelines.
  Outputs: Branded versions of existing artifacts (presentations, documents, web interfaces).
  Keywords: Anthropic brand, brand colors, brand guidelines, corporate identity, visual identity, brand styling, company branding
license: Complete terms in LICENSE.txt
---

# Anthropic Brand Guidelines Skill

**Expertise Level**: Brand Manager / Visual Identity Specialist

This skill applies Anthropic's official brand identity to artifacts, ensuring visual consistency and professional brand representation.

---

## Related Skills

- **[canvas-design](../canvas-design)**: Create visual art that can then receive brand styling
- **[pptx](../pptx)**: Create presentations that can be branded
- **[frontend-design](../frontend-design)**: Create web interfaces with brand styling

---

## Overview

To access Anthropic's official brand identity and style resources, use this skill.

**Keywords**: branding, corporate identity, visual identity, post-processing, styling, brand colors, typography, Anthropic brand, visual formatting, visual design

## Brand Guidelines

### Color System

**Primary Palette:**

| Color | Hex | Usage |
|-------|-----|-------|
| Dark | `#141413` | Primary text, dark backgrounds |
| Light | `#faf9f5` | Light backgrounds, text on dark |
| Mid Gray | `#b0aea5` | Secondary elements, borders |
| Light Gray | `#e8e6dc` | Subtle backgrounds, dividers |

**Accent Palette:**

| Color | Hex | Usage |
|-------|-----|-------|
| Orange | `#d97757` | Primary accent, CTAs, highlights |
| Blue | `#6a9bcc` | Secondary accent, links, information |
| Green | `#788c5d` | Tertiary accent, success states |

**Color Application Rules:**
- Use Dark (`#141413`) for primary text on light backgrounds
- Use Light (`#faf9f5`) for text on dark backgrounds
- Limit accent colors to highlights and interactive elements
- Maintain sufficient contrast (WCAG AA minimum)

### Typography System

| Element | Font | Fallback | Weight |
|---------|------|----------|--------|
| Headings (24pt+) | Poppins | Arial | 500-700 |
| Body Text | Lora | Georgia | 400 |
| UI Elements | Poppins | Arial | 400-500 |
| Code/Monospace | System mono | Consolas | 400 |

**Typography Guidelines:**
- Headings: Poppins for clean, modern authority
- Body: Lora for warm, readable elegance
- Pre-install fonts for best results; fallbacks ensure compatibility
- Maintain clear hierarchy with size, weight, and spacing

### Visual Language

**Design Principles:**
1. **Clarity**: Clean layouts with generous whitespace
2. **Warmth**: Approachable through rounded elements and warm colors
3. **Intelligence**: Refined details that suggest thoughtfulness
4. **Trust**: Consistent, professional execution

**Avoid:**
- Harsh contrasts or neon colors
- Cluttered layouts
- Overly playful or casual styling
- Generic stock imagery

---

## Application Process

### For Presentations (PPTX)

1. Apply Poppins to slide titles and headings (24pt+)
2. Apply Lora to body text and bullet points
3. Set background to Light (`#faf9f5`) or Dark (`#141413`)
4. Use accent colors for shapes, highlights, and emphasis
5. Cycle through Orange → Blue → Green for multiple accent elements

### For Documents

1. Headings: Poppins, appropriate hierarchy sizes
2. Body: Lora, 11-12pt for readability
3. Links: Blue (`#6a9bcc`)
4. Emphasis: Orange (`#d97757`) sparingly

### For Web Interfaces

```css
:root {
  /* Brand colors */
  --brand-dark: #141413;
  --brand-light: #faf9f5;
  --brand-gray: #b0aea5;
  --brand-gray-light: #e8e6dc;
  
  /* Accents */
  --brand-orange: #d97757;
  --brand-blue: #6a9bcc;
  --brand-green: #788c5d;
  
  /* Typography */
  --font-heading: 'Poppins', Arial, sans-serif;
  --font-body: 'Lora', Georgia, serif;
}
```

---

## Technical Implementation

### Font Management

- Fonts should be pre-installed in the environment
- Automatic fallback to Arial (headings) and Georgia (body)
- No installation required—works with system fonts

### Color Application

- Use RGB color values for precise brand matching
- Apply via python-pptx's RGBColor class for presentations
- Use CSS variables for web implementations
- Maintain color fidelity across different systems

---

## Quality Checklist

- [ ] All headings use Poppins (or Arial fallback)
- [ ] All body text uses Lora (or Georgia fallback)
- [ ] Color palette strictly follows brand guidelines
- [ ] Accent colors used sparingly and purposefully
- [ ] Sufficient contrast maintained throughout
- [ ] Layout feels clean and professional
- [ ] Overall aesthetic conveys warmth and intelligence

---

*This skill ensures every artifact reflects Anthropic's brand identity with the precision of an in-house brand team.*
