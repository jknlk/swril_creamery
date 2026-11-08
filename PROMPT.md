# Build Prompt

The prompt below describes the full Swirl Creamery website. Give it to an AI
coding assistant to recreate the site or as a reference when extending it.
Swap in your own images, copy, prices and offers.

---

```
Build a single-page website for "Swirl Creamery", a premium ice cream brand.
Tagline: "Ice cream, just the way you like it."

TECH STACK
- Next.js 14 (App Router) + React 18 + TypeScript
- Tailwind CSS 3 for styling
- GSAP 3 (with ScrollTrigger) for timelines, Framer Motion 11 for reveals and
  scroll-linked values, Lenis for smooth scrolling
- next/font: Fraunces (display headings), Poppins (body)
- next/image for all product images (transparent WebP)

BRAND
- Colours: page background #fcecf0, pink #f3c9d4, soft pink #f6d5df,
  burgundy #5a1a2e, deep burgundy #3d0f1f, festive gold #f2b544
- Headings: big uppercase Fraunces, first word outlined (text-stroke),
  second line solid deep burgundy, small spaced-out eyebrow label above
- Logo: soft-serve-in-a-cone mark + italic "Swirl" + spaced "CREAMERY"
- Rounded cards (2rem radius), soft burgundy shadows, playful but premium

SECTIONS (in order)

1. Preloader (~1.6s): full-screen deep-burgundy sheet with the logo cut out of
   it as an SVG mask so the hero shows through the letters. The logo starts
   tiny, grows to full size with a slight bounce, then zooms through the
   letters until the hero fills the screen, and fades away. Lock scrolling
   while it plays.

2. Hero: navbar (logo, links Flavors / Our Story / Best Sellers / Offers /
   Contact, social icons). Headline on the right: "ICE CREAM" outlined,
   "JUST THE WAY YOU LIKE IT" solid. On the left, five ice cream tubs on a
   diagonal conveyor, small at the top-back and large at the bottom-front;
   every ~1.4s each tub moves one stop forward, the front tub exits at the
   bottom and a twin enters from the top. The hero background fades to a soft
   tint of the front tub's container colour (strawberry → soft pink,
   pistachio → pale green, vanilla → butter yellow, and so on). Bottom left:
   "Premium ice cream. Playful flavors. Made to swirl." and a Shop Now button.
   Bottom right: colour swatches; clicking one runs the conveyor quickly until
   that tub is in front.

3. Flavors: "Your Favorite Flavors". An endless horizontal marquee of nine
   ice cream cups, each over a spinning spiral swirl in its flavour colour,
   with name, price and an Add to cart button. Draggable and flickable, sideways
   trackpad scroll supported. On scroll, the hero's front tub flies down and
   morphs into the matching cup in the marquee, and the marquee glides that cup
   to the centre to catch it.

4. Our Story: "Made To Swirl". Left: a paragraph whose words light up one by
   one as you scroll, plus three value cards (Small batch, Real ingredients,
   Nine flavors). Right: a transparent image sequence (green screen removed)
   of toppings falling into a Swirl Creamery cup, drawn on a canvas and
   scrubbed by scroll. The section pins once it reaches the top of the screen
   and the sequence plays while pinned. A circular text badge
   ("Small batch • Real ingredients • Made to swirl") spins on its corner.
   Load the frames only when the section is near.

5. Best Sellers: "Best Sellers". Four ranked cards (#1–#4) with a tag
   ("Most loved"…), the cup on a circle in its flavour colour, name,
   "N cups sold" and Add to cart.

6. Diwali Offers: dark burgundy rounded panel with a gold glow, three
   flickering diya lamps and twinkling sparkles. "Diwali Offers" heading and
   three offer cards (Festive Gift Box 25% off, Buy 2 Get 1 Free,
   Pistachio + Caramel pair): discount badge, fanned cups, description, a
   dashed code button that copies the code, and Add to cart which adds the
   whole bundle and opens the cart.

7. Reviews: "Loved By Our Regulars" with an average-rating badge. Two rows of
   review cards scrolling in opposite directions, pausing on hover. Each card:
   star rating, quote, initials avatar in the flavour colour, name, city and a
   flavour chip with the cup thumbnail.

8. Contact (footer): deep-burgundy rounded panel, "Got a craving?" heading,
   email button, Diwali offers link, social links, copyright row.

CART
- React Context store persisted to localStorage: add, set quantity, clear.
- Floating cart button (count badge + subtotal) appears once the cart has
  items. Clicking opens a slide-in drawer with line items, − / + controls,
  subtotal and Checkout. Checkout empties the cart, closes the drawer and shows
  an "Order placed!" toast.

QUALITY
- Fully responsive (phone to large desktop), no horizontal page scroll.
- Respect prefers-reduced-motion: no drifting marquees or conveyor, show the
  final video frame only.
- Fast: compressed WebP images, lazy-loaded frames, pause off-screen
  animations, AVIF/WebP via next/image.
- Accessible: alt text, aria labels on icon buttons, focus management in the
  cart drawer, Escape to close.
- Keep all content (flavours, prices, offers, reviews) in typed data files so
  it can be edited without touching components.
```
