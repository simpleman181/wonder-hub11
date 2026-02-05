# Neal.fun Technical Specification

## 1. Tech Stack Overview

| Category | Technology |
|----------|------------|
| Framework | React 18 + TypeScript |
| Build Tool | Vite |
| Styling | Tailwind CSS 3.4 |
| UI Components | shadcn/ui |
| Animation | Framer Motion |
| Icons | Lucide React |
| Fonts | Comic Sans MS (system), Inter (system-ui) |

## 2. Tailwind Configuration

```javascript
// tailwind.config.js extensions
{
  theme: {
    extend: {
      fontFamily: {
        'comic': ['"Comic Sans MS"', 'cursive'],
        'sans': ['system-ui', '-apple-system', 'sans-serif'],
      },
      colors: {
        'card-green': '#00AA00',
        'card-red': '#FF0044',
        'card-blue': '#44AAFF',
        'card-yellow': '#FFCC00',
      },
      animation: {
        'float': 'float 3s ease-in-out infinite',
      },
      keyframes: {
        float: {
          '0%, 100%': { transform: 'translateY(0)' },
          '50%': { transform: 'translateY(-5px)' },
        },
      },
    },
  },
}
```

## 3. Component Inventory

### Shadcn/UI Components (Pre-installed)
- Button (for "Get new posts")

### Custom Components

| Component | Props | Description |
|-----------|-------|-------------|
| Header | - | Logo, tagline, floating plant |
| NewsletterButton | - | Fixed position newsletter CTA |
| ProjectGrid | projects: Project[] | 3-column grid of project cards |
| ProjectCard | project: Project | Individual project card with hover |
| Footer | - | Links and contact info |
| FloatingPlant | - | Animated plant decoration |

### Type Definitions

```typescript
interface Project {
  id: string;
  title: string;
  url: string;
  image: string;
  bgColor?: string;
}
```

## 4. Animation Implementation Plan

| Interaction Name | Tech Choice | Implementation Logic |
|------------------|-------------|---------------------|
| Page Load Fade | Framer Motion | `initial={{ opacity: 0 }}` `animate={{ opacity: 1 }}` on main container |
| Card Stagger | Framer Motion | `staggerChildren: 0.05` on grid, `variants` on cards |
| Plant Float | CSS Animation | `animate-float` class with keyframes |
| Card Hover | Tailwind + FM | `whileHover={{ scale: 1.02 }}` with shadow transition |
| Button Hover | Tailwind | `hover:bg-gray-100` transition |
| Link Hover | Tailwind | `hover:underline` |

### Animation Specifications

```typescript
// Card container variants
const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.05,
      delayChildren: 0.3,
    },
  },
};

// Individual card variants
const cardVariants = {
  hidden: { opacity: 0, y: 20 },
  visible: {
    opacity: 1,
    y: 0,
    transition: {
      duration: 0.3,
      ease: [0.4, 0, 0.2, 1],
    },
  },
};

// Card hover
const cardHover = {
  scale: 1.02,
  transition: { duration: 0.2, ease: 'easeOut' },
};
```

## 5. Project File Structure

```
/mnt/okcomputer/output/app/
├── src/
│   ├── components/
│   │   ├── Header.tsx
│   │   ├── NewsletterButton.tsx
│   │   ├── ProjectGrid.tsx
│   │   ├── ProjectCard.tsx
│   │   ├── Footer.tsx
│   │   └── FloatingPlant.tsx
│   ├── data/
│   │   └── projects.ts
│   ├── types/
│   │   └── index.ts
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── public/
│   └── images/
│       └── (project card images)
├── index.html
├── tailwind.config.js
├── vite.config.ts
└── package.json
```

## 6. Package Installation

```bash
# Already included in init
# - react, react-dom
# - typescript
# - vite
# - tailwindcss
# - @radix-ui/react-*
# - class-variance-authority
# - clsx
# - tailwind-merge

# Additional needed
npm install framer-motion
npm install lucide-react
```

## 7. Responsive Breakpoints

| Breakpoint | Grid Columns | Changes |
|------------|--------------|---------|
| Mobile (<640px) | 1 | Stack all, hide plant |
| Tablet (640-1024px) | 2 | 2-column grid |
| Desktop (>1024px) | 3 | Full layout, show plant |

## 8. Performance Considerations

- Use `will-change: transform` on animated elements
- Lazy load project images below the fold
- Use `transform` and `opacity` for animations (GPU accelerated)
- Preload critical images (plant, first row of cards)
