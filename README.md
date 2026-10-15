# Plasma Bars

A WebGL experiment. Vertical beams of light shine down from above the screen. Move your cursor through them and they ignite into plasma.

## How it behaves

Each beam starts at a soft glow at the top edge and widens and dims as it falls, with slow haze drifting through it. When the cursor comes within about 170px, the nearby beams brighten from within and pulse, and each pulse ripples outward along the beam from the cursor.

Moving through the beams leaves a trail of energy that rises slowly and fades over about a second. Inside the trail the light shifts from cool cyan and violet to magenta and orange, and thin plasma strands peel off the beams and twist around them. Faster swipes leave more energy. Click or tap for a larger burst.

Touch screens get one cursor per finger, up to eight. If nobody touches the page for four seconds, an invisible cursor sweeps through the beams until someone does.

## Running it

It is a single HTML file with no dependencies or build step. Open `index.html` in a browser that supports WebGL2.

## How it works

The page runs two shader passes each frame.

The first pass updates a quarter-resolution field that stores plasma energy and velocity for each point on screen. Each frame the field drifts with its own velocity, rises a little, blurs into its neighbors, and decays. Every cursor adds energy along the path it moved since the last frame, so fast movements leave a continuous trail instead of dots.

The second pass draws the beams. For each pixel it adds up light from the three nearest beams: a thin bright line at the center, a wider Gaussian body, and an exponential scatter into the dark around it. Each beam's light fades to zero before its slot edge so no seams show between beams. The energy field controls the color shift, the plasma strands, and the haze. Cursor distance controls the brightening and the pulse.

The field uses half-float textures where the browser can render to them. Otherwise it falls back to 8-bit textures with the values encoded into the 0 to 1 range.

## Tuning

These values live in `index.html`.

| Value | What it controls |
| --- | --- |
| `W / 72` | Beam spacing in CSS pixels |
| `wb = (13.0 + ...)` | Beam width |
| `spread`, `falloff` | How much beams widen and dim toward the bottom |
| `170.0` | How close the cursor must be to brighten a beam |
| `t * 6.5` | Pulse speed |
| `0.985` | How fast the plasma trail fades |
| `40.0 + 70.0 * ee` | How far plasma strands reach from a beam |
