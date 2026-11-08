# Pseudo Code

Plain-language pseudo code for each part of the site, in page order.

---

## 1. Page

```
PAGE:
  wrap everything in SmoothScroll (one Lenis instance)
  show Preloader on top
  inside CartProvider:
    render Hero
    render FavoriteFlavors
    render OurStory
    render BestSellers
    render DiwaliOffers
    render Reviews
    render FlyObject          // floating stand-in for the tub-to-cup flight
    render Footer (Contact)
    render CartDrawer         // floating cart button + slide-in panel
```

## 2. Preloader (intro, ~1.6s)

```
ON MOUNT:
  stop smooth scroll, scroll to top, lock page overflow
  draw a full-screen burgundy sheet with the logo CUT OUT of it (SVG mask),
    so the hero shows through the letters

  IF user prefers reduced motion:
    fade the sheet out after 0.4s
  ELSE:
    animate logo scale 0.08 → 0.65 over 0.65s (slight bounce)
    at 0.8s: zoom logo scale → 34 over 0.9s   // flies through the letters
    at 1.35s: fade the sheet out over 0.3s

ON COMPLETE:
  remove the preloader, unlock scrolling
```

## 3. Hero

```
STATE activeId = first product

BACKGROUND:
  colour = activeProduct.bg      // soft tint of that tub's container colour
  transition colour over 0.7s
  fade bottom edge into page pink

ON MOUNT:
  fade background in
  slide headline lines up one after another
  parallax headline upward while scrolling past the hero

SHOW: navbar, headline, tagline + Shop Now (from 0.9s), flavour swatches

WHEN a swatch is clicked:
  activeId = that product   // conveyor will run until it reaches the front
```

## 4. Product conveyor (ProductOrbit)

```
SLOTS = 5 visible stops, top-back → bottom-front, each larger and in front
EACH flavour has 2 copies (one can leave the bottom while its twin enters the top)

ON MOUNT:
  place copies on stops / off-screen above
  fade + scale visible tubs in, staggered
  after ENTRANCE + HOLD: start the loop

LOOP advance():
  IF page is scrolled down AND not jumping to a clicked flavour:
    wait 0.3s and check again      // front tub is busy flying to its cup
  ELSE:
    move every tub one stop forward (park the one that left, bring a twin in)
    activeId = flavour now at the front stop
    wait MOVE, then HOLD (or 0 if still jumping), then advance() again

WHEN activeId changes from outside (swatch click):
  set jumpTarget, advance immediately with faster moves until it's at the front
```

## 5. Tub-to-cup flight (FlyObject)

```
ON every scroll frame:
  tub = element at the front conveyor stop
  cup = copy of the matching flavour cup nearest the screen centre
  IF no tub/cup OR scrollY == 0:
    hide stand-in, show real tub and cup, flight.flavor = null
    RETURN

  end      = scroll position where the cup is centred on screen
  progress = clamp(scrollY / end, 0, 1)

  IF progress >= 1:
    landed → hide stand-in, real cup visible, RETURN

  flight.flavor = this flavour      // marquee glides that cup to centre
  t = easeInOut(progress)
  stand-in position/size = lerp(tub box, cup box, t)
  cross-fade tub image → cup image between 30% and 70% progress
  hide real tub and real cup while the stand-in is in the air
```

## 6. Flavors marquee

```
TRACK = all cups twice in a row
EACH FRAME (only while on screen):
  IF a tub is flying to flavour X:
    ease the track so X's nearest cup sits at screen centre
  ELSE IF not dragging:
    offset += velocity * dt
    velocity eases back toward -SPEED (steady leftward drift)
  wrap offset within one set width (seamless loop)

ON pointer drag: move with the pointer, remember flick velocity
ON sideways wheel/trackpad: move offset, stop momentum
EACH CUP: spinning swirl behind it, name, price, AddToCart button
```

## 7. Cart

```
CartProvider:
  items = { flavourId: quantity }, restored from localStorage after mount
  ON items change: save to localStorage (ignore if storage is blocked)
  add(id)          → quantity + 1
  setQuantity(id,n)→ n <= 0 removes the line, max 99
  clear()          → empty cart
  lines, count, subtotal derived from items × flavour prices

CartDrawer:
  IF count > 0 AND drawer closed: show floating button (count badge + subtotal)
  WHEN open: lock scroll, focus close button, Escape closes
  list lines with − / + controls and line totals
  Checkout button:
    clear cart, close drawer, show "Order placed!" toast for 3s
    // TODO: send the order to a real checkout first
```

## 8. Our Story

```
LEFT COLUMN (sticky on desktop):
  heading "Made To Swirl"
  paragraph where each word fades 15% → 100% as it scrolls into place
  3 value cards fade up

RIGHT COLUMN = tall track (225svh):
  sticky full-screen box holding the canvas + spinning badge
  progress = scroll through the track, from track top hitting screen top
             to track bottom hitting screen bottom
  frameIndex = round(progress × (FRAME_COUNT − 1))
  draw frames[frameIndex] on the canvas
    (fall back to the nearest earlier loaded frame so it never blanks)

FRAME LOADING:
  wait until the canvas is within ~2 screens (IntersectionObserver)
  load frame 0, the last frame, then the rest in order
  reduced motion → load and show only the last frame
```

## 9. Best Sellers

```
ranked = bestSellers mapped to their flavour data
FOR EACH (rank i):
  card: "#i" outline number, tag badge, cup on a flavour-coloured circle,
        name, "N cups sold", AddToCart
  fade up, staggered by i
```

## 10. Diwali Offers

```
dark panel with warm gold glow, 3 flickering diyas, twinkling sparkles
FOR EACH offer:
  discount badge, fanned cups, title, description
  code button  → copy code to clipboard, show "Copied!" for 1.6s
  Add to cart  → add one of each pictured flavour, open the cart drawer
```

## 11. Reviews

```
summary badge: average rating + count
split reviews into two rows
EACH ROW: list doubled, slides −50% forever (second row reversed, slower)
          pauses on hover
EACH CARD: stars, quote, initials avatar in the flavour colour
           (dark or light text chosen by brightness), name, city,
           flavour chip with cup thumbnail
```

## 12. Footer / Contact

```
"Got a craving?" heading, short note
email button (mailto), "Diwali offers" link
Follow list with social icons
copyright row
```

## 13. Video → transparent frames (build-time script)

```
extract frames from the video at 30fps, starting at 3s, crop around the cup
FOR EACH frame:
  FOR EACH pixel:
    excess = green − max(red, blue)
    alpha  = 1 − (excess − 12) / (34 − 12), clamped 0..1   // green → transparent
    despill: cap green near max(red, blue)
    feather: fade alpha within 48px of the frame border
  save as WebP with alpha
```
