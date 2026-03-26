<<<<<<< Page-home001
# Praxium study case Website

A fast, lightweigh  single-page website built for IoT  software and hardware solutions bussiness.

Tech Stack & Core Technologies

Framework: Svelte 5 (Utilizing Runes for granular state and DOM tracking)
Styling Engine: Tailwind CSS v3/v4 (Utility-first styling, arbitrary value injection, and complex backdrop filters)
Animation & Physics: Svelte native transitions (svelte/transition), spring-physics motion stores (svelte/motion), and native Web APIs (requestAnimationFrame)
Asset Optimization: Vite bundling for static media and localized font serving (@fontsourceor Google Fonts ‘Outfit’)

## Local Development & Deployment

To run this project in a Codespaces or local Node.js environment, follow these steps:

- Open your terminal.
- Install the dependency tree using Bash:

    npm install

- Initialize the Vite development server with Hot Module Replacement (HMR) using Bash:

    npm run dev

- Click the local or forwarded port link (typically http://localhost:5173) to view the live build.

## Scroll Architecture & Navigation

The application’s fundamental layout rejects traditional body scrolling in favor of a strictly controlled, full-viewport snapping mechanism.

The Container

HTML

<div class=“snap-y snap-mandatory h-screen overflow-y-scroll scroll-smooth relative z-10…”>

- snap-y snap-mandatory: Forces the browser’s scroll engine to strictly lock onto the nearest defined snap point along the Y-axis. The user cannot rest halfway between sections.
Scrollbar Hiding: Custom CSS (.snap-y::-webkit-scrollbar { display: none; }and -ms-overflow-style: none) completely conceals the visual scrollbar, creating the illusion of a presentation-style deck instead of a webpage.

Programmatic Navigation
TypeScript

function scrollTo(element: HTMLElement | undefined) {
  if (element) element.scrollIntoView({ behavior: ‘smooth’ });
}

The $state() runes sectionHero, sectionWhatWeDo, and sectionGetInTouch are directly linked to the <section> nodes. When a user clicks a nav link or the bouncing down-arrow, the scrollTo function bypasses the user’s mouse wheel and utilizes the native Web API scrollIntoView with a smooth easing behavior to precisely reach the top edge of the target section.

## Layering, Transparency & Glassmorphism

The visual identity of Praxium heavily relies on precise opacity (alpha channel) control and CSS blending modes to create deep, volumetric backgrounds.

The Global Glow Orb

A floating cursor orb serves as a dynamic lighting source:

Opacity & Blending: It operates at an opacity of 80% (80% visibility) but employs the mix-blend-screen blend mode. This blend mode causes the orb to lighten the pixels behind it, mimicking a literal flashlight rather than a painted circle.

Gradients: It utilizes a radial gradient originating at rgba(59, 130, 246, 0.4) (40% opaque blue) at the center, gradually fading to 10% opacity at 30% of its radius, and achieving absolute transparency at 60%.

Testimonial Glassmorphism Cards

HTML

<div class=“bg-white/[0.04] backdrop-blur-3xl border border-white/10…”>
The “What We Do” section employs premium frosted-glass techniques.

The background is filled with a microscopic 4% white color using the bg-white/[0.04] selector.

The backdrop-blur-3xl filter applies a heavy Gaussian blur to the background video and meshes behind the card.

A delicate border-white/10 (10% opacity) acts as a rim-light, defining the card’s physical edge.

Interactive fading is achieved by dynamically shifting the opacity of non-targeted cards to 0.3 (30%) when a user hovers over a specific card (onmouseenter={() => hoveredIndex = i}). This immediate focal attention draws the user’s attention to the hovered item.

## Animation & Physics Engine

The site departs from linear CSS transitions and instead utilizes spring physics and continuous mathematical animation loops.

The Parallax Physics (Svelte spring) is implemented using TypeScript.

Let parallaxCoords = spring({ x: 0, y: 0 }, { stiffness: 0.03, damping: 0.1 });
let glowCoords = spring({ x: 0, y: 0 }, { stiffness: 0.1, damping: 0.3 });

Svelte’s spring store interpolates values over time to simulate physical weight.

The Glow Orb (glowCoords) uses a stiffness of 0.1 and damping of 0.3, closely and responsively trailing the user’s actual cursor (e.clientX, e.clientY) with a slight, fluid delay.

The Typography Parallax (parallaxCoords) tracks normalized coordinates (-1 to 1 across the screen). It employs an extremely low stiffness (0.03) and damping (0.1), resulting in a very loose, heavy, and slow-moving 3D shift for the “PRAXIUM” header (transform: translate({$parallaxCoords.x * 20}px…)).
The Continuous Animation Loop (requestAnimationFrame)

The testimonial cards don’t use CSS keyframes. Instead, they employ a mathematically driven hardware-accelerated loop:

TypeScript

const loop = () => {
  time += 0.01;
  frame = requestAnimationFrame(loop);
};

Every frame (approximately 60 times a second), time increments by 0.01.

The Math: transform: translateY({Math.sin(time + (i * 2)) * 30}px)

This applies a Sine wave (Math.sin) to the Y-axis. The * 30 creates an amplitude of 30 pixels (moving up 30px, then down 30px). The + (i * 2) offset uses the array index i to ensure the cards are out of phase with each other—meaning they bob up and down asynchronously, like buoys in the water.

Svelte Directives (in:fly, out:fade)

The form submission logic utilizes precise entry and exit timing:

out:fade={{ duration: 400 }}: The contact form takes exactly 400ms to linearly fade out of the DOM.

in:fly={{ y: 30, duration: 600, delay: 450 }}: The “Message Received” component waits 450ms (giving the form time to clear), then animates upwards from 30px below its final resting place over 600ms.

## Texture Generation (Tiles & Noise)

To prevent the deep blue backgrounds (#06114F, #081663) from appearing flat, the project generates intricate overlays using pure CSS and base64 SVGs.
The Global Grain (.bg-noise-grain) is a fixed element across the entire app, set to an opacity of [0.03] and blended with overlay. It utilizes an inline data-URI SVG containing <feTurbulence type=‘fractalNoise’ baseFrequency=‘0.65’ numOctaves=‘3’ />. This generates randomized, high-frequency static that imparts a tactile, cinematic “film stock” feel to the viewport.

The Dot Mesh (.bg-mesh-pattern) is prominently featured in sections 2 and 3, with an opacity of [0.15]. It employs a background-size of 40px 40px to create a grid matrix. Within this grid, a radial-gradient draws a precise 1-pixel dot at 5% white opacity (rgba(255,255,255,0.05) 1px), which instantly fades to transparency at 3px. This meticulously spaced dot-grid evokes an engineering or blueprint aesthetic.
=======
# Engenharia-de-softwaress
#Software engeneering class
# Praxium study case Website (software devellopment process from resource needed to testing (no intent on deployment))

A fast, lightweight single-page website built for digital and consulting and software solutions bussiness.
>>>>>>> main

The CSS Mask (.nav-vertical-mask) is used for the top navigation bar. Instead of a solid background, it employs a webkit mask: mask-image: linear-gradient(to bottom, black 0%, black 50%, transparent 100%); This instructs the browser to render the navigation background at 100% opacity at the top edge, maintain it to the 50% mark, and then mathematically fade the actual element into transparency at the bottom edge, resulting in a seamless bleed into the content scrolling beneath it.

## Sections
1. Home
2. Testimonials
3. Get in Touch

## How to Run in Codespaces
1. Open the Codespace terminal.
2. Install dependencies: `npm install`
3. Start the server: `npm run dev`
4. Click the port forwarding link to view the live site.*//
