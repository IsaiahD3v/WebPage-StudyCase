<script lang="ts">
  import { fade, fly } from 'svelte/transition';
  import { spring } from 'svelte/motion';
  import { onMount } from 'svelte';

  // --- Files / import images ---
  import imgDataScience from '$lib/assets/pictures_/datascience.jpeg';
  import imgDoctor from '$lib/assets/pictures_/Doctor.jpg';
  import imgFarm from '$lib/assets/pictures_/Farm.jpg';
  import imgIot from '$lib/assets/pictures_/IOT.jpg';
  import imgServers from '$lib/assets/pictures_/servers.webp';
  import imgSupermarket from '$lib/assets/pictures_/supermarkety.jpg';
  import imgTeacher from '$lib/assets/pictures_/teacher.webp';
  import imgLogo from '$lib/assets/pictures_/LoGo.png';
  import bgVideo from '$lib/assets/pictures_/eye.mp4';

  const heroSliderImages = [
    imgDataScience, imgDoctor,  
    imgIot, imgServers, imgSupermarket, imgTeacher
  ];

  const testimonials = [
    { name: "Sarah J.", role: "Professor Ph.D", text: "Praxium revolutionized our classroom connectivity.", img: imgTeacher },
    { name: "Dr. Aris", role: "Hospital Director", text: "The IOT integration was seamless and life-saving.", img: imgDoctor },
    { name: "Mark T.", role: "Agro Engeneer", text: "Scaling production with Praxium meant an year of time saved.", img: imgFarm }
  ];

  //  HTML element types - clearing errors from terminal (not really necessary )
  let sectionHero: HTMLElement | undefined = $state();
  let sectionWhatWeDo: HTMLElement | undefined = $state();
  let sectionGetInTouch: HTMLElement | undefined = $state();
  
  // SVELTE 5 RUNES animation and form
  let isSubmitted = $state(false); 

  // SVELTE 5 RUNES video sync
  let videoTime = $state(0);
  let showSec2Content = $derived(videoTime >= 1); // true after a sec

  function handleSubmit(e: Event) {
    e.preventDefault();
    isSubmitted = true;
  }

  // scrolling and section
  function scrollTo(element: HTMLElement | undefined) {
    if (element) element.scrollIntoView({ behavior: 'smooth' });
  }

  // SVELTE 5 RUNES relative variables
  let currentSlide = $state(0);
  onMount(() => {
    const sliderInterval = setInterval(() => {
      currentSlide = (currentSlide + 1) % heroSliderImages.length;
    }, 4000); 
    return () => clearInterval(sliderInterval);
  });

  let time = $state(0);
  //allow null assignments in the form
  let hoveredIndex = $state<number | null>(null);
  
  onMount(() => {
    let frame: number; // Typed the frame variable
    const loop = () => {
      time += 0.01; 
      frame = requestAnimationFrame(loop);
    };
    loop();
    return () => cancelAnimationFrame(frame);
  });

  // MOUSE  GRAPHICS 
  let parallaxCoords = spring({ x: 0, y: 0 }, { stiffness: 0.03, damping: 0.1 });
  let glowCoords = spring({ x: 0, y: 0 }, { stiffness: 0.1, damping: 0.3 });
  
  let innerWidth = $state(0);
  let innerHeight = $state(0);

  //Parameters fo mouse graphics; MOUSEEVENT
  function handleMouseMove(e: MouseEvent) {
    const px = (e.clientX / innerWidth) * 2 - 1;
    const py = (e.clientY / innerHeight) * 2 - 1;
    parallaxCoords.set({ x: px, y: py });
    glowCoords.set({ x: e.clientX, y: e.clientY });
  }
</script>

<svelte:window onmousemove={handleMouseMove} bind:innerWidth bind:innerHeight />

<div 
  class="pointer-events-none fixed top-0 left-0 z-[60] w-[500px] h-[500px] rounded-full opacity-80 mix-blend-screen transition-opacity duration-500"
  style="
    transform: translate({$glowCoords.x - 250}px, {$glowCoords.y - 250}px);
    background: radial-gradient(circle, rgba(59, 130, 246, 0.4) 0%, rgba(59, 130, 246, 0.1) 30%, transparent 60%);
  "
