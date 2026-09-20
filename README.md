PAGE BeatFood
  LOAD fonts: Fraunces (display), Public Sans (body), Space Mono (labels/prices)
  DEFINE design tokens
    colors: bg, surface, ink, brown, tan, muted, line, dark, footer, accent-orange
    IF viewer prefers dark: OVERRIDE bg, surface, ink, brown, button colors
    layout: content width max 1440, side padding 80 (40 tablet, 20 mobile)

  RENDER SiteHeader (sticky, 96px tall, orange tint, bottom border)
    LEFT:   Logo = 4 equalizer bars (alternating tan/brown) + "Beat" + italic "Food"
    CENTER: Nav links -> Home, Menu, About us, Contact Us (scroll to sections)
    RIGHT:  Reserve button -> #reserve, phone number
    IF width <= 1000: HIDE nav, SHOW burger button that opens/closes nav dropdown

  RENDER Hero (min-height 875)
    LEFT COLUMN (656px wide, stacked with 32px gaps)
      Eyebrow: short tan line + "Now Serving · Mon–Sun"
      Headline: "Flavor that hits" + italic "different."
      Lead paragraph
      Buttons: [View the Menu] solid, [Reserve a Table] outline
      FlavorConsole card
        header: "Flavor Console v1.0" + 3 colored LED dots
        4 faders: Spice 85, Char 60, Umami 95, Fresh 70
          each = vertical track + colored fill + name + percent
        divider
        footer line: rating, dish count, opening hours
    RIGHT SIDE
      Cork half-ellipse hugging right edge
      Dashed ring containing spinning vinyl disc
        disc = grooves + light sheen + tan center label + hole
      NowSpinningCard: red dot, "Chili Beat Bowl", "Track 01 · 128 BPM", progress bar, time
    IF width <= 1000: STACK left column, then vinyl, then card

  RENDER FeatureStrip (dark background, 3 columns, tan left border on each)
    (🔥 Fresh, Fired Daily) (🌶️ Bold by Default) (🌿 Locally Sourced)

  RENDER FeaturedSection
    header: "Featured" + "This Week's Beats"
    FOR EACH dish IN [Chili Beat Bowl, Mango Groove Chicken, Basil Bassline Pasta]
      card = photo slot (260px) + badge pill top-left
           + "TRACK n" + title row (name, price) + description

  RENDER dark divider bar

  RENDER MenuSection ("The Tracklist")
    two columns: Side A - Starters (01-04), Side B - Main Dishes (05-08)
    FOR EACH item
      row = number + name + short description + price
      row background alternates light / white

  RENDER dark divider bar

  RENDER GallerySection (id = about)
    header: "Inside the groove" + "From the Kitchen & Floor"
    row 1 (440px): one big photo + column of 2 stacked photos (300px wide)
    row 2 (320px): 3 equal photos
    Mission card + Vision card side by side

  RENDER ReviewsSection (dark)
    header: "Crowd Reaction" + "On Repeat With Diners"
    FOR EACH review: star rating, italic quote, author name

  RENDER ReserveSection (id = reserve)
    LEFT: "Secure Entry", headline, paragraph, clock icon + opening hours
    RIGHT: Ticket card (dashed border, notches on both sides)
      header: "Admit One Tab" + ticket number
      fields: Name, Party size (select), Date, Time slot (select),
              Phone, Birthday (optional), Email
      button: Confirm Reservation

  RENDER Footer (id = contact)
    4 columns: brand + social icons | Visit | Hours | Explore links
    divider
    bottom bar: copyright | school credit

BEHAVIOR

  ON burger click:
    TOGGLE nav open state, UPDATE aria-expanded
  ON nav link click:
    CLOSE nav

  FOR EACH fader track:
    FUNCTION setFader(track, percent)
      CLAMP percent to 0..100
      SET fill height, aria-valuenow, and the % label
    ON pointer down:
      CAPTURE pointer
      setFader(track, (track.bottom - pointer.y) / track.height * 100)
      WHILE pointer moves: repeat the same calculation
      ON pointer up: STOP tracking
    ON key ArrowUp/Right:  setFader(value + 5)    (Shift = 10)
    ON key ArrowDown/Left: setFader(value - 5)    (Shift = 10)

  ON page load:
    SET date input min = today

  ON reservation submit:
    PREVENT default
    IF form invalid:
      SHOW browser validation messages
      SET status = "Fill in the highlighted fields..."
      STOP
    READ name, party, date, time
    FORMAT date as "Month D, YYYY"
    MARK ticket as done (button turns green)
    SET button text = "Reservation Confirmed"
    SET status = "See you <date> at <time>, <name> - table for <party>. Demo only."

  IF user prefers reduced motion:
    STOP vinyl spin and smooth scrolling
