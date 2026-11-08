# Tech Stack

## Core

| Layer | Technology | Version | Used for |
|---|---|---|---|
| Framework | **Next.js** (App Router) | 14.2 | Routing, static pre-rendering, image and font optimisation |
| UI library | **React** | 18.3 | Components and state |
| Language | **TypeScript** | 5.x | Type safety across components and data |
| Styling | **Tailwind CSS** | 3.4 | Utility classes, brand colours, custom keyframes |
| CSS tooling | PostCSS + Autoprefixer | — | Tailwind build and vendor prefixes |

## Animation and scrolling

| Library | Version | Used for |
|---|---|---|
| **GSAP** (+ ScrollTrigger) | 3.x | Preloader timeline, hero headline intro, tub conveyor loop, headline parallax |
| **Framer Motion** | 11.x | Section reveals, scroll-linked values (`useScroll`, `useTransform`), cart drawer, toasts |
| **Lenis** | 1.x | Smooth scrolling; one shared instance drives the tub-to-cup flight |
| HTML Canvas 2D | — | Draws the Our Story image sequence frame by frame |

## Fonts

Loaded with `next/font/google` (self-hosted at build time, `display: swap`):

- **Fraunces** (500–900): display headings
- **Poppins** (400–700): body text

## Brand tokens (Tailwind)

| Token | Hex |
|---|---|
| `swirl-pink` | `#f3c9d4` |
| `swirl-pink-soft` | `#f6d5df` |
| `swirl-burgundy` | `#5a1a2e` |
| `swirl-burgundy-deep` | `#3d0f1f` |
| Page background | `#fcecf0` |
| Festive gold (offers) | `#f2b544` |

Custom animations: `ribbon` / `ribbon-reverse` (marquees), `spin-slow` (badge),
`flicker` (diya flame), `twinkle` (sparkles).

## State and data

- **Cart:** React Context (`CartProvider`), persisted to `localStorage`
  (`swirl-cart`), with add / set quantity / clear / open / close.
- **Content:** plain TypeScript modules in `data/`: `products.ts` (hero tubs),
  `flavors.ts` (cups and prices), `promos.ts` (best sellers and offers),
  `reviews.ts`, `story.ts`.
- **Cross-component signal:** a tiny shared object (`flightState.ts`) tells the
  flavors marquee which cup the flying hero tub is heading for.

## Media pipeline

- Product tubs and cups: transparent **WebP**, 1000×1000, served through
  `next/image` (AVIF/WebP negotiation, responsive `sizes`).
- Our Story animation: a green-screen video converted into **211 transparent
  WebP frames** (30fps, 640×880) with `ffmpeg` (extract and crop) and `sharp`
  (chroma key, despill, edge feathering).

## Performance measures

- Images compressed from ~33MB of PNG to ~1.8MB of WebP.
- Our Story frames load only when the section is within about two screens.
- Preloader shortened to ~1.6s; hero text starts at 0.9s.
- Marquees and the conveyor pause work when off-screen (IntersectionObserver).
- `prefers-reduced-motion` respected: no drifting, no conveyor, last video frame only.

## Project structure

```
app/
  layout.tsx          fonts, metadata
  page.tsx            section order
  globals.css
components/
  Hero.tsx  Navbar.tsx  HeroHeadline.tsx  CTAButton.tsx  FlavorSelector.tsx
  ProductOrbit.tsx    tub conveyor
  FlyObject.tsx       tub-to-cup flight on scroll
  FavoriteFlavors.tsx cup marquee
  Preloader.tsx  SmoothScroll.tsx  brand.tsx  flightState.ts
  cart/               CartProvider, CartDrawer, AddToCart
  sections/           OurStory, StoryFrames, BestSellers, DiwaliOffers,
                      Reviews, Footer, SectionHeading
data/                 products, flavors, promos, reviews, story
hooks/useLenis.ts
public/images/        Cups/, containers/, story-frames/
```
