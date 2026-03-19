<script>
  import { fade, fly } from 'svelte/transition';

  // --- EDIT YOUR CONTENT HERE ---
  const logoText = "LO\nGO";
  const navLinks = ['HOME', 'ABOUT US', 'GALLERY', 'CONTACT'];
  const heroTitleMain = "SMART CITY";
  const heroTitleSub = "SOLUTIONS";
  const bulletPoints = ['Automation', 'Control Systems', 'IT'];
  
  // To use your actual image, put the file inside the "static" folder 
  // and change this path to "/your-image-name.png"
  const imagePath = "https://via.placeholder.com/600x400/0a1157/ffffff?text=Add+City+Image+Here"; 
  // ------------------------------
</script>

<div class="min-h-screen bg-[#06114F] text-white font-sans overflow-hidden">
  
  <nav class="flex justify-between items-center p-6 lg:px-12" in:fade={{ duration: 800 }}>
    <div class="border-2 border-white p-2 font-bold text-xl tracking-widest whitespace-pre-line leading-tight">
      {logoText}
    </div>
    
    <ul class="hidden md:flex space-x-8 font-semibold tracking-wide">
      {#each navLinks as link}
        <li class="hover:text-blue-300 cursor-pointer transition-colors duration-200">{link}</li>
      {/each}
    </ul>
  </nav>

  <main class="flex flex-col-reverse lg:flex-row items-center justify-between p-6 lg:px-12 lg:py-20 max-w-7xl mx-auto">

    <div class="w-full lg:w-3/5 mt-12 lg:mt-0" in:fly={{ x: -50, duration: 1000, delay: 200 }}>
      <img
        src={imagePath}
        alt="Smart City Solutions"
        class="w-full h-auto drop-shadow-2xl opacity-90 hover:opacity-100 transition-opacity duration-300"
      />
    </div>

    <div class="w-full lg:w-2/5 flex flex-col items-start lg:items-end text-right" in:fly={{ x: 50, duration: 1000, delay: 400 }}>
      
      <h1 class="text-5xl lg:text-7xl font-extrabold tracking-tight mb-2">
        {heroTitleMain}
      </h1>
      <h2 class="text-3xl lg:text-5xl font-bold tracking-widest mb-6">
        {heroTitleSub}
      </h2>

      <div class="text-lg lg:text-xl text-blue-200 mb-10 space-y-2 flex flex-col items-end">
        {#each bulletPoints as point}
          <p class="font-medium">{point}</p>
        {/each}
      </div>

      <div class="flex flex-col space-y-4 w-56">
        <button class="bg-white text-[#06114F] font-bold py-3 px-6 rounded-lg shadow-lg hover:bg-gray-200 transition-colors duration-200">
          GET STARTED
        </button>
        <button class="bg-white text-[#06114F] font-bold py-3 px-6 rounded-lg shadow-lg hover:bg-gray-200 transition-colors duration-200">
          D E M O
        </button>
      </div>

    </div>

  </main>
</div>