></div>

<nav class="fixed top-0 left-0 w-full flex justify-between items-center px-8 lg:px-16 z-[100] h-24 nav-vertical-mask font-modern">
  <div 
    class="w-20 h-20 cursor-pointer transition-all hover:scale-105 flex-shrink-0" 
    role="button" 
    tabindex="0" 
    onclick={() => scrollTo(sectionHero)}
    onkeydown={(e) => (e.key === 'Enter' || e.key === ' ') && scrollTo(sectionHero)}
  >
    <img src={imgLogo} alt="Logo" class="w-full h-full object-contain" />
  </div>
  <ul class="hidden md:flex space-x-12 font-medium tracking-[0.2em] text-white/70">
    <li><button class="hover:text-white cursor-pointer transition-colors uppercase text-[10px]" onclick={() => scrollTo(sectionWhatWeDo)}>What We Do</button></li>
    <li><button class="hover:text-white cursor-pointer transition-colors uppercase text-[10px]" onclick={() => scrollTo(sectionGetInTouch)}>Get In Touch</button></li>
  </ul>
</nav>

<div class="pointer-events-none fixed inset-0 z-50 opacity-[0.03] mix-blend-overlay" style="background-image: url('https://grainy-gradients.vercel.app/noise.svg');"></div>

<div class="snap-y snap-mandatory h-screen overflow-y-scroll relative z-10 scroll-smooth bg-[#06114F] font-modern">
  
  <section bind:this={sectionHero} class="snap-start min-h-screen text-white flex flex-col items-center justify-center relative overflow-hidden">
    <div class="absolute inset-0 z-0">
      {#each heroSliderImages as img, i}
        <div class="absolute inset-0 transition-opacity duration-1000 ease-in-out" style="opacity: {currentSlide === i ? '0.8' : '0'};">
          <div class="absolute inset-0 bg-gradient-to-b from-black/70 via-transparent to-[#081663] z-10"></div>
          <img src={img} alt="Slider" class="w-full h-full object-cover" />
        </div>
      {/each}
    </div>

    <div 
      class="absolute w-[600px] h-[400px] bg-blue-500/20 rounded-full blur-[100px] z-10 pointer-events-none"
      style="transform: translate({$parallaxCoords.x * 40}px, {$parallaxCoords.y * 40}px)"
    ></div>

    <main 
      class="relative z-20 flex flex-col items-center text-center px-4"
      style="transform: translate({$parallaxCoords.x * 20}px, {$parallaxCoords.y * 20}px)"
    >
      <div in:fly={{ y: 30, duration: 1000 }}>
        <h1 class="text-7xl lg:text-[9rem] font-bold tracking-[0.05em] mb-4 italic leading-none uppercase drop-shadow-2xl">
          PRAXIUM
        </h1>
        <h2 class="text-lg lg:text-2xl font-light tracking-[0.6em] text-white-300 uppercase drop-shadow-md">
          Solutions that Bridge Expectations
        </h2>
      </div>
    </main>

    <button 
      onclick={() => scrollTo(sectionWhatWeDo)}
      aria-label="Scroll to What We Do Section"
      class="absolute bottom-10 left-1/2 -translate-x-1/2 z-30 opacity-40 hover:opacity-100 transition-opacity"
    >
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="animate-bounce">
        <path d="M7 10l5 5 5-5" />
      </svg>
    </button>
  </section>

  <section bind:this={sectionWhatWeDo} class="snap-start min-h-screen bg-[#081663] text-white relative overflow-hidden flex flex-col justify-between items-center pt-24 pb-12 px-6">
    
    <video 
      src={bgVideo} 
      autoplay 
      muted 
      playsinline
      bind:currentTime={videoTime}
      class="absolute inset-0 w-full h-full object-cover opacity-50 z-0 pointer-events-none"
    ></video>

    <div class="absolute inset-0 opacity-[0.15] pointer-events-none bg-mesh-pattern z-0"></div>
    <div class="absolute inset-0 opacity-[0.05] pointer-events-none bg-noise-grain z-0"></div>

    <div class="z-10 text-center max-w-5xl relative transition-opacity duration-500 ease-in-out {showSec2Content ? 'opacity-100' : 'opacity-0'}">
      <h2 class="text-6xl lg:text-8xl font-black italic tracking-tighter uppercase mb-8 leading-none">VISIONS ACHIEVED</h2>
      <p class="text-blue-100 tracking-[0.25em] uppercase text-sm md:text-lg font-medium leading-relaxed max-w-4xl mx-auto px-4 drop-shadow-sm">
        We develop custom software and hardware solutions for wide applications so you can achieve your best. Doctors, teachers, supermarkets, farmers, government, artists, and much more—we strive for diverse visions and the solutions we build for them.
      </p>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-8 max-w-6xl w-full z-10 mb-4 mt-12 relative transition-opacity duration-500 ease-in-out {showSec2Content ? 'opacity-100' : 'opacity-0'}">
      {#each testimonials as item, i}
        <div 
          role="presentation"
          class="bg-white/[0.04] backdrop-blur-3xl p-10 rounded-[3rem] border border-white/10 flex flex-col items-center text-center shadow-2xl transition-all duration-700 ease-out"
          onmouseenter={() => hoveredIndex = i}
          onmouseleave={() => hoveredIndex = null}
          style="
            transform: translateY({hoveredIndex === i ? -20 : Math.sin(time + (i * 2)) * 30}px); 
            opacity: {hoveredIndex !== null && hoveredIndex !== i ? 0.3 : 1};
          "
        >
          <div class="w-20 h-20 rounded-full overflow-hidden mb-6 border-2 border-blue-400/50">
            <img src={item.img} alt={item.name} class="w-full h-full object-cover" />
          </div>
          <p class="text-xl leading-relaxed mb-8 font-light text-white italic">"{item.text}"</p>
          <div>
            <div class="font-bold text-lg tracking-widest uppercase text-white">{item.name}</div>
            <div class="text-[12px] uppercase tracking-[0.3em] text-blue-300 mt-2 font-bold">{item.role}</div>
          </div>
        </div>
      {/each}
    </div>
  </section>

  <section bind:this={sectionGetInTouch} class="snap-start min-h-screen bg-[#040a2e] text-white flex flex-col lg:flex-row items-center p-6 lg:p-24 gap-16 relative overflow-hidden">
    <div class="absolute inset-0 opacity-[0.1] pointer-events-none bg-mesh-pattern"></div>
    <div class="absolute inset-0 opacity-[0.05] pointer-events-none bg-noise-grain"></div>

    <div class="w-full lg:w-1/2 z-10 pb-20 lg:pb-0">
      <h2 class="text-6xl lg:text-8xl font-bold mb-10 leading-[0.9] tracking-tighter">Elevate your <br/><span class="text-blue-500 italic">potential.</span></h2>
      <p class="text-blue-50 max-w-md mb-10 text-lg md:text-xl uppercase tracking-[0.15em] font-light leading-snug border-l-2 border-blue-500 pl-8">
        From digital consulting, Specialized software, or hardware, from farming to enterprise, our consultors are ready to hear your needs, so your vision can flourish.
      </p>
    </div>

    <div class="w-full lg:w-1/2 relative z-10 grid items-center justify-items-center min-h-[550px] pb-20 lg:pb-0">
      
      {#if !isSubmitted}
        <div 
          class="col-start-1 row-start-1 w-full bg-white/[0.02] backdrop-blur-3xl border border-white/10 rounded-[4rem] p-12 lg:p-16"
          out:fade={{ duration: 400 }}
        >
          <form 
            class="w-full space-y-10" 
            onsubmit={handleSubmit} 
          >
            <div class="space-y-6">
              <div class="group">
                <input type="text" placeholder="NAME" class="w-full bg-transparent border-b border-white/20 py-4 focus:border-blue-500 transition-all outline-none text-[12px] tracking-[0.4em] uppercase font-bold placeholder:text-white/40 text-white" />
              </div>
              <div class="group">
                <input type="email" placeholder="EMAIL" class="w-full bg-transparent border-b border-white/20 py-4 focus:border-blue-500 transition-all outline-none text-[12px] tracking-[0.4em] uppercase font-bold placeholder:text-white/40 text-white" />
              </div>
              <div class="group">
                <textarea rows="3" placeholder="YOUR VISION" class="w-full bg-transparent border-b border-white/20 py-4 focus:border-blue-500 transition-all outline-none text-[12px] tracking-[0.4em] uppercase font-bold placeholder:text-white/40 text-white resize-none"></textarea>
              </div>
            </div>
            <button type="submit" class="w-full bg-white text-[#040a2e] font-black py-6 rounded-full hover:bg-blue-600 hover:text-white transition-all transform hover:scale-[1.02] active:scale-95 uppercase tracking-[0.5em] text-[11px] shadow-lg">
              Get in touch
            </button>
          </form>
        </div>
      {/if}

      {#if isSubmitted}
        <div 
          class="col-start-1 row-start-1 w-full lg:w-[75%] bg-white shadow-[0_0_60px_rgba(59,130,246,0.3)] rounded-[5rem] p-10 flex flex-col items-center text-center"
          in:fly={{ y: 30, duration: 600, delay: 450 }} 
        >
          <div class="w-20 h-20 bg-blue-500 rounded-full flex items-center justify-center mb-8 shadow-xl">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="20 6 9 17 4 12"></polyline>
            </svg>
          </div>
          
          <h3 class="text-[#040a2e] text-2xl font-black uppercase tracking-tighter mb-4">Message Received</h3>
          <p class="text-[#040a2e]/70 font-bold uppercase tracking-[0.2em] text-[11px] leading-relaxed max-w-xs">
            A consultor will get in touch with you soon to discuss your vision.
          </p>
          
          <button 
            onclick={() => isSubmitted = false} 
            class="mt-8 text-[10px] uppercase tracking-[0.3em] font-black text-blue-600 hover:underline"
          >
            Reset Form
          </button>
        </div>
      {/if}
    </div>

    <footer class="absolute bottom-0 left-0 w-full p-6 lg:px-16 flex flex-col md:flex-row justify-between items-center z-20 text-[10px] uppercase tracking-[0.3em] font-bold text-white/50 gap-4">
      <div class="flex space-x-8 md:w-1/3 justify-center md:justify-start">
       <a href="https://github.com/IsaiahD3v" target="_blank" rel="noopener noreferrer" class="hover:text-white transition-colors">Careers</a>
       <a href="https://www.ifms.edu.br/" target="_blank" rel="noopener noreferrer" class="hover:text-white transition-colors">Education</a>
      </div>
      <div class="md:w-1/3 text-center text-white/30">
        Praxium 2026 ™
      </div>
      <div class="hidden md:block md:w-1/3"></div> 
    </footer>
  </section>
</div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@100;300;400;500;700;900&display=swap');

  :global(body) {
    background-color: #06114F;
    margin: 0;
    overflow: hidden;
  }

  .font-modern {
    font-family: 'Outfit', sans-serif;
  }
/* making scroll bar invisible*/
  .snap-y::-webkit-scrollbar { display: none; }
  .snap-y { -ms-overflow-style: none; scrollbar-width: none; }

  .nav-vertical-mask {
    background: #020617;
    -webkit-mask-image: linear-gradient(to bottom, black 0%, black 50%, transparent 100%);
    mask-image: linear-gradient(to bottom, black 0%, black 50%, transparent 100%);
  }

  /* TEXTURES overlays the background color */
  .bg-noise-grain {
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
  }

  /* the mash isnt even visible which was a great desapointment*/
  .bg-mesh-pattern {
    background-size: 40px 40px;
    background-image: radial-gradient(circle, rgba(255,255,255,0.05) 1px, transparent 3px);
  }

  section { overflow: hidden; }
</style>