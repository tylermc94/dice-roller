# D&D Dice Roller

A self-contained, browser-based dice roller for Dungeons & Dragons and other tabletop RPGs. Dice roll with 2D physics in a virtual tray, bouncing off walls and each other before settling. Features a lasso-based reroll system and a dedicated roll history panel with visual die shape icons.

## How to Use

Open `dice_roller.html` in any modern web browser (double-click the file or drag it into a browser window). No server, build tools, or dependencies required.

### Rolling Dice

1. Use the **+/-** buttons in the sidebar to select how many of each die type you want (d3, d4, d6, d8, d10, d12, d20, d100).
2. Click **Roll Dice** or press **Space** to throw them.
3. Dice will bounce around the tray and settle naturally. The total appears in the bar above the tray.

### Interacting with Dice

- **Drag** any settled die to reposition it in the tray.
- **Flick** a die (drag and release with velocity) to re-roll that individual die. The total updates automatically.

### Rerolling Dice

There are three ways to reroll dice after a roll has settled:

1. **Double-click** any single die to reroll just that die.
2. **Lasso select** multiple dice (click and drag on empty tray space to draw a selection box), then **double-click** any selected die to reroll all of them.
3. **Lasso select** multiple dice, then click the **Reroll Selected** button (or press **Space**).

Selected dice display a pulsing gold glow ring. The lasso tool is disabled while dice are actively rolling. Click on empty tray space to clear the selection. Selected dice can include mixed die types (e.g., a d6 and a d20 together).

When rerolling, unselected dice keep their exact values and positions. Only the selected dice get new random values and animate from their current position. Keep/Bonus settings are recalculated fresh after the reroll.

### Roll History

The right sidebar shows a scrollable history of all rolls and rerolls. Each entry displays:

- The roll total in large gold text
- A row of small die shape icons (monochrome outlines matching each die type) with the rolled value on top
- A text breakdown of all dice values

For **reroll entries**, the history shows:

- The old total struck through in red with an arrow to the new total (e.g., ~~23~~ → 25)
- A "REROLL" badge
- Rerolled dice show the old value struck through in red above the die icon, with the new value in green on the icon
- Non-rerolled dice display normally

Click **Clear** in the history header to clear all entries.

### Keep Highest / Lowest

Use the **Keep** dropdown to keep only the top N dice (great for rolling ability scores with 4d6 keep 3). Switch the **Mode** dropdown to **Lowest** for disadvantage rolls.

### Bonus Modifier

Enter a number in the **Bonus** field to add a flat modifier to the total (supports negative values for penalties).

### Max Value Flames

When a die rolls its maximum value (e.g., 20 on a d20), it gets a flame particle effect and glowing text.

## Customization

### Visual Settings

Expand the **Visual Settings** panel to change:

- **Tray Background** - 7 presets: Purple Felt, Green Felt, Red Felt, Blue Felt, Dark, Black, Wood
- **Dice Color Theme** - 7 themes: Classic, Fire, Ice, Neon, Pastel, Earth, Galaxy
- **Font** - 6 options for the numbers on dice faces: Cinzel, Medieval Sharp, Uncial Antiqua, Pirata One, Georgia, Monospace
- **Accent Color** - 8 colors for the tray border and UI highlights

### Physics Settings

Expand the **Physics Settings** panel to tune the simulation:

| Slider | What it controls |
|--------|-----------------|
| Time to Lock | Maximum seconds before dice are force-settled |
| Friction | How quickly dice slow down |
| Throw Speed | Initial velocity when rolled |
| Bounce | How much energy is preserved on wall/die collisions |
| Gravity Well | Strength of pull toward the tray center |
| Dice Size | Scale multiplier for all dice |
| Spin | How much dice rotate when thrown |

Click **Reset to Defaults** to restore all physics settings.

## Layout

The app uses a three-column layout:

- **Left sidebar** (290px) - Dice selection, Keep/Bonus settings, Visual Settings, Physics Settings, Roll/Clear buttons
- **Center canvas** - The rolling tray where dice animate and settle
- **Right sidebar** (290px) - Scrollable roll history with die shape icons and reroll tracking

Left sidebar panels (Dice Selection, Keep/Bonus, Visual Settings, Physics Settings) can be popped out into draggable, resizable floating windows on the canvas. Pop-out windows act as physics obstacles that dice bounce off of.

## Technical Details

- Single self-contained HTML file (~2000 lines) with embedded CSS and JavaScript
- HTML5 Canvas 2D rendering with `devicePixelRatio` support for sharp display on HiDPI screens
- Custom 2D physics: per-frame friction, wall bounce with restitution, circle-based die-to-die collision with impulse response, configurable gravity well toward tray center
- Dice settle via a speed threshold timer: once a die's speed stays below the threshold for 1 second, it locks in place
- Maximum roll time prevents dice from sliding indefinitely
- Each die has a unique ID for tracking across rerolls, enabling old-vs-new value comparison in the history panel
- Lasso selection uses a 5px movement threshold to distinguish a click (clear selection) from a drag (draw selection rectangle)
- History die icons are drawn on individual canvas elements using the same `SHAPE_DRAW` path functions as the tray dice, but in monochrome
- Touch support for mobile devices
- Fonts loaded from Google Fonts (requires internet on first load; cached by browser after)

## Die Shapes

Each die type is rendered with a distinct 2D shape approximating its real-world geometry:

| Die | Shape | Detail |
|-----|-------|--------|
| d3 | Triangle | - |
| d4 | Triangle | Inner inverted triangle (edge midpoints) |
| d6 | Square | - |
| d8 | Diamond (rotated square) | - |
| d10 | Pointed diamond (kite) | - |
| d12 | Pentagon | Inner rotated pentagon |
| d20 | Decagon (10-sided) | Inner pentagon with connecting facet lines |
| d100 | Circle | Displays values as 10, 20, ..., 90, 00 |
