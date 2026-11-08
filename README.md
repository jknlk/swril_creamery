# Swirl Creamery — Website Docs

Documentation for the **Swirl Creamery** website: a single-page, scroll-animated
landing page for a premium ice cream brand, with a working cart.

| File | What's in it |
|---|---|
| [TECH_STACK.md](TECH_STACK.md) | Frameworks, libraries, tooling and how the project is organised |
| [PSEUDOCODE.md](PSEUDOCODE.md) | Pseudo code for every section and the main animation logic |
| [PROMPT.md](PROMPT.md) | A build prompt that describes the whole site, for recreating or extending it with an AI coding assistant |

## The page at a glance

```
┌─────────────────────────────────────────────┐
│ Preloader   logo grows, then zooms open     │
├─────────────────────────────────────────────┤
│ Hero        tub conveyor, tinted background │
├─────────────────────────────────────────────┤
│ Flavors     endless cup marquee + cart      │
├─────────────────────────────────────────────┤
│ Our Story   scroll-scrubbed video frames    │
├─────────────────────────────────────────────┤
│ Best Sellers  top 4 ranked cups             │
├─────────────────────────────────────────────┤
│ Diwali Offers festive bundles + codes       │
├─────────────────────────────────────────────┤
│ Reviews     two scrolling rows of cards     │
├─────────────────────────────────────────────┤
│ Contact     footer with email and socials   │
└─────────────────────────────────────────────┘
  + floating cart button and slide-in cart drawer
```

## Running it

```bash
npm install
npm run dev      # http://localhost:3000
npm run build    # production build
npm start        # serve the production build
```
