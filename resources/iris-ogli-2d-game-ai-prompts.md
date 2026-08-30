============================================================
FULL PROMPTS — Iris Ogli: Create an Entire 2D Game with AI
Source: https://irisogli.com/create-an-entire-2d-game-with-ai-characters-assets-animation-and-parallax-free
============================================================


TOOLS USED:
- ChatGPT (DALL-E)
- Photopea (free browser Photoshop)
- Blender (green screen removal + frame export)
- Tencent AI Studio (free AI animation)
- Microsoft Photos (asset cropping)


============================================================
PROMPT 1 — MAIN CHARACTER (ChatGPT / DALL-E)
============================================================

Stylized cartoon boy character, full body, messy spiky black hair, large expressive eyes looking upward, soft innocent facial expression, wearing oversized mustard yellow hoodie, black ripped skinny jeans, brown boots, small backpack, standing straight, next to a small fluffy white creature with rounded body, tiny legs, black oval eyes and nose, minimal facial features, cute companion design. Clean green background, soft lighting, painterly texture, hand-drawn illustration style, slightly rough sketch lines, subtle shading, whimsical children's book aesthetic, high detail, centered composition.


============================================================
PROMPT 2 — ENVIRONMENT ASSET PACK (ChatGPT / DALL-E)
============================================================

Create a complete, cohesive 2D side scrolling game enviroment asset pack in a whimsical hand drawn storybook illustration style matching a stylized cartoon boy with messy spiky black hair and a small fluffy white creature companion.

include a full modular set of environment assets: ground tiles (grass, dirt, stone), seamless tileable textures, grass variations, dirt paths, rocks, bushes, flowers, trees (small, medium, large), tree stumps, wooden fences, signposts, logs, mushrooms, small props, decorative elements design everything in a soft painterly style with visible brush texture, slightly rough sketchy ink linework, subtle shading, warm friendly color palette, playful proportions, simple but charming shapes mood: magical, calm, cozy, childlike adventure world ensure strict visual consistency across all assets: same lighting direction, same color harmony, same line thickness, same texture style layout: assets clearly separated, evenly spaced, no overlap, centered and organized like a sprite sheet background: clean flat green background perspective: side view, 2D platformer style technical requirements: tileable seamless textures, modular design, grid-aligned assets, consistent scale between objects, game-ready high resolution, sharp edges,  Make the aspect ratio 16:9.


============================================================
PROMPT 3 — PARALLAX BACKGROUND LAYERS (ChatGPT / DALL-E)
============================================================

Generate separate depth layers for a 2D side scrolling game environment. Each layer must be created as an individual image file.
The purpose is to create a consistent parallax scrolling background system.
Make the aspect ratio 16:9.
Generate The Following Depth Layers

1. Far Background Layer (Slowest Movement)
Only distant environmental elements.
Low contrast and reduced detail.
No gameplay platforms.
No foreground objects.
No characters.
Leave space in lower areas for stacking additional layers.

2 Mid Background Layer
Environmental structures with medium detail and contrast.
Still no gameplay platforms.
No characters.
Designed to sit in front of the far background but behind gameplay elements.

3. Gameplay Layer
Playable ground, platforms, stairs, props, or environmental objects relevant to gameplay.
Clean separation from background.
No far background elements included.
Optimized for collision and interaction.

4. Foreground Layer (Fastest Movement)
High-contrast silhouette or decorative foreground elements.
Positioned at the bottom or edges of the frame.
No background elements.
No character.
Designed to enhance depth when moving faster than other layers.
No text.


============================================================
PROMPT 4 — CHARACTER FOR ANIMATION (ChatGPT / DALL-E)
============================================================

Stylized cartoon adventure boy and his loyal fluffy white dog companion walking to the right in a 2D side-scrolling platformer game. Full side view profile facing right. Boy has messy spiky black hair, oversized mustard yellow hoodie, black ripped skinny jeans, brown boots, and a small backpack. Dog is small, fluffy, white, round-bodied, cute and expressive. Both characters are mid-walk cycle with natural stepping poses and slight motion feeling. Clean silhouette, production-ready game art, whimsical hand-drawn storybook style, painterly textures, soft shading, expressive design, crisp outlines, high readability for animation and sprite extraction.

IMPORTANT:
solid chroma key green background only (#00FF00 style green screen),
no environment, no shadows outside characters, no props, no text, no UI, no extra objects.
Centered composition, side-scrolling platformer perspective, animation-ready pose, consistent proportions, high detail, clean edges, studio quality 2D game asset.


============================================================
PROMPT 5 — WALK ANIMATION (Tencent AI Studio / HY Video 1.5)
============================================================

Full body character walk cycle, fixed side profile view, walking to the right. A young boy with messy black hair wearing a yellow hoodie, backpack, and brown bootstatic camera, no camera movement, locked shot, 3D animated film style, Pixar aesthetic, clean solid background, consistent movement, high detail.


============================================================
PROMPT 6 — IDLE ANIMATION (Tencent AI Studio / HY Video 1.5)
============================================================

Full body character Idle stand, fixed side profile view, standing, waiting to the right. A young boy with messy black hair wearing a yellow hoodie, backpack, and brown bootstatic camera, no camera movement, locked shot, 3D animated film style, Pixar aesthetic, clean solid background, consistent movement, high detail.


============================================================
WORKFLOW STEPS (from video)
============================================================

1. ChatGPT: Generate character (Prompt 1)
2. ChatGPT: Generate environment asset pack (Prompt 2)
3. ChatGPT: Generate parallax backgrounds (Prompt 3)
4. Microsoft Photos: Crop/isolate individual assets
5. ChatGPT: Upscale each asset (just type "UPSCALE")
6. Photopea: Remove backgrounds → transparent PNGs
   - Select → Color Range → adjust fuzziness → delete
7. Photopea: Slice multiple assets automatically
   - Slice Tool → Right-click → Divide
   - Export as PNG ZIP
8. Photopea: Create seamless tileable textures
   - Filter → Other → Offset → spot healing on seam
9. ChatGPT: Generate character on green screen (Prompt 4)
10. Tencent AI Studio: Animate walk + idle (Prompts 5-6)
11. Blender: Remove green screen + export transparent frames
    - Video Editing → Add Movie
    - Compositor → Keying node → eyedropper on green
    - Output: PNG → RGBA → Render Animation
